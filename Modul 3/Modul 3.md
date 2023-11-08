Percobaan instalasi NodeJS

1. Buka halaman https://nodejs.org/en

2. Download dan jalankan node setup
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/e12e7e80-a826-4153-849e-beda18480033)

3. Setelah instalasi selesai jalankan command node -v untuk memeriksa apakah NodeJS sudah terinstall
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/5ecb0b48-2c57-40f5-be65-54b0da0fdf66)

### Inisiasi project Express dan pemasangan package

1. Lakukan pembuatan folder dengan nama express-mongodb dan masuk ke dalam folder tersebut lalu buka melalui text editor masing-

2. Lakukan npm init untuk mengenerate file package.json dengan menggunakan command

   ```
   npm init -y
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/872707f5-0a99-443a-a095-0b7937577029)

3. Lakukan instalasi express, mongoose, dan dotenv dengan menggunakan command

   ```
   npm i express mongoose dotenv
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/2501af75-ea2f-4c2d-a2ef-a66de17ef27d)

### Koneksi Express ke MongoDB (VSCode)

1. Buatlah file index.js pada root folder dan masukkan kode di bawah ini

   ```
   require('dotenv').config();
   const express = require('express');
   const mongoose = require('mongoose');

   const app = express();

   app.use(express.json());

   app.get('/', (req, res) => {
       res.status(200).json({
           message: '<nama>,<nim>'
       })
   })

   const PORT = 8000;
   app.listen(PORT, () => {
       console.log(`Running on port ${PORT}`);
   })
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/0a0aec24-0daa-45a9-8aed-c422d14ceba6)

Setelah itu coba jalankan aplikasi dengan command node index.js
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/9aafb96a-cb2d-4224-a070-3598e5c2e3f2)

2. Lakukan pembuatan file .env dan masukkan baris berikut

```
PORT=5000
```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/b2545258-8b0e-46c8-a656-33ab9bd07e8d)

Setelah itu ubahlah kode pada listening port menjadi berikut dan coba jalankan aplikasi kembali

```
   const PORT = process.env.PORT || 8000;
   app.listen(PORT, () => {
       console.log(`Running on port ${PORT}`);
   })
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/a289b8c5-32c5-4d83-b812-fa5162e63522)

![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/7f32580a-6aa7-45bb-94d0-f021e0fa7f80)

3. Copy connection string yang terdapat pada compas atau atlas dan paste kan pada .env seperti berikut

   ```
   MONGO_URI = Connection string masing-masing
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/19ff0a1e-a4db-415e-b445-48996678d0d0)

4. Tambahkan baris kode berikut pada file index.js

   ```
   require('dotenv').config();
   const express = require('express');
   const mongoose = require('mongoose');

   mongoose.connect(process.env.MONGO_URI);
   const db = mongoose.connection;

   db.on('error', (error) => {
       console.log(error);
   });

   db.once('connected', () => {
       console.log('Mongo connected');
   })
   ...
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/8890dbcd-88fb-4af3-8e5e-b1d2b3980d9e)

Setelah itu coba jalankan aplikasi kembali
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/47e48b90-f8bf-4888-81d3-6b5899cd1d90)

### Pembuatan routing

1. Pembuatan directory routes di tingkat yang sama dengan index.js

2. Buatlah file book.route.js di dalamnya
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/9333e95c-c754-46c6-81a3-3584a4963656)

3. Tambahkan baris kode berikut untuk fungsi getAllBooks

   ```
   const router = require('express').Router();

   router.get('/', function getAllBooks(req, res) {
       res.status(200).json({
           message: 'mendapatkan semua buku'
       })
   })

   module.exports = router;
   ```

4. Lakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook

   ```
   const router = require('express').Router();
   ...

   router.get('/:id', function getOneBook(req, res) {
       const id = req.params.id;
       res.status(200).json({
           message: 'mendapatkan satu buku',id,
           })
   })

   router.post('/', function createBook(req, res) {
       res.status(200).json({
           message: 'membuat buku baru'
       })
   })

   router.put('/:id', function updateBook(req, res) {
       const id = req.params.id;
       res.status(200).json({
       message: 'memperbaharui satu buku',id,
       })
   })

   router.delete('/:id', function deleteBook(req, res) {
       const id = req.params.id;
       res.status(200).json({
           message: 'menghapus satu buku',id,
       })
   })

   module.exports = router;
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/923cf9f6-fde0-4b04-8615-eecde9b2aef2)

5. Lakukan import book.route.js pada file index.js dan tambahkan baris kode berikut

   ```
   require('dotenv').config();
   const express = require('express');
   const mongoose = require('mongoose');
   const bookRoutes = require('./routes/book.route'); //

   ...

   app.get('/', (req, res) => {
       res.status(200).json({
           message: '<nama>,<nim>'
       })
   })

   app.use('/books', bookRoutes); //

   const PORT = process.env.PORT || 8000;
   app.listen(PORT, () => {
       console.log(`Running on port ${PORT}`);
   })
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d64caaf0-b38e-4e2b-ac8f-258591374edb)

6. Uji salah satu endpoint menggunakan Postman
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/cdcf78d9-72ed-40cc-af3a-6e581c9951c3)

### Pembuatan controller

1. Lakukan pembuatan direktori controllers di tingkat yang sama dengan index.js

2. Buatlah file book.controller.js di dalamnya

3. Salin baris kode dari routes untuk fungsi getAllBooks

   ```
   function getAllBooks(req, res) {
       res.status(200).json({
       message: 'mendapatkan semua buku'
       })
   };

   module.exports = {
       getAllBooks,
   }
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/7bea8e3e-99c3-4f1e-b2e1-900d146124d2)

4. Lakukan hal yang sama untuk getOneBook, createBook, updateBook, dan deleteBook

   ```
   ...
   function getOneBook(req, res) {
       const id = req.params.id;
       res.status(200).json({
           message: 'mendapatkan satu buku',id,
       })
   }

   function createBook(req, res) {
       res.status(200).json({
           message: 'membuat buku baru'
       })
   }

   function updateBook(req, res) {
       const id = req.params.id;
       res.status(200).json({
           message: 'memperbaharui satu buku',id,
       })
   }

   function deleteBook(req, res) {
       const id = req.params.id;
       res.status(200).json({
           message: 'menghapus satu buku',id,
       })
   }

   module.exports = {
   getAllBooks,
   getOneBook, //
   createBook, //
   updateBook, //
   deleteBook //
   }
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/be558a5e-1838-45be-a022-0db7bf4d97b1)

5. Lakukan import book.controller.js pada file book.route.js

   ```
   const router = require('express').Router();
   const book = require('../controllers/book.controller'); //
   ...
   module.e
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/bc429678-3032-437b-adc9-f0c7c00bb726)

6. Lakukan perubahan pada book.route agar dapat memanggil fungsi dari book.controller.js

   ```
   const router = require('express').Router();
   const book = require('../controllers/book.controller');

   router.get('/', book.getAllBooks);
   router.get('/:id', book.getOneBook);
   router.post('/', book.createBook);
   router.put('/:id', book.updateBook);
   router.delete('/:id', book.deleteBook);
   module.exports = router;
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/5d4c76d7-e950-4ae1-978c-169fedf4011b)

7. Lakukan pengujian kembali, pastikan response tetap sama
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/8c0420aa-f10a-4fe9-b55a-374a265f0ef2)

### Pembuatan Model

1. Lakukan pembuatan direktori models di tingkat yang sama dengan index.js

2. Buatlah file book.model.js di dalamnya
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/e04e67dc-180b-4f58-ac6c-7ae04e1bcdf1)

3. Tambahkan baris kode berikut sesuai dengan tabel di atas

   ```
   const mongoose = require('mongoose');
   const bookSchema = new mongoose.Schema({
       title: {
           type: String
       },
       author: {
           type: String
       },
       year: {
           type: Number
       },
       pages: {
           type: Number
       },
       summary: {
           type: String
       },
       publisher: {
           type: String
       }
   })
   module.exports = mongoose.model('book', bookSchema);
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d6e575af-e23c-4962-8e9f-cbfcd420e2db)

### Operasi CRUD

1. Hapus semua data pada collection books
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/345c63fd-a832-4ae8-b17a-a79674727b4b)

2. Lakukan import book.model.js pada file book.controller.js

   ```
   const Book = require('../models/book.model');
   ...
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/f65686d8-106a-4383-b975-b2d871f396dc

3. Lakukan perubahan pada fungsi createBook

   ```
   const Book = require('../models/book.model');
   ...
   async function createBook(req, res) {
       const book = new Book({
           title: req.body.title,
           author: req.body.author,
           year: req.body.year,
           pages: req.body.pages,
           summary: req.body.summary,
           publisher: req.body.publisher,
       })
       try {
           const savedBook = await book.save();
           res.status(200).json({
               message: 'membuat buku baru',
               book: savedBook,
           })
       } catch (error) {
           res.status(500).json({
           message: 'kesalahan pada server',
           error: error.message,
           })
       }
   }
   ...
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/ef390965-5f68-4c7d-ae1b-05913bae5907)

4. Jalankan `node index.js` dan buat dua buah buku dengan data di bawah ini dengan postman

   ```
   {
   "title": "Dilan 1990",
   "author": "Pidi Baiq",
   "year": 2014,
   "pages": 332,
   "summary": "Mirea, anata wa utsukushī",
   "publisher": "Pastel Books"
   }
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/e3cb22db-cb41-4e06-b281-0cf00697af1a)
   

   ```
   {
   "title": "Dilan 1991",
   "author": "Pidi Baiq",
   "year": 2015,
   "pages": 344,
   "summary": "Watashi ga kare o aishite iru to ittara",
   "publisher": "Pastel Books"
   }
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/dfe3ae64-3afe-4926-a3a9-6dc31bd777e3)

5. Lakukan perubahan pada fungsi getAllBooks

   ```
   const Book = require('../models/book.model');
   async function getAllBooks(req, res) {
       try {
           const books = await Book.find();
           res.status(200).json({
           message: 'mendapatkan semua buku',books,
           })
       } catch (error) {
       res.status(500).json({
           message: 'kesalahan pada server',
           error: error.message,
           })
       }
   }
   ...
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/c146c60c-bc08-439f-9bb0-4d4bbd0fa6bd)

6. Lakukan perubahan pada fungsi getOneBook

   ```
   const Book = require('../models/book.model');
   ...
   async function getOneBook(req, res) {
       const id = req.params.id;
       try {
           const book = await Book.findById(id);
           res.status(200).json({
           message: 'mendapatkan satu buku',book,
           })
       } catch (error) {
           res.status(500).json({
           message: 'kesalahan pada server',
           error: error.message,
           })
       }
   }
   ...
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/9009ca2c-28ca-4190-9f74-dc76a913cba3)
   
7. Tampilkan semua buku dengan Postman
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/796029ef-b202-4504-8754-cb63433091c6)

8. Tampilkan buku Dilan 1990 dengan Postman
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/630e44b2-c3bd-4c84-beaa-088d5d24da88)

9. Lakukan perubahan pada fungsi updateBook

   ```
   const Book = require('../models/book.model');
   ...
   async function updateBook(req, res) {
       const id = req.params.id;
       try {
           const book = await Book.findByIdAndUpdate(
           id, req.body, { new: true }
           )
           res.status(200).json({
               message: 'memperbaharui satu buku',book,
           })
       } catch (error) {
           res.status(500).json({
           message: 'kesalahan pada server',
           error: error.message,
           })
       }
   }
   ...
   ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/15d10df5-dfe3-4481-9d5c-287ed7b2987a)
   
10. Ubah judul buku Dilan 1991 menjadi “NAMA_PANGGILAN 1991” dengan Postman
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/bf0506a5-35bf-490d-94f9-89d1195d38cb)

11. Lakukan perubahan pada fungsi deleteBook

    ```
    const Book = require('../models/book.model');
    ...
    async function deleteBook(req, res) {
        const id = req.params.id;
        try {
            const book = await Book.findByIdAndDelete(id);
            res.status(200).json({
                message: 'menghapus satu buku',book,
            })
        } catch (error) {
            res.status(500).json({
            message: 'kesalahan pada server',
            error: error.message,
            })
        }
    }
    ...
    ```
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/8322f2c1-20c5-41ee-8ad9-5771e7d1e1bb)
    
12. Hapus buku Dilan 1990 dengan Postman
![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/ba9ddab4-9d5c-4e7f-b74b-e6e16fc0e44f)
