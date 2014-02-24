---
layout: post
title: Cara Cepat Membuat Project Java dengan Maven
tags: java,maven
---

[Maven](http://maven.apache.org/) sudah lama dikenal oleh programmer Java untuk mengelola dependency atas library-library yang digunakan oleh sebuah aplikasi. Namun sebenarnya ada satu fitur Maven yang juga sangat bermanfaat terutama saat kita hendak membuat project Java baru yaitu fitur Archetype.

Archetype sendiri adalah bahasa kerennya dari project template. Jadi dengan Maven Archetype kita bisa membuat project dengan cepat sesuai dengan template yang kita pilih. Mau buat aplikasi Java standar? web dengan Servlet dan JSP? web dengan Spring MVC dan Hibernate? Kita tinggal pilih saja.

Misal untuk membuat project Java standar, kita cukup jalankan perintah berikut:

{% highlight bash %}
mvn archetype:generate -DgroupId=com.bustanil -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
{% endhighlight %}

- **groupId** merupakan nama group dari aplikasi kita, akan menjadi nama package aplikasi kita
- **artifactId** adalah nama dari aplikasi kita
- **archetypeArtifactId** adalah nama template yang akan kita gunakan
- **interactiveMode** adalah menentukan apakah kita butuh Maven memandu kita saat membuat project baru atau tidak (default true)

Setelah perintahnya selesai dijalankan akan ada direktori baru dengan nama sesuai dengan nama artifactId yang telah kita set sebelumnya, dalam kasus ini adalah helloworld. Di dalam direktori tersebut Maven sudah buatkan struktur direktori sesuai standar Maven beserta file pom.xml-nya. Selanjutnya untuk mulai bekerja, kita bisa buka file pom.xml tersebut menggunakan IDE favorit kita.

Beberapa archetype yang sering digunakan adalah sebagai berikut:
- **maven-archetype-quickstart** - untuk membuat sebuah project Java dengan satu main class
- **maven-archetype-webapp** - untuk membuat sebuah project web Java dengan Servlet dan JSP
- **spring-mvc-quickstart** - untuk membuat sebuah project web yang menggunakan Spring MVC, JPA 2, Twitter Bootstrap, MongoDB, JUnit/Mocikto dan Spring Security (archetype ini membutuhkan sebuah katalog custom, silahkan ikuti instruksi detilnya di [sini](https://github.com/kolorobot/spring-mvc-quickstart-archetype))