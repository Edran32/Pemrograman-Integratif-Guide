# Register Authentication dan Authorization

## Dasar teori

## Langkah Percobaan

> [!NOTE]
> menggunakan aplikasi lumenapi pada modul 4

<br> php -S localhost:8000 -t public

### Register

1. Pastikan terdapat tabel users yang dibuat menggunakan migration pada bab 3 Basic Routing dan Migration. Berikut informasi kolom yang harus ada
   `id`,`createdAt`, `updatedAt`, `name`, `email`, `password`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/29389aca-70a6-478e-9c34-39bcbb6f8028)
2. Pastikan terdapat model User.php yang digunakan pada bab 5 Model Controller dan Request-Response Handler. Berikut baris kode yang harus ada

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

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/12fcf7ee-2d41-4e45-bfee-100a19f93ec4)

3. Buatlah file AuthController.php dan isilah dengan baris kode berikut

   ```
   <?php

   namespace App\Http\Controllers;
   use App\Models\User;
   use Illuminate\Http\Request;
   use Illuminate\Support\Facades\Hash;

   class AuthController extends Controller
   {
      /**
      * Create a new controller instance.
      *
      * @return void
      */
      public function __construct()
      {
      //
      }
      public function register(Request $request)
      {
         $name = $request->name;
         $email = $request->email;
         $password = Hash::make($request->password);
         $user = User::create([
               'name' => $name,
               'email' => $email,
               'password' => $password
         ]);
         return response()->json([
               'status' => 'Success',
               'message' => 'new user created',
               'data' => [
                  'user' => $user,
               ]
         ],200);
      }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d310bd74-d21a-4c13-a2b4-154af25c2945)

4. Tambahkan baris berikut pada routes/web.php

   ```
   <?php
   ...
   $router->group(['prefix' => 'auth'], function () use ($router) {
   $router->post('/register', ['uses'=> 'AuthController@register']);
   });
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/933a70a5-32fb-4d69-8b90-41550a72c1c5)

5. Jalankan aplikasi pada endpoint /auth/register dengan body berikut

   ```
   {
      "name": "Scaramouche",
      "email": "scaramouche@fatui.org",
      "password": "wanderer"
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/4bf780b2-f844-4c1e-9275-e83ffe63ca5b)

### Authentication

1. Buatlah fungsi login(Request $request) pada file AuthController.php

   ```
   public function login(Request $request)
      {
         $email = $request->email;
         $password = $request->password;
         $user = User::where('email', $email)->first();
         if (!$user) {
               return response()->json([
               'status' => 'Error',
               'message' => 'user not exist',
               ],404);
         }
         if (!Hash::check($password, $user->password)) {
               return response()->json([
               'status' => 'Error',
               'message' => 'wrong password',
               ],400);
         }
         return response()->json([
               'status' => 'Success',
               'message' => 'successfully login',
               'data' => [
               'user' => $user,
               ]
         ],200);
      }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/90306b5a-f489-417d-b5ad-bdd028155d27)

2. Tambahkan baris berikut pada routes/web.php

   ```
   <?php
   ...
   $router->group(['prefix' => 'auth'], function () use ($router) {
      $router->post('/register', ['uses'=> 'AuthController@register']);
      $router->post('/login', ['uses'=> 'AuthController@login']); // route login
   });
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/e1ca9773-6f07-4488-997d-dea523424327)

3. Jalankan aplikasi pada endpoint /auth/login dengan body berikut

   ```
   {
      "email": "scaramouche@fatui.org",
      "password": "wanderer"
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/68ab7e8a-756d-470e-ac2c-bfb3dfac9b40)
   <br>coba dengan menyalahkan email atau password
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/79b52625-08e7-4daa-b8a1-09d29ecc519c)

### Token

1. Jalankan perintah berikut untuk membuat migrasi baru
   `php artisan make:migration add_column_token_to_users`

2. Tambahkan baris berikut pada migration yang baru terbuat

   ```
   <?php

   use Illuminate\Database\Migrations\Migration;
   use Illuminate\Database\Schema\Blueprint;
   use Illuminate\Support\Facades\Schema;

   return new class extends Migration
   {
      /**
      * Run the migrations.
      */
      public function up(): void
      {
         Schema::table('users', function (Blueprint $table) {
               $table->string('token', 72)->unique()->nullable();
         });
      }

      /**
      * Reverse the migrations.
      */
      public function down(): void
      {
         Schema::table('users', function (Blueprint $table) {
               //
         });
      }
   };

   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/aaad3439-e789-47ea-8e78-d06ba22e6e02)

3. Tambahkan atribut token di $fillable pada User.php

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
         'name', 'email', 'password', 'token'
      ];
      /**
      * The attributes excluded from the model's JSON form.
      *
      * @var array
      */
      protected $hidden = [];
   }

   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/f9dc1dc0-ee4a-42a0-be26-f8b6d1a487d8)

4. Tambahkan baris berikut pada file AuthController.php

   ```
   use Illuminate\Support\Str;
   ...
   public function login(Request $request)
      {
         $email = $request->email;
         $password = $request->password;
         $user = User::where('email', $email)->first();
         if (!$user) {
               return response()->json([
               'status' => 'Error',
               'message' => 'user not exist',
               ],404);
         }
         if (!Hash::check($password, $user->password)) {
               return response()->json([
               'status' => 'Error',
               'message' => 'wrong password',
               ],400);
         }
         $user->token = Str::random(36); //
         $user->save(); //
         return response()->json([
               'status' => 'Success',
               'message' => 'successfully login',
               'data' => [
                  'user' => $user,
               ]
         ],200);
      }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/ac8e60bd-3de1-42b8-9173-276a1d62c8d6)

5. Jalankan perintah di bawah untuk menjalankan migrasi terbaru `php artisan migrate`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/b3c1c99c-3e0f-44d3-8257-32573175b4ae)

6. Jalankan aplikasi pada endpoint /auth/login dengan body berikut. Salinlah token yang didapat ke notepad

   ```
      {
         "email": "scaramouche@fatui.org",
         "password": "wanderer"
      }

   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/1ad72002-8c36-4980-ab3a-255393285afa)

### Authorization

1. Buatlah file Authorization.php pada folder App/Http/Middleware dan isilah dengan baris berikut

   ```
   <?php

   namespace App\Http\Middleware;
   use App\Models\User;
   use Closure;

   class Authorization
   {
      /**
      * Handle an incoming request.
      *
      * @param \Illuminate\Http\Request $request
      * @param \Closure $next
      * @return mixed
      */
      public function handle($request, Closure $next)
      {
         $token = $request->header('token') ?? $request->query('token');
         if (!$token) {
               return response()->json([
               'status' => 'Error',
               'message' => 'token not provided',
               ],400);
         }
         $user = User::where('token', $token)->first();
         if (!$user) {
               return response()->json([
               'status' => 'Error',
               'message' => 'invalid token',
               ],400);
         }
         $request->user = $user;
         return $next($request);
      }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/edf06e61-0a74-4196-9b36-1a9e256dccaf)

2. Tambahkan middleware yang baru dibuat pada bootstrap/app.php.

   ```
   /*
   |--------------------------------------------------------------------------
   | Register Middleware
   |--------------------------------------------------------------------------
   |
   | Next, we will register the middleware with the application. These can
   | be global middleware that run before and after each request into a
   | route or middleware that'll be assigned to some specific routes.
   |
   */
   // $app->middleware([
   // App\Http\Middleware\ExampleMiddleware::class
   // ]);
   $app->routeMiddleware([
      'auth' => App\Http\Middleware\Authorization::class, //
   ]);
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/4368e8f5-129d-4a92-8cb6-d5fedb73f5ff)

3. Buatlah fungsi home() pada HomeController.php

   ```
   public function home(Request $request)
      {
         $user = $request->user;
         return response()->json([
               'status' => 'Success',
               'message' => 'selamat datang ' . $user->name,
      ],200);
      }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d24b00f4-ca84-4782-9a04-0fe22946c164)

4. Tambahkan baris berikut pada routes/web.php

   ```
   <?php
   $router->get('/', ['uses' => 'HomeController@index']);
   $router->get('/hello', ['uses' => 'HomeController@hello']);
   $router->get('/home', ['middleware' => 'auth','uses' => 'HomeController@home']); //
   ...
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/2959c17e-b601-42bc-b97f-bc5f68bcb402)

5. Jalankan aplikasi pada endpoint /home dengan melampirkan nilai token (pada header) yang
   didapat setelah login pada header
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/166cbbc0-176a-47a5-8c30-7770350259a1)
   <br>Dengan token yang tidak sesuai
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d39dcdcd-7e69-49f6-9d4f-6f8c92475819)
   <br>Tanpa token
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/7dc85440-6907-4e02-a0a1-1b6db065813f)
