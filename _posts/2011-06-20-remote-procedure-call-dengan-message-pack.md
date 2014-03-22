---
layout: post
title: Remote Procedure Call dengan MessagePack!
tags: java
---

Remote Procedure Call di Java sudah lama ada, siapa yang tidak kenal RMI dengan segala kelebihan dan kekurangannya. Kini MessagePack hadir sebagai alternatif solusi RPC yang diklaim lebih cepat, simple dan [cross-language](http://stackoverflow.com/questions/25865/what-is-a-language-binding).

MessagePack terdiri atas dua modul terpisah, yaitu MessagePack Serialization dan MessagePack RPC.

MessagePack Serialization merupakan library untuk [serialisasi/deserialisasi](http://en.wikipedia.org/wiki/Serialization) object yang cross-language (kompatibel antar bahasa pemrograman) seperti JSON namun dengan format binary dan ukuran data yang **lebih kecil** sedemikian sehingga lebih cepat. Sedangkan MessagePack RPC merupakan library RPC untuk komunikasi client-server yang juga cross-language. MessagePack RPC menggunakan MessagePack Serialization untuk pertukaran data antara client dan server.

Sebelum ada [spring-remoting](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/remoting.html), mengekspos satu service sederhana menggunakan RMI cukup menguras waktu dan pikiran. Namun saat menggunakan MessagePack, semuanya terasa lebih mudah karena tidak ada file konfigurasi yang wajib disetup. Saya sendiri menghabiskan waktu kurang dari 15 menit untuk implementasi satu kasus sederhana.

Sebelum menggunakan MessagePack, kita mesti include library-nya ke classpath. Karena saya menggunakan [gradle](/2011/06/20-remote-procedure-call-dengan-message-pack.html), saya cukup definisikan satu repository yaitu repository dari MessagePack dan satu dependency di file build.gradle saya.

{% highlight groovy %}
apply plugin: 'java'
 
dependencies {
  compile group: 'org.msgpack', name: 'msgpack-rpc', version: '0.6.1-devel'
}
 
repositories {
  mavenCentral()
  mavenRepo urls: ["http://msgpack.org/maven2", "http://repository.jboss.org/nexus/content/groups/public-jboss"]
}
{% endhighlight %}

#### Sample case

Misal kita punya satu aplikasi Restoran dimana waiter dapat mengirim order dari meja customer. Data yang dikirim yaitu order id, customer name, order detail, dan status order agar waiter/waitress dapat mengetahui apakah ordernya sudah diproses server.

Berikut adalah class modelnya:

{% highlight java linenos %}
package org.inharmonia.msgpack.sample;
 
import org.msgpack.annotation.MessagePackMessage;
 
@MessagePackMessage
public class Order {
 
    public long id;
    public String detail;
    public boolean processed;
 
}
{% endhighlight %}

Class Order kita beri annotation @MessagePackMessage. Ini menandakan bahwa objek dengan tipe ini agar diserialisasi menggunakan MessagePack Serialization saat akan diserialisasi melalui network.

Di bawah ini adalah implementasi service yang akan digunakan untuk memproses order yang datang (mencetak isi dari objek order kemudian mengubah property processed-nya menjadi true)

{% highlight java linenos %}
package org.inharmonia.msgpack.sample;
 
public interface IOrderProcessor {
 
  Order processOrder(Order order);
 
}

package org.inharmonia.msgpack.sample;
 
import static java.text.MessageFormat.format;
 
public class OrderProcessor implements IOrderProcessor {
 
    @Override
    public Order processOrder(Order order){
        System.out.println(format("Processing order [id: {0}, detail: {1}]" , order.id, order.detail));
        order.processed = true;
        return order;
    }
 
}
{% endhighlight %}

Sekarang kita buat satu class yang bertindak sebagai aplikasi servernya. Pertama kita buat satu objek Server yang menerima satu objek EventLoop. Kemudian kita passing objek OrderProcessor ke objek server tadi untuk menandakan bahwa semua public method yang ada di dalam objek OrderProcessor dapat dipanggil secara remote.

{% highlight java linenos %}
package org.inharmonia.msgpack.sample;
 
import org.msgpack.rpc.Server;
import org.msgpack.rpc.loop.EventLoop;
 
public class ServerApp {
 
    public static void main(String[] args) throws Exception {
        EventLoop loop = EventLoop.defaultEventLoop();
 
        Server svr = new Server(loop);
        svr.serve(new OrderProcessor());
        svr.listen("localhost", 1985);
 
        loop.join();
    }
}
{% endhighlight %}

Aplikasi client-nya pun cukup sederhana, cukup buat satu objek Client yang menerima satu objek EventLoop dan hostname dan port dari aplikasi server-nya. Kemudian kita buat satu proxy menggunakan interface dari service yang akan dipanggil. Sisanya mestinya dapat ditebak, kita coba instansiasi satu objek Order kemudian mengirimkannya ke server lalu kita cetak ke console property processed dari objek order tersebut untuk mengetahui status dari order kita.

{% highlight java linenos %}
package org.inharmonia.msgpack.sample;
 
import org.msgpack.rpc.Client;
import org.msgpack.rpc.loop.EventLoop;
 
public class ClientApp {
 
    public static void main(String[] args) throws Exception {
        EventLoop loop = EventLoop.defaultEventLoop();
 
        Client cli = new Client("localhost", 1985, loop);
        IOrderProcessor iface = cli.proxy(IOrderProcessor.class);
 
        Order order = new Order();
        order.id = 99L;
        order.detail = "Nasi Goreng Seafood";
        order.processed = false;
 
        order = iface.processOrder(order);
 
        System.out.println("Has order been processed? " + order.processed);
 
        cli.close();
        loop.shutdown();
    }
}
{% endhighlight %}