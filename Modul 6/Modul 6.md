# Model, Controller dan Request - Response Handler

## Langkah Praktikum

> [!NOTE]
> menggunakan aplikasi lumenapi pada modul 4

### Model

1. Pastikan terdapat tabel users yang dibuat menggunakan migration pada bab sebelumnya. Berikut informasi kolom yang harus ada
   `id, createdAt, updatedAt, name, email, password`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/6e19464f-5074-4755-b9a9-0dd3507c4f4e)

2. Bersihkan isi app/Models/User.php yang ada sebelumnya dan isi dengan baris kode berikut

   ```
   <?php
   namespace App\Models;
   use Illuminate\Database\Eloquent\Model;
   class User extends Model {
       /**
       * The attributes that are mass assignable.
       *
       * @var array
       */
       protected $fillable = [
           'name', 'email', 'password'
       ];
       /**
       * The attributes excluded from the model's JSON form.
       *
       * @var array
       */
       protected $hidden = [];
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/6076de60-9b50-4d9c-b837-e33cab330ede)

### Controller

1. Buatlah salinan ExampleController.php pada folder app/Http/Controllers dengan nama HomeController.php dan buatlah fungsi index() yang berisi

   ```
   <?php
   namespace App\Http\Controllers;
   class HomeController extends Controller{
       /**
       * Create a new controller instance.
       *
       * @return void
       */
       public function __construct()
       {
           //
       }
       public function index()
       {
           return 'Hello, from lumen!';
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/c7051f43-495c-4ad5-b015-4e023eb053cd)

2. Ubah route '/' pada file routes/web.php menjadi seperti ini

   ```
   $router->get('/', function () use ($router) {
       return $router->app->version();
   });

   MENJADI

   $router->get('/', ['uses' => 'HomeController@index']);
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/cd511b92-a2c1-4f4a-a1ae-49bb0cb695d0)

3. Jalankan aplikasi
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/a328a702-cae7-45ff-aa31-d0efc38f50e6)

### request handler

1. Lakukan import library Request dengan menambahkan baris berikut di bagian atas file

   ```
   <?php
   namespace App\Http\Controllers;

   use Illuminate\Http\Request;     // Import Library Request
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/1d53e98e-4a15-49b3-8091-0229283f5034)

2. Ubah fungsi index menjadi

   ```
   <?php
   namespace App\Http\Controllers;
   use Illuminate\Http\Request;
   class HomeController extends Controller{
       /**
       * Create a new controller instance.
       *
       * @return void
       */
       public function __construct()
       {
       //
       }
       public function index (Request $request)
       {
           return 'Hello, from lumen! We got your request from endpoint: ' . $request->path();
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/259f0c8f-b4ed-4cec-981b-33c76bafeb71)

3. Jalankan aplikasi
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d4c4bfde-ba7e-4a86-b2de-5ca9076ba102)


### Response Handler

1. Lakukan import library Response dengan menambahkan baris berikut di bagian atas file

   ```
   <?php
   namespace App\Http\Controllers;
   use Illuminate\Http\Request;
   use Illuminate\Http\Response;        // import library Response
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d32f5712-3ba2-42e7-a6f0-f0aa6e95aaa8)

2. Tambahkan fungsi hello() dalam controller HomeController.php

   ```
   public function hello()
   {
    $data['status'] = 'Success';
    $data['message'] = 'Hello, from lumen!';
    return (new Response($data, 201))->header('Content-Type', 'application/json');
    }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/2acf503a-4276-4734-9082-20bc95f368f9)

3. Tambahkan route /hello pada file routes/web.php

   ```
   $router->get('/hello', ['uses' => 'HomeController@hello']); // route hello
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/346fdd1f-29e1-41cd-9922-65c9f517c9fc)

4. Jalankan aplikasi pada route /hello
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/9463d4b9-9018-4bc4-a048-205db4f41c14)

## Penerapan

1. Lakukan import model User dengan menambahkan baris berikut di bagian atas file

   ```
   <?php
   namespace App\Http\Controllers;
   use Illuminate\Http\Request;
   use Illuminate\Http\Response;
   use App\Models\User;     // import model User
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/6219dae6-8b1c-4e30-96c0-701a48f60196)

2. Tambahkan ketiga fungsi berikut di HomeController.php

   ```
    public function defaultUser()
    {
        $user = User::create([
            'name' => 'Nahida',
            'email' => 'nahida@akademiya.ac.id',
            'password' => 'smol'
        ]);

        return response()->json([
            'status' => 'Success',
            'message' => 'default user created',
            'data' => ['user' => $user,]
        ],200);
    }
    public function createUser(Request $request)
    {
        $name = $request->name;
        $email = $request->email;
        $password = $request->password;
        $user = User::create([
            'name' => $name,
            'email' => $email,
            'password' => $password
        ]);

        return response()->json([
            'status' => 'Success',
            'message' => 'new user created',
            'data' => [
            'user' => $user,]
        ],200);
    }
    public function getUsers()
    {
        $users = User::all();
        return response()->json([
           'status' => 'Success',
           'message' => 'all users grabbed',
           'data' => [
           'users' => $users,
           ]
        ],200);
    }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/645bf929-e8f5-4d4f-b162-d0c17c0e4042)

3. Tambahkan ketiga route pada file routes/web.php menggunakan group route

   ```
   $router->group(['prefix' => 'users'], function () use ($router) {
   $router->post('/default', ['uses' => 'HomeController@defaultUser']);
   $router->post('/new', ['uses' => 'HomeController@createUser']);
   $router->get('/all', ['uses' => 'HomeController@getUsers']);
   });
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/2ada4287-7c46-423e-bcb6-2d8f2135ddd9)

4. Jalankan aplikasi pada route /users/default menggunakan Postman
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/1266fc86-2930-4695-8fb3-49dce4481880)

5. Jalankan aplikasi pada route /users/new dengan mengisi body sebagai berikut
   <br>`name: cyno`
   <br>`email: cyno@akademiya.ac.id`
   <br>`password: mahamatra`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/4384994e-ddc9-474f-a53b-ff9df20d3b51)


6. Jalankan aplikasi pada route /users/all
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/fea83150-0052-44de-8cb8-db92044bfd45)

   <br>Data tersimpan dalam database
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/fe7f2327-ddb9-419f-a26b-a1367e2d9759)
