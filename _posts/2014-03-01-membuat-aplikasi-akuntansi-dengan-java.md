---
layout: post
title: Membuat aplikasi akuntansi sederhana dengan Java
tags: java wicket accounting
---

Sewaktu saya masih SMA dan kuliah, pelajaran akuntansi selalu membuat saya ngeri. Membuat jurnal, membuat neraca, membuat laporan laba rugi selalu membuat saya bingung. Akuntansi bagi saya waktu itu sangatlah tidak masuk akal. Bukankah kredit itu artinya bertambah, lantas kenapa kalau perusahaan dapat uang, kasnya malah didebit? Membingungkan!

Setiap ujian pasti selalu ada yang tidak _balance_, makanya saya terkadang pakai sedikit _hack_ dengan menambahkan entri fiktif. Sayang guru yang memeriksa ujiannya sangat teliti dan akhirnya hasil ujian saya tidak begitu menyenangkan. Semenjak saat itulah, saya bisa dibilang anti dengan yang namanya akuntansi.  

Setelah saya bekerja, barulah saya sadar bahwa sebuah bisnis sangat bergantung pada akuntansi untuk pencatatan transaksi dan membuat laporan keuangan. Mau tidak mau, sebagai _enterprise software developer_, kita harus belajar untuk mengerti dasar-dasar akuntansi. Saya pun berdiskusi dengan teman-teman sesama _software developer_ dan menurut mereka akuntansi tidak sulit sama sekali setelah kita memahami persamaan akuntansi.

Semangat pun timbul untuk belajar akuntansi kembali. Saya segera bergegas ke toko buku untuk mencari sebuah buku yang menjelaskan akuntansi untuk pemula. Dan saya pun menemukan sebuah buku dengan judul yang sangat menarik, yaitu **"Akuntansi itu ternyata Logis dan Mudah"**. Berikut ini covernya:

![Akuntansi itu ternyata Logis dan Mudah](https://dl.dropboxusercontent.com/u/7031801/WP_000471.jpg)

Singkat cerita saya sudah baca bukunya sampai lecek, tapi sebagai seorang programmer saya pun punya ide untuk membuat sebuah aplikasi akuntansi sederhana berbasis web dengan Java. Saya akan coba pendekatan [Test Driven Development](http://en.wikipedia.org/wiki/Test-driven_development) dalam membuat aplikasi ini.

Artikel ini akan dibagi atas lima bagian:
1. Persiapan project
2. Membuat domain model
3. Menambahkan logic
4. Membuat laporan dengan Jasper Report
5. Membuat tampilan web

[Lanjut ke setup project]()