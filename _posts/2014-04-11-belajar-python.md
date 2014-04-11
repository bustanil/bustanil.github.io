---
layout: post
title: Belajar Python
tags: python
---

Saya sudah pelajari banyak bahasa pemrograman, mulai dari Pascal sampai dengan Scala. Tapi ada satu bahasa yang sama sekali belum pernah saya pelajari, Python. Salah satu alasannya adalah Python menggunakan indentasi untuk menentukan scoping yang saya pikir sebagai fitur aneh untuk sebuah bahasa pemrograman.

Setelah saya menyerah mempelajari Scala yang ternyata memiliki (terlalu) banyak cara untuk melakukan sesuatu. Saya berpikir apakah ada bahasa pemrograman yang hanya punya satu cara untuk melakukan sesuatu. Saya pun ingat dengan Python. Filosofi Python adalah hanya menyediakan [satu cara terbaik untuk melakukan sesuatu](http://en.wikipedia.org/wiki/The_Zen_of_Python#Programming_philosophy). Saya pikir, bahasa ini akan sangat baik untuk diterapkan dalam sebuah tim. Siapapun programmernya pasti sintaks yang digunakan untuk _looping_ sebuah list adalah sama. Artinya akan lebih mudah mempelajari code orang lain dan akan memudahkan praktek [collective code ownership](http://www.extremeprogramming.org/rules/collective.html).

#### Python 2 atau 3?

Saya pun segera buka website Python untuk mengunduh _installer_-nya. Versi terbaru saat tulisan ini dibuat adalah 3.4.0, tapi dari yang saya [baca](https://wiki.python.org/moin/Python2orPython3/), belum banyak library yang mendukung versi ini. Jadi saya pun cari aman dan menginstal versi 2.7.0.

Karena tidak mau repot, saya install versi Windows Installer-nya. Setelah selesai saya cek apakah Python sudah terinstall dengan baik dengan menjalankan command `python --version`.

#### "Hello World"

Untuk langkah pertama, mari mulai dengan membuat program yang mencetak teks "Hello World" 10 kali.


{% highlight python %}
# helloworld.py

for i in range(0, 10):
	print 'Hello World'

{% endhighlight %}

Jalankan dengan perintah `python helloworld.py`, hasilnya:

{% highlight bash %}
C:\workspace\learnpython>python helloworld.py
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
Hello World
{% endhighlight %}

#### Tebak Angka

Untuk mempelajari beberapa sintaks dasar sebuah bahasa (definisi variabel, perulangan, percabangan, input, output), saya biasanya membuat program tebak angka. Program akan men-generate satu angka acak dari 1 sampai 100, kemudian program akan menerima input (tebakan) dari user. Bila tebakan sama dengan angka acak yang ditentukan maka program selesai. Bila masih meleset, maka program akan memberitahu user apakah tebakannya lebih besar atau lebih kecil dari angka acaknya.

{% highlight python linenos %}
#guessthenumber.py

import random
counter = 0
number = random.randint(1, 100)
while True:
	guess = input("Tebakan anda: ")
	counter = counter + 1
	if guess == number:
		print 'Anda menang. Jumlah tebakan: ', counter
		break
	elif guess > number:
		print 'Terlalu besar'
	else:
		print 'Terlalu kecil'
{% endhighlight %}

**Penjelasan:**

- Baris 3: Untuk men-generate angka acak kita gunakan modul `random`
- Baris 5: Kita gunakan fungsi `randint(from, to)` dari objek `random` untuk menghasilkan angka acak 1 - 100
- Baris 9: variable `guess` sebenarnya bertipe string
- Baris 12: `elif` merupakan singkatan dari `else if`. Sebenarnya menggunakan `else if` tidak masalah, tapi mengapa tidak kita menghemat beberapa karakter :)

Source code: [https://github.com/bustanil/learnpython](https://github.com/bustanil/learnpython)