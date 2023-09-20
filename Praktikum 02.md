# Modul 2 Praktikum Pemin TI-A
>### RM Ferdinandrey Hananta
>### 215150701111043

## MongoDB Compass
### 1. Lakukan koneksi ke MongoDB menggunakan connection string
>![Alt text](Screenshot02/Ss1.png)

### 2. Buat database dengan melakukan klik “Create Database”
>![Alt text](Screenshot02/Ss2.png)

### 3. Lakukan insert buku pertama dengan melakukan klik “Add Data”, pilih “Insert Document”, isi dengan data yang diinginkan dan klik “Insert”
>![Alt text](Screenshot02/Ss3.png)

### 4. Lakukan insert buku kedua dengan cara yang sama.
>![Alt text](Screenshot02/Ss4.png)

### 5. Lakukan pencarian buku dengan author “Osamu Dazai” dengan mengisi filter yang diinginkan dan klik “Find”
>![Alt text](Screenshot02/Ss5.png)

### 6. Lakukan perubahan summary pada buku “No Longer Human” menjadi “Buku yang bagus (<NAMA>,<NIM>) dengan melakukan klik “Edit Document” (berlambang pensil), mengisi nilai summary yang baru, dan melakukan klik “Update”
>![Alt text](Screenshot02/Ss6.png)
>![Alt text](Screenshot02/Ss7.png)

### 7. Lakukan penghapusan pada buku “I Am a Cat” dengan melakukan klik “Remove Document” (berlambang tong sampah) dan melakukan klik “Delete”
>![Alt text](Screenshot02/Ss8.png)
>![Alt text](Screenshot02/Ss9.png)


## MongoDB Shell

### 1. Lakukan koneksi ke MongoDB Server dengan menjalankan command mongosh bagi yang menggunakan terminal build in OS, Apabila kalian menggunakan MongoDB atlas, silahkan copy connection string dari MongoDB atlas kalian masing-masing dan paste kan di terminal kalian sehingga tampilan terminal kalian akan menjadi seperti berikut
>![Alt text](Screenshot02/Ss10.png)

### 2. Mencoba melihat list database yang ada di server dengan menjalankan command show dbs
>![Alt text](Screenshot02/Ss11.png)

>Untuk berpindah ke database “bookstore” gunakan command use bookstore , kalian dapat memastikan telah berpindah ke database yang benar dengan melihat tulisan sebelum tanda “>”
>![Alt text](Screenshot02/Ss12.png)

>Cobalah untuk melihat collection yang ada pada database tersebut dengan menggunakan command show collections
>![Alt text](Screenshot02/Ss13.png)

### 3. Lakukan insert buku “Overlord I” dengan menggunakan command db.books.insertOne(<data kalian>) , setelah insert buku berhasil maka MongoDB akan mengembalikan pesan sebagai berikut.
>![Alt text](Screenshot02/Ss14.png)

### 4. Lakukan insert buku “The Setting Sun” dan “Hujan” dengan insert many dengan menggunakan command db.books.insertMany(<data kalian>) , dan akan mengembalikan pesan sebagai berikut.
>![Alt text](Screenshot02/Ss15.png)

### 5. Lakukan pencarian buku dengan menggunakan command db.books.find() untuk melakukan pencarian semua buku.
>![Alt text](Screenshot02/Ss16.png)

6. Tampilkan seluruh buku dengan author “Osamu Dazai” dengan mengisi argument pada find() dengan menggunakan command db.books.find({<filter yang ingin diisi>})
>![Alt text](Screenshot02/Ss17.png)

### 7. Lakukan perubahan summary pada buku “Hujan” menjadi “Buku yang bagus (<NAMA>,<NIM>) dengan mengunakan command db.books.updateOne({<filter>}, {$set: {<data yang akan di update>}}) sehingga output yang dihasilkan oleh MongoDB akan menjadi seperti berikut
>![Alt text](Screenshot02/Ss18.png)

### 8. Lakukan perubahan publisher menjadi “Yen Press” pada semua buku “Osamu Dazai” dengan menggunakan command db.books.updateMany({<filter>}, {$set: {<data yang akan di update>}})
>![Alt text](Screenshot02/Ss19.png)

### 9.Lakukan penghapusan pada buku “Overlord I” dengan menggunakan command db.books.deleteOne({<argument>})
>![Alt text](Screenshot02/Ss20.png)

### 10. Lakukan penghapusan pada semua buku “Osamu Dazai dengan menggunakan command db.books.deleteMany({<argument>})
>![Alt text](Screenshot02/Ss21.png)
