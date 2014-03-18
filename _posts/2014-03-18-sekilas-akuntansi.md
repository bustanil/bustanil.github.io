---
layout: post
title: Sekilas Akuntansi
tags: java akuntansi
---

Sebelum kita mulai membuat aplikasi, ada baiknya kita memahami terlebih dahulu proses bisnis yang ada, yaitu akuntansi itu sendiri. Pemahaman mendasar terhadap proses bisnis akan membantu kita dalam merancang [domain model](http://martinfowler.com/eaaCatalog/domainModel.html).

#### Persamaan Akuntansi

Akuntansi mendasarkan diri pada persamaan dasar akuntansi, yaitu:
{% highlight java %}
Aset = Utang + Ekuitas
{% endhighlight %}

Keseimbangan dari sisi kiri dengan sisi kanan harus dijaga. Ini adalah prinsip matematika paling mendasar dari akuntansi.

Persamaan di atas kemudian dikembangkan lebih lanjut menjadi persamaan ekstensi akuntansi, yaitu:

{% highlight java %}
Aset + Biaya + Pengembalian Ekuitas = Utang + Ekuitas + Pendapatan
{% endhighlight %}

Tampak memusingkan? Awalnya juga saya berpikir seperti itu. Teman saya bilang untuk mempermudah pemahaman kita bisa pikirkan bahwa akun-akun di sebelah kiri adalah sisi penggunaan dana dan akun-akun di sebelah kanan adalah sisi pemerolehan dana. Setiap dana yang digunakan (sisi kiri) akan ada pertanggungjawabannya di sisi kanan. Logis bukan?

Menurut aturan akuntansi, pertambahan di sisi kiri berarti debit sedangkan di sisi kanan berarti kredit dan juga sebaliknya.

<table class="table">
	<tr>
		<td></td>
		<th>Penggunaan dana</th>
		<th>Pemerolehan dana</th>
	</tr>
	<tr>
		<th>Bertambah</th>
		<td>Debit</td>
		<td>Kredit</td>
	</tr>
	<tr>
		<th>Berkurang</th>
		<td>Kredit</td>
		<td>Debit</td>
	</tr>
</table>

Dengan memakai aturan di atas, sebuah transaksi dapat pecah menjadi bagian-bagian di sisi kiri dan sisi kanan persamaan akuntansi.

#### Contoh Transaksi

Untuk memahami lebih lanjut mengenai persamaan akuntansi di atas, kita coba terapkan pada transaksi.

__Transaksi 1__: Pemilik perusahaan memberikan modal berupa uang tunai sebesar Rp. 5.000.000,-

Modal merupakan komponen ekuitas (sisi kanan), bertambah berarti kredit

Kas merupakan komponen aset (sisi kiri), bertambah berarti debit

<table class="table">
	<tr>
		<th>Akun</th>
		<th>Debit</th>
		<th>Kredit</th>
	</tr>
	<tr>
		<td>Modal</td>
		<td></td>
		<td>5.000.000</td>
	</tr>
	<tr>
		<td>Kas</td>
		<td>5.000.000</td>
		<td></td>
	</tr>
</table>

__Transaksi 2__: Perusahaan menerima pendapatan dari hasil penjualan secara tunai sebesar Rp. 300.000,-

Pendapatan merupakan komponen ekuitas (sisi kanan), bertambah berarti kredit

Kas merupakan komponen aset (sisi kiri), bertambah berarti debit

<table class="table">
	<tr>
		<th>Akun</th>
		<th>Debit</th>
		<th>Kredit</th>
	</tr>
	<tr>
		<td>Pendapatan</td>
		<td></td>
		<td>300.000</td>
	</tr>
	<tr>
		<td>Kas</td>
		<td>300.000</td>
		<td></td>
	</tr>
</table>
