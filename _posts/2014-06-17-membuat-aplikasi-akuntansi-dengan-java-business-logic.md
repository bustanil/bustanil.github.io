---
layout: post
title: Membuat aplikasi akuntansi sederhana dengan Java (4) - Business Logic
tags: java akuntansi
---

Business logic utama dari aplikasi ini adalah pembuatan jurnal untuk setiap transaksi yang terjadi. Tipe transaksi menentukan akun-akun yang terlibat berikut posisinya (debit/kredit). Maka dari itu, business logic pembuatan jurnal merupakan responsibility dari class `TransactionType`.

{% highlight java %}
package com.bustanil.easyaccounting.model;

public class TransactionType {

    private List<AccountConfig> involvedAccounts;

    public void addAccountConfig(Account account, DebitCredit debitCredit) {
        if(involvedAccounts == null){
            involvedAccounts = new ArrayList<AccountConfig>();
        }
        involvedAccounts.add(new AccountConfig(account, debitCredit));
    }

    public Journal createJournal(Transaction transaction) {
        Journal journal = new Journal(transaction.getTransactionDate(), new Date());
        for (AccountConfig accountConfig : involvedAccounts) {
            journal.addEntry(accountConfig, transaction);
        }
        return journal;
    }
}


{% endhighlight %}

Method `addEntry` kita tambahkan ke class Journal untuk mengenkapsulasi fungsi penambahan sebuah objek `JournalEntry` baru ke sebuah objek `Journal`.

{% highlight java %}
package com.bustanil.easyaccounting.model;

public class Journal {

    public void addEntry(AccountConfig accountConfig, Transaction transaction) {
        JournalEntry journalEntry = new JournalEntry(accountConfig.getAccount(), accountConfig.getDrCr(), transaction.getAmount());
        if(journalEntries == null){
            journalEntries = new ArrayList<JournalEntry>();
        }
        journalEntries.add(journalEntry);
    }

}
{% endhighlight %}

Selanjutkan kita test logic yang baru kita tambahkan. Untuk itu, kita buat sebuah unit test dengan nama `TestTransactions` di direktori `src/test/java` di package `com.bustanil.easyaccounting.model` dengan nama test method yaitu `testAddEquity`. Salah satu skenario yang akan kita uji adalah penambahan modal dalam bentuk tunai oleh pemilik perusahaan. Untuk skenario ini, kita siapkan dua akun yang terlibat yaitu akun kas dan akun modal, masing-masing dengan kode "CSH001" di posisi debit dan "EQU001" di posisi kredit. Di akhir test method kita cek apakah sebuah objek `Journal` berhasil dibuat dengan dua buah entry.

{% highlight java %}
package com.bustanil.easyaccounting.model;

public class TestTransactions {

    @Test
    public void testAddEquity() {
        TransactionType addEquity = new TransactionType();
        Account cashAccount = new Account("CSH001", "Cash account", AccountType.ASSET);
        Account equityAccount = new Account("EQU001", "Equity", AccountType.EQUITY);
        addEquity.addAccountConfig(cashAccount, DebitCredit.DR);
        addEquity.addAccountConfig(equityAccount, DebitCredit.CR);
        Transaction stockPurchaseTransaction = new Transaction(new Date(), "tambah modal usaha", addEquity, BigDecimal.valueOf(100));
        Journal journal = addEquity.createJournal(stockPurchaseTransaction);

        // pastikan ada 2 entri jurnal
        assertEquals(2, journal.getEntries().size());

        // pastikan ada entri dengan akun CSH001 di posisi debit
        JournalEntry cashJournalEntry = journal.getEntryByAccountCode("CSH001"); 
        assertNotNull(cashJournalEntry);
        assertEquals(cashJournalEntry.getDrCr(), DebitCredit.DR);
        assertEquals(cashJournalEntry.getAmount(), BigDecimal.valueOf(100));

        // pastikan ada entri dengan akun EQU001 di posisi kredit
        JournalEntry equityJournalEntry = journal.getEntryByAccountCode("EQU001");
        assertNotNull(equityJournalEntry);
        assertEquals(equityJournalEntry.getDrCr(), DebitCredit.CR);
        assertEquals(equityJournalEntry.getAmount(), BigDecimal.valueOf(100));

        // pastikan jurnalnya balance
        assertTrue(journal.isBalance());
    }

}
{% endhighlight %}

Ada dua method baru yang ditambahkan untuk mempermudah proses testing, yaitu `getEntryByAccountCode` dan `isBalance`
Method `getEntryByAccountCode` di class `Journal` digunakan untuk mencari entri jurnal berdasarkan kode akun tertentu. Method `isBalance` digunakan untuk memeriksa apakah jumlah sisi debit dan sisi kredit dari sebuah jurnal adalah sama.

Berikut ini adalah class `Journal` yang sudah diupdate:

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

    public Journal(Date transactionDate, Date postingDate) {
        this.transactionDate = transactionDate;
        this.postingDate = postingDate;
    }

    public List<JournalEntry> getEntries() {
        return journalEntries;
    }

    public JournalEntry getEntryByAccountCode(String accountCode) {
        for (JournalEntry journalEntry : journalEntries) {
            if(journalEntry.getAccount().getCode().equals(accountCode)){
                return journalEntry;
            }
        }
        throw new IllegalArgumentException("Entry not found for account code: " + accountCode);
    }

    public boolean isBalance() {
        BigDecimal drTotal = BigDecimal.ZERO;
        BigDecimal crTotal = BigDecimal.ZERO;
        for (JournalEntry journalEntry : journalEntries) {
            if(journalEntry.getDrCr() == DebitCredit.CR) {
                crTotal = crTotal.add(journalEntry.getAmount());
            }
            else {
                drTotal = drTotal.add(journalEntry.getAmount());
            }
        }
        return drTotal.compareTo(crTotal) == 0;
    }

    public void addEntry(AccountConfig accountConfig, Transaction transaction) {
        JournalEntry journalEntry = new JournalEntry(accountConfig.getAccount(), accountConfig.getDrCr(), transaction.getAmount());
        if(journalEntries == null){
            journalEntries = new ArrayList<JournalEntry>();
        }
        journalEntries.add(journalEntry);
    }
}

{% endhighlight %}


### Source code

Source code bagian 4 dapat didownload di [https://github.com/bustanil/easyaccounting/tree/part4](https://github.com/bustanil/easyaccounting/tree/part4)