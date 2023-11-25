# Basic Routing dan Migration

## Langkah Praktikum

### Basic Routing

1. GET
   Gunakan aplikasi `lumenapi` yang sebelumnya telah dibuat
   Untuk menambahkan endpoint dengan method GET pada aplikasi kita, kita dapat mengunjungi file web.php pada folder routes. Kemudian tambahkan baris ini pada akhir file

   ```
   $router->get('/get', function () {
       return 'GET';
   });
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/f33f1e41-51f1-4353-8fdc-4ce0124dcbe6)

   jalankan aplikasi menggunakan command

   > [!NOTE]
   > Pastikan buka terminal pada folder aplikasi

   ```
   php -S localhost:8000 -t public
   ```

    <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/31142be0-d6c5-4944-8b14-bf8314c8452a)

   Setelah aplikasi berhasil dijalankan, path default berbentuk http://{BASE_URL}{PATH}, jika BASE_URL kita adalah localhost:8000 dan PATH kita adalah /get, maka url akan berbentuk http://localhost:8000/get

    <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/301e9ba8-6ae5-4bc8-8db7-6aec976e233a)

2. POST, PUT, PATCH, DELETE, dan OPTIONS
   Sama halnya saat menambahkan method GET, kita dapat menambahkan methode POST, PUT, PATCH, DELETE, dan OPTIONS pada file web.php dengan code seperti ini,

   ```
   ...
    $router->post('/post', function () {
        return 'POST';
    });
    $router->put('/put', function () {
        return 'PUT';
    });
    $router->patch('/patch', function () {
        return 'PATCH';
    });
    $router->delete('/delete', function () {
        return 'DELETE';
    });
    $router->options('/options', function () {
        return 'OPTIONS';
    });
    ...
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/65a11991-a66e-416c-83c7-08aacc7d170e)

   Setelah selesai menambahkan route untuk method POST, PUT, PATCH, DELETE, dan OPTIONS, kita dapat menjalankan server seperti pada saat percobaan GET. Setelah server berhasil menyala, kita dapat membuka aplikasi Postman atau Insomnia atau kita juga dapat menggunakan PowerShell (Windows) / Terminal (Linux atau Mac) untuk melakukan request ke server.
   Namun, pada percobaan kali ini kita akan menggunakan extensions pada VSCode yaitu Thunder Client.

   Instalasi thunder client
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/c8a5a73f-1d7b-484b-b2d3-700c455be94b)

   - membuka panel extensions, cari "thunder client"
   - setelah install, dapat membuka logo petir pada bar sebelah kiri
   - membuat request dengan "New Request"

   > [!NOTE]
   > thunder client tidak dapat terhubung dengan localhost, maka menggunakan url khusus yaitu

   ```
   http://[::1]:{PORT_NUMBER}/{PATH}

   Contoh
   http://[::1]:8000/post
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/7246264b-8a13-487a-8a37-774766f51959)

### Migration Database

1. Sebelum melakukan migrasi database pastikan server database aktif kemudian pastikan sudah membuat database dengan nama `lumenapi`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d5eeb124-fb9e-4541-97f1-5cc0ff3dee87)

2. Kemudian ubah konfigurasi database pada file .env menjadi seperti ini

   ```
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=lumenapi
   DB_USERNAME=root
   DB_PASSWORD=<<password masing-masing>>
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/baf915bf-948f-4d72-8c9e-87d1ddb892fa)

3. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file app.php pada folder bootstrap dan mengubah baris ini,

   ```
   # sebelumnya
   ...
   //$app->withFacades();
   //$app->withEloquent();
   ...

   # diubah menjadi
   ...
   $app->withFacades();
   $app->withEloquent();
   ...

   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/fed3bc29-52e4-4eee-ac15-8a80ab05644b)

4. Setelah itu jalankan command berikut untuk membuat file migration,

   ```
   php artisan make:migration create_users_table        #untuk membuat migrasi untuk tabel users
   php artisan make:migration create_products_table     #untuk membuat migrasi untuk tabel products
   ```

   Setelah menjalankan 2 syntax diatas, akan terbuat 2 file baru pada folder database/migrations dengan format YYYY_MM_DD_HHmmss_nama_migrasi.

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/fb90872b-5e6a-4daf-b8ae-a33ea933f1b1)

   <br>Pada file migrasi kita akan menemukan fungsi up() dan fungsi down(), fungsi up() akan digunakan pada saat kita melakukan migrasi, fungsi down() akan digunakan saat kita ingin me-rollback migrasi.

5. Ubah fungsi up pada file migrasi create_users_table
   ```
   # sebelumnya
   ...
   public function up(){
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
   ...
   # diubah menjadi
   ...
   public function up(){
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('name');
            $table->string('email');
            $table->string('password');
        });
    }
   ...
   ```
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/f9079824-c8d0-4727-b1d3-ee12465982d5)
6. Ubah fungsi up pada file migrasi create_products_table

   ```
   # sebelumnya
   ...
   public function up(){
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
    }
   ...
   # diubah menjadi
   ...
   public function up(){
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('name');
            $table->integer('category_id');
            $table->string('slug');
            $table->integer('price');
            $table->integer('weight');
            $table->text('description');
        });
    }
   ...
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/59b40996-8aa5-4ae5-a581-2e2281371f95)

7. Jalankan command,
   ```
   php artisan migrate
   ```
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/3b1566df-8eea-4220-ab35-7437a90c2713)
   tabel products telah terbuat
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/259c9ab6-1df6-4720-996d-2347cfa55eaf)
   tabel users telah terbuat
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/1bd0240f-29e0-441b-8059-de7d86a68d0c)
