---
layout: post
title: Meningkatkan Performa Aplikasi dengan Hibernate Cache
tags: java hibernate caching
---

Banyak cara untuk meningkatkan performa aplikasi yang kita buat menggunakan framework Hibernate. Salah satunya adalah menggunakan fasilitas second-level cache. Data-data yang tidak sering berubah seperti data-data referensi atau master sangatlah cocok untuk ditempatkan di cache. Dengan begitu beban database dapat dikurangi secara signifikan.

Kita tentu saja dapat membuat sendiri mekanisme caching objek di aplikasi kita. Namun hal tersebut mengharuskan kita untuk memelihara objek-objek yang ada di cachenya. Terutama saat objek tersebut dihapus atau diupdate. Selain itu source code kita juga akan dipenuhi oleh code-code yang digunakan untuk mengambil data dari cache tersebut.

Bila kita menggunakan Hibernate Cache maka hal di atas tidak perlu kita lakukan karena proses caching di Hibernate sangatlah transparan. Cukup tambahkan satu konfigurasi di hibernate.cfg.xml dan annotation di setiap class yang akan kita cache maka fasilitas caching sudah dapat dimanfaatkan.

Di Hibernate terdapat dua level cache yaitu first level cache dan second level cache.

- First level cache terletak pada objek `Session` itu sendiri. Bila kita `read()` dua kali atau lebih dengan id sama, maka Hibernate hanya meng-query satu kali saja ke database. Objek hasil query kemudian disimpan di memori. Objek inilah yang akan diberikan untuk pemanggilan read selanjutnya. Objek tersebut hidup di dalam cache selama session tersebut aktif.

- Second level cache digunakan saat kita ingin mengcache objek-objek lintas `Session`. Artinya masa hidup objek tersebut tidak lagi bergantung pada siklus hidup Session tapi diatur oleh mekanisme lain.

Untuk lebih memahami kedua jenis cache kita ambil contoh sebuah class `Category`.

{% highlight java %}
package com.codequ.hibernate.model;

import org.hibernate.annotations.*;

import javax.persistence.*;
import javax.persistence.Entity;
import javax.persistence.Table;

public class Category {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    @Column(name = "name")
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

{% endhighlight %}

Class `Category` ini merupakan sebuah class persistence sederhana yang dipetakan pada sebuah tabel `category` dengan kolom `id` dan `name`.

## First Level Cache

Untuk menunjukkan fungsi first-level cache maka kita baca sebuah objek `Category` dengan id `1` sebanyak lima kali. Hal tersebut kita lakukan di program berikut:

{% highlight java %}
package com.codequ;

import com.codequ.hibernate.model.Category;
import com.codequ.hibernate.util.HibernateUtil;
import org.hibernate.Session;

public class FirstLevelCacheDemo
{
    public static void main( String[] args ) {
        Session session = HibernateUtil.openSession();
        Category sepatu;
        for(int i = 0; i < 100; i++){
            sepatu = (Category) session.get(Category.class, 1L);
            System.out.println(sepatu.getName());
        }
        session.close();
    }
}
{% endhighlight %}

Bila program di atas dijalankan maka berikut ini adalah outputnya:

	Hibernate: select category0_.id as id0_0_, category0_.name as name0_0_ from category category0_ where category0_.id=?
	Sepatu
	Sepatu
	Sepatu
	Sepatu
	Sepatu

Dari output di atas terbukti bahwa dengan adanya first-level cache, maka kita dapat mengurangi beban database sebesar 80% sehingga data dapat dibaca dengan lebih cepat. Masih berani bilang kalau Hibernate itu lambat?

## Second-level Cache

Berbeda dengan first-level cache yang sepenuhnya dikelola oleh Hibernate, pada second-level cache Hibernate berkolaborasi dengan *cache provider*. Cache provider merupakan library/framework yang berfungsi untuk meng-cache objek. Masing-masing cache provider memiliki cara masing-masing dalam menerapkan fungsi caching. Implementasi caching tidak melulu di memori, beberapa provider menyediakan fasilitas untuk meng-cache objek ke disk. Berikut ini adalah cache provider yang populer di kalangan programmer Java:

1. ehcache
2. OSCache
3. Memcached

Pada artikel ini kita akan menggunakan "ehcache" sebagai cache provider. Untuk itu kita tambahkan dua property yaitu `hibernate.cache.use_second_level_cache` dan `hibernate.cache.region.factory_class` di konfigurasi session-factory kita di dalam file `hibernate.cfg.xml`.

	<?xml version="1.0" encoding="utf-8"?>
	<!DOCTYPE hibernate-configuration PUBLIC
	        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	        "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
	<hibernate-configuration>
	    <session-factory>
	        <property name="hibernate.cache.use_second_level_cache">true</property>
	        <property name="hibernate.cache.region.factory_class">net.sf.ehcache.hibernate.EhCacheRegionFactory</property>
	        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
	        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/shop</property>
	        <property name="hibernate.connection.username">root</property>
	        <property name="hibernate.connection.password"></property>
	        <property name="hibernate.dialect">org.hibernate.dialect.MySQL5Dialect</property>
	        <property name="show_sql">true</property>
	        <mapping class="com.codequ.hibernate.model.Category" />
	        <mapping class="com.codequ.hibernate.model.Product" />
	    </session-factory>
	</hibernate-configuration>

Setelah itu kita mesti memberi tahu Hibernate class mana saja yang akan dicache di second-level cache dengan menggunakan annotation yang sudah disediakan.

{% highlight java %}
package com.codequ.hibernate.model;

import org.hibernate.annotations.*;

import javax.persistence.*;
import javax.persistence.Entity;
import javax.persistence.Table;

@Entity
@Table(name = "category")
@org.hibernate.annotations.Cache(usage=CacheConcurrencyStrategy.READ_ONLY, region="category")
public class Category {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "id")
    private Long id;

    @Column(name = "name")
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
{% endhighlight %}

Selanjutnya untuk membuktikan bahwa objeknya tetap dicache walaupun diakses dari dua atau lebih `Session` yang berbeda, maka kita buat sebuah class Data Access Object untuk class Category di atas.

{% highlight java %}
package com.codequ.hibernate.dao;


import com.codequ.hibernate.model.Category;
import com.codequ.hibernate.util.HibernateUtil;
import org.hibernate.Session;

public class CategoryDao {

    public Category read(Long id) {
        Session session = HibernateUtil.openSession();
        Category category = (Category) session.get(Category.class, id);
        session.close();
        return category;
    }

}

{% endhighlight %}

Selanjutnya kita panggil method `read()` di class DAO tersebut di sebuah program Java.

{% highlight java %}
package com.codequ;

import com.codequ.hibernate.dao.CategoryDao;
import com.codequ.hibernate.model.Category;

public class SecondLevelCacheDemo {

    public static void main(String[] args) {
        CategoryDao dao = new CategoryDao();
        for (int i = 0; i < 5; i++) {
            Category sepatu = dao.read(1L);
            System.out.println(sepatu.getName());
        }
    }

}

{% endhighlight %}

Output dari program di atas adalah sebagai berikut:

	Hibernate: select category0_.id as id0_0_, category0_.name as name0_0_ from category category0_ where category0_.id=?
	Sepatu
	Sepatu
	Sepatu
	Sepatu
	Sepatu

Hasilnya sama dengan contoh program first-level cache. Perbedaan dengan first-level cache adalah setiap kali `dao.read(1L)` dipanggil, objek category "Sepatu" dibaca dari objek session yang berbeda, namun karena kita sudah mengaktifkan second-level cache untuk class `Category` maka objek yang sama yang akan diberikan di setiap session.

Bila kita non-aktifkan konfigurasi Cache pada class `Category` lalu menjalankan ulang program di atas, berikut ini adalah outputnya:

	Hibernate: select category0_.id as id0_0_, category0_.name as name0_0_ from category category0_ where category0_.id=?
	Sepatu
	Hibernate: select category0_.id as id0_0_, category0_.name as name0_0_ from category category0_ where category0_.id=?
	Sepatu
	Hibernate: select category0_.id as id0_0_, category0_.name as name0_0_ from category category0_ where category0_.id=?
	Sepatu
	Hibernate: select category0_.id as id0_0_, category0_.name as name0_0_ from category category0_ where category0_.id=?
	Sepatu
	Hibernate: select category0_.id as id0_0_, category0_.name as name0_0_ from category category0_ where category0_.id=?
	Sepatu

Terlihat bahwa tanpa second-level cache, program di atas akan mengeksekusi lima kali query ke database.

Source code: [https://github.com/bustanil/hibernate-caching](https://github.com/bustanil/hibernate-caching)