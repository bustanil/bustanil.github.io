---
layout: post
title: Membangun aplikasi Java dengan Gradle
tags: java
---

[Gradle](http://www.gradle.org/) merupakan sebuah build tool dengan Groovy sebagai scripting language-nya. Bagi saya, menggunakan Gradle itu artinya kita menggunakan segala kebaikan dari Ant, Maven dan Groovy, yaitu fleksibilitas di sisi konfigurasi, layout dan logika dari build system dan convention over configuration. Walaupun dibangun dari tools yang identik dengan Java, tapi Gradle dapat digunakan untuk hal-hal lain. Saya akan coba sedikit share bagaimana saya menggunakan Gradle untuk pengembangan aplikasi Java. 

FYI, Hibernate juga baru saja menggantikan build toolnya dari [Maven ke Gradle](http://community.jboss.org/wiki/Gradlewhy).

#### Instalasi

Pastikan Groovy sudah terinstalasi dengan benar dengan mengikuti instruksi di [sini](http://groovy.codehaus.org/Tutorial+1+-+Getting+started).

Instalasinya cukup mudah, setelah kita download, daftarkan direktori (direktori_gradle)/bin ke environmental variable PATH.

Jika masih kesulitan, silahkan lihat instruksi instalasi lebih lengkapnya di [sini](http://gradle.org/installation.html).

#### "Hello Gradle"

Kita akan coba build satu program simple yang hanya menampilkan teks "Hello Gradle". Buka IDE favorit anda sekarang, buat satu project baru dengan nama HelloGradle.

Secara convention, Gradle akan mengasumsikan bahwa struktur direktori project kita mengikuti struktur direktori standar yang digunakan oleh Maven.

{% highlight bash %}
src
   /main
        /java
        /resources
   /test
        /java
        /resources
{% endhighlight %}

Direktori 'main' dan 'test' di atas adalah [source set](http://www.gradle.org/java_plugin.html#N11BAD) standar untuk pengembanga aplikasi Java.

Di dalam direktori src/main/java dan di dalam package gradle.example, kita buat class seperti di bawah ini.

{% highlight java %}
package gradle.example;
 
public class HelloGradle {
    public static void main(String[] args){
        System.out.print("Hello Gradle");
    }
}
{% endhighlight %}

Kode di atas adalah kode yang pastinya sangat membosankan bagi anda, tapi yang saya ingin tunjukkan adalah bagaimana kita build program di atas menggunakan Gradle.

Buat file build.gradle di root directory project anda yang isinya sebagai berikut.

{% highlight bash %}
apply plugin: 'java'
{% endhighlight %}

Baris di atas memberitahukan Gradle bahwa kita ingin menggunakan plugin dengan nama 'java'. Gradle terdiri atas banyak plugin yang dapat kita gunakan untuk berbagai kebutuhan. Namun untuk pengembangan aplikasi Java, paling tidak kita menggunakan plugin java. Dengan menggunakan plugin java maka kita dapat mengeksekusi task-task yang berkaitan dengan Java, misalnya task untuk compile source code.

Untuk mengcompile program di atas, kita cukup eksekusi perintah berikut di root directory project kita:

{% highlight bash %}
gradle compileJava
{% endhighlight %}

Untuk lebih lengkapnya mengenai plugin java bisa dilihat di [sini](http://www.gradle.org/java_plugin.html).