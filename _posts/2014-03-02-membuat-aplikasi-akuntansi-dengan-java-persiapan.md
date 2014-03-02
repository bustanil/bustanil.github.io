---
layout: post
title: Membuat aplikasi akuntansi sederhana dengan Java (1) - Persiapan project
tags: java wicket accounting
---

#### Arsitektur aplikasi

Secara arsitektur, aplikasi yang akan dibangun akan menggunakan [_multitier architecture_](http://en.wikipedia.org/wiki/Multitier_architecture). Dengan arsitektur ini, fungsi dari aplikasi akan dibagi menjadi beberapa tier, yaitu:

1. Presentation tier: Tier teratas dari aplikasi yang bertugas untuk menampilkan informasi kepada user dan juga menerima input dari user. Karena aplikasi yang akan dibuat adalah berbasis web, maka presentation tier dari aplikasi ini adalah web page.

2. Application tier: Merupakan tier yang berisi fungsi-fungsi yang berkaitan dengan proses bisnis baik itu berupa pengambilan keputusan, kalkulasi, akses data dan lain-lain.

3. Data tier: Merupakan tier paling bawah yang bertanggung jawab untuk proses membaca dan menulis data ke database.

#### Framework yang digunakan

1. Apache Wicket digunakan sebagai web framework

2. Spring digunakan sebagai application container

3. Hibernate digunakan untuk menyediakan fungsi-fungsi yang berurusan dengan database

4. JUnit digunakan untuk melakukan unit testing pada aplikasi


#### Tools lainnya

1. Maven akan bertindak sebagai build tool. Mempersiapkan project Java tidak lagi membutuhkan waktu lama karena di Maven sudah tersedia project template dan library dependency management. Kita tidak perlu lagi repot-repot buat build file dari awal dan juga tidak perlu download banyak library. Maven akan melakukan semua itu untuk kita.

2. IDE yang digunakan adalah Intellij IDEA.

#### Setup project

Saya biasa membuat project baru menggunakan Maven. Kebetulan ada archetype yang biasa saya gunakan untuk membuat aplikasi yang menggunakan Apache Wicket, yaitu: wicket-archetype-quickstart

Di dalam direktori workspace saya, saya jalankan perintah berikut di terminal:

{% highlight bash %}
mvn archetype:generate -DarchetypeGroupId=org.apache.wicket -DarchetypeArtifactId=wicket-archetype-quickstart -DarchetypeVersion=6.14.0 -DgroupId=com.bustanil -DartifactId=easyaccounting -DarchetypeRepository=https://repository.apache.org/ -DinteractiveMode=false
{% endhighlight %}

Saya bisa langsung jalankan aplikasi bawaan dari archetypenya dengan menjalankan perintah:

{% highlight bash %}
mvn jetty:run
{% endhighlight %}

Perintah di atas akan menyebabkan aplikasinya dideploy ke sebuah server Jetty. Tapi sebelum itu, Maven akan mendownload dulu library-library yang dibutuhkan aplikasi terlebih dahulu. Setelah Jetty-nya up

{% highlight bash %}
********************************************************************
*** WARNING: Wicket is running in DEVELOPMENT mode.              ***
***                               ^^^^^^^^^^^                    ***
*** Do NOT deploy to your live server(s) without changing this.  ***
*** See Application#getConfigurationType() for more information. ***
********************************************************************
INFO  - WebApplication             - [wicket.easyaccounting] Started Wicket version 6.14.0 in DEVELOPMENT mode
2014-03-02 18:35:33.947:WARN:oejsh.RequestLogHandler:!RequestLog
2014-03-02 18:35:34.009:INFO:oejs.AbstractConnector:Started SelectChannelConnector@0.0.0.0:8080
2014-03-02 18:35:34.206:INFO:oejus.SslContextFactory:Enabled Protocols [SSLv2Hello, SSLv3, TLSv1] of [SSLv2Hello, SSLv3, TLSv1]
2014-03-02 18:35:34.208:INFO:oejs.AbstractConnector:Started SslSocketConnector@0.0.0.0:8443
[INFO] Started Jetty Server
{% endhighlight %}

Kita bisa langsung akses http://localhost:8080

Bila semuanya berjalan lancar, page berikut ini akan tampil.

![Wicket quickstart](https://dl.dropboxusercontent.com/u/7031801/wicket.png)

