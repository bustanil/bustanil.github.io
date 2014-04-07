---
layout: post
title: Membuat aplikasi akuntansi sederhana dengan Java (3) - Domain Model
tags: java akuntansi
---

Setelah memahami teori dasar akuntansi, kita bisa mulai merancang domain model. Domain model merupakan model konseptual atas objek-objek yang terlibat di dalam sebuah problem domain.

Banyak cara yang dapat digunakan untuk menentukan domain model, misalnya dengan menuliskan proses bisnis yang terjadi lalu mengidentifikasi subjek dan objek dari setiap kejadian bisnis. Dalam kasus ini, proses bisnis yang terjadi adalah sebagai berikut:

- Satu **jenis transaksi** melibatkan dua **akun**
- **Akun-akun** yang terlibat dan posisi debit kreditnya **dikonfigurasikan** berdasarkan **jenis transaksi**nya
- Setiap **transaksi** akan dicatat dalam sebuat **jurnal**
- Pada setiap **entri jurnal** jumlah debit dan kredit haruslah seimbang
- Selain **akun-akun** yang terlibat, **jurnal** juga mencatat tanggal posting

Berikut ini adalah domain modelnya:

![](https://dl.dropboxusercontent.com/u/7031801/diagram.png)

Class **Account** merepresentasikan sebuah akun. Sebuah akun memiliki atribut kode, nama dan tipe. Misalnya akun Kas memiliki kode 'ASSET01' dengan nama 'Kas' dan tipe 'Asset'. 

{% highlight java %}
package com.bustanil.easyaccounting.model;

public class Account {

    private String code;
    private String name;
    private AccountType type;

    // setter dan getter tidak ditampilkan

}
{% endhighlight %}

Class **TransactionType** merepresentasikan jenis transaksi yang mungkin terjadi di sistem, misalnya transaksi 'Penjualan Tunai'. Setiap jenis transaksi melibatkan dua akun. Konfigurasi akun-akun yang terlibat didefinisikan pada atribut involvedAccounts.

{% highlight java %}
package com.bustanil.easyaccounting.model;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class TransactionType {

    private List<AccountConfig> involvedAccounts;

    // setter dan getter tidak ditampilkan

}
{% endhighlight %}

Class **Transaction** merupakan kejadian bisnis yang memperngaruhi akun-akun tertentu.

{% highlight java %}
package com.bustanil.easyaccounting.model;

import java.math.BigDecimal;
import java.util.Date;

public class Transaction {

    private Date transactionDate;
    private String description;
    private TransactionType transactionType;
    private BigDecimal amount;

   	// setter dan getter tidak ditampilkan

}
{% endhighlight %}

Class **AccountConfig** merupakan class yang digunakan untuk mendefinisikan posisi sebuah akun di dalam sebuah jenis transaksi.

{% highlight java %}
package com.bustanil.easyaccounting.model;

public class AccountConfig {

    private Account account;
    private DebitCredit drCr;

    // setter dan getter tidak ditampilkan
}
{% endhighlight %}

Class **Journal** digunakan untuk merekam satu transaksi.

{% highlight java %}
package com.bustanil.easyaccounting.model;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

public class Journal {

    private Date postingDate;
    private Date transactionDate;
    private String referenceNo;
    private List<JournalEntry> journalEntries;

    // setter dan getter tidak ditampilkan
}
{% endhighlight %}

Class **JournalEntry** berisi detil dari rekaman transaksi.

{% highlight java %}
package com.bustanil.easyaccounting.model;

import java.math.BigDecimal;

public class JournalEntry {

    private Account account;
    private DebitCredit drCr;
    private BigDecimal amount;

{% endhighlight %}

Enum **DebitCredit** mendefinisikan dua konstanta yang digunakan untuk menentukan bertambah/berkurangnya sebuah akun.

{% highlight java %}
package com.bustanil.easyaccounting.model;

public enum DebitCredit {

    DR, CR

}
{% endhighlight %}

Enum **AccountType** mendefinisikan jenis-jenis akun yang merupakan komponen utama dalam persamaan akuntansi.

{% highlight java %}
package com.bustanil.easyaccounting.model;

public enum AccountType {

    ASSET(DebitCredit.DR), EXPENSE(DebitCredit.DR),
    EQUITY(DebitCredit.CR), LIABILITY(DebitCredit.CR), REVENUE(DebitCredit.CR);

    private DebitCredit defaultDrCr;

    private AccountType(DebitCredit defaultDrCr){
        this.defaultDrCr = defaultDrCr;
    }

    public DebitCredit getDefaultDrCr(){
        return this.defaultDrCr;
    }

}
{% endhighlight %}