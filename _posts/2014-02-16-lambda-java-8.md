---
layout: post
title: Functional Programming di Java 8 dengan Lambda
tags: java
---

Lambda adalah fitur Java 8 yang paling ditunggu-tunggu. Setelah batal diimplementasikan di Java 7, akhirnya fitur ini akan hadir di Java 8.
Bagi anda yang familiar dengan bahasa pemrograman seperti Ruby, Scala, Groovy tentunya fitur ini bukanlah hal yang baru (dengan nama yang berbeda seperti block, closure, function). Dengan lambda sebuah fungsi dapat diperlakukan sebagai objek yang berdiri sendiri sehingga bisa di-assign ke sebuah variabel, di-return dari sebuah method atau di-passing sebagai parameter ke fungsi yang lain. Istilah kerennya [first-class function](http://en.wikipedia.org/wiki/First-class_function).

Di saat tulisan ini ditulis Java 8 masih ada di tahap early access dan direncanakan akan dirilis 18 Maret 2014. Download versi terakhir Java 8 dengan dukungan lambda di [sini](https://jdk8.java.net/lambda/).

Penggunaan lambda sangat berkaitan dengan functional interface, yaitu interface yang hanya mempunyai satu method seperti [Comparator](http://docs.oracle.com/javase/7/docs/api/java/util/Comparator.html), [Runnable](http://docs.oracle.com/javase/7/docs/api/java/lang/Runnable.html) atau [ActionListener](http://docs.oracle.com/javase/7/docs/api/java/awt/event/ActionListener.html). Functional interface ini digunakan untuk menerapkan konsep functional programming di Java. Sebenarnya programmer Java sudah tidak asing dengan interface jenis ini, hanya saja di Java 8 functional interface lebih ditegaskan lagi karena kehadiran fitur lambda. Functional Interface akan saya bahas di posting selanjutnya.

Untuk kesempatan kali ini kita akan lihat bagaimana implementasi Comparator di Java 8 menggunakan lambda. Tapi sebelumnya kita harus punya sesuatu untuk dibandingkan. Untuk itu kita punya class Person dengan definisi seperti di bawah ini:

{% highlight java linenos %}
public class Person
{
	private String firstName;
	private String lastName;

	public Person(String firstName, String lastName)
	{
		this.firstName = firstName;
		this.lastName = lastName;
	}

	public String getFirstName()
	{
		return this.firstName;
	}

	public String getLastName()
	{
		return this.lastName;
	}

	public String getFullName()
	{
		return this.firstName + " " + this.lastName;
	}
}
{% endhighlight %}

Misalkan kita ingin mengurutkan objek-objek yang ada di dalam sebuah [List](http://docs.oracle.com/javase/7/docs/api/java/util/List.html) berdasarkan nama belakang secara descending. Di Java 7, kita bisa melakukannya seperti berikut:

{% highlight java linenos %}
List<Person> persons = Arrays.asList(
	new Person("Bustanil", "Arifin"),
	new Person("Hesty", "Ernawati"),
	new Person("Joko", "Widodo")
);

Comparator<Person> sortByLastNameDesc = new Comparator<Person>()
{

	public int compare(Person p1, Person p2)
	{
		return p2.getLastName().compareTo(p1.getLastName());
	}

}

Collections.sort(persons, sortByLastNameDesc);
{% endhighlight %}

Di Java 8, kita bisa sederhanakan definisi Comparator di atas dengan menggunakan lambda:

{% highlight java linenos %}
Comparator<Person> sortByLastNameDesc = (p1, p2) -> p2.getLastName().compareTo(p1.getLastName());

Collections.sort(persons, sortByLastNameDesc);
{% endhighlight %}

Atau kita bisa buat menjadi inline seperti berikut:

{% highlight java %}
Collections.sort(persons, (p1, p2) -> p2.getLastName().compareTo(p1.getLastName()));
{% endhighlight %}

Dengan menggunakan lambda, kita bisa mengurangi jumlah baris kode secara signifikan dan juga meningkatkan readability karena kita lagi harus baca 5 - 10 baris hanya untuk mengetahui bagaimana functional interfacenya diimplementasikan. 

Kapan kita gunakan lambda? Lambda akan sangat bermanfaat saat kita bermaksud untuk membungkus sebuah perilaku yang tidak membutuhkan konstruktor, field dan fungsi tambahan. Bila perilaku yang akan diimplementasikan masih membutuhkan konstruktor, field atau fungsi tambahan maka kita tetap harus menggunakan cara lama misalnya dengan menggunakan anonymous inner class.

Contoh kode bisa diunduh di [https://github.com/bustanil/java8-lambda](https://github.com/bustanil/java8-lambda).
