---
layout: post
title: Belajar Python - Data Structure
tags: python
---

#### List dan Dictionary

Dua class collection di Java yang paling sering digunakan adalah `java.util.List` dan `java.util.Map`. Python memiliki tipe data yang sepadan yaitu `List` dan `Dictionary`

##### List

List merupakan tipe data yang digunakan untuk menampung banyak nilai seperti sebuah array yang dinamis ukurannya. Nilai-nilai di dalam `list` boleh berbeda tipe (di Java ini sama dengan sebuah raw `java.util.List`)

Mengakses isi elemen sebuah list layaknya sebuah array di Java menggunakan square-bracket, contoh:

{% highlight python %}
a = [1, 2, 3]
b = a[1]          # b bernilai 2
{% endhighlight %}

Menambah dan menghapus elemen menggunakan `append()` dan `remove()`, contoh:

{% highlight python %}
a = []
a.append('orange')
a.append('blue')
a.append('red')
a.append('brown')

a.remove('red')        # a = ['orange', 'blue', 'brown']
{% endhighlight %}

Yang menarik adalah kita bisa menggunakan operator aritmetika untuk "menjumlahkan" atau "mengalikan" sebuah objek `list`.

{% highlight python %}
a = [1, 2, 3, 4, 5]
b = [6, 7]
c= a + b               # c = [1, 2, 3, 4, 5, 6, 7]

d = b * 3              # d = [6, 7, 6, 7, 6, 7]

{% endhighlight %}

`list` juga memiliki method `index()` yang digunakan untuk mencari index dari elemen tertentu.

{% highlight python %}
a = ['Hello', 2, 3, 3.4, 'Test']

i = a.index('Hello')           # i = 0

a.index('Tidak ada')           # menghasilkan runtime error

{% endhighlight %}

Untuk memeriksa keberadaan nilai di dalam sebuah `list`, gunakan method `in()`.

##### Dictionary

Dictionary merupakan tipe data yang digunakan untuk menyimpan banyak nilai dalam bentuk pasangan key - value.

Bila di `List` index pasti merupakan bilangan bulat, maka pada tipe `Dictionary` index ini dapat berupa objek apapun, misalnya string.

{% highlight python %}
capitals =  {'indonesia' : 'jakarta', 'thailand' : 'bangkok', 'malaysia' : 'kuala lumpur'}

# mengakses nilai berdasarkan key
my_capital = capitals['indonesia']       # my_capital = 'jakarta'

# menambahkan nilai
capitals['phillipine'] = 'manila'

# mengubah nilai
capitals['indonesia'] = 'jonggol'
{% endhighlight %}