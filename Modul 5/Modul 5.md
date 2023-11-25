
# Dynamic Route dan Middleware

## Langkah Praktikum

> [!NOTE]
> menggunakan aplikasi lumenapi pada modul 4

### Dynamic Route

Dynamic route adalah route yang dapat berubah-ubah, contohnya pada saat kita membuka suatu halaman web, kadang kita melihat `/users/1` atau `/users/2`, hal ini yang dinamakan dynamic routes.
Untuk menambahkan dynamic routes pada aplikasi lumen kita, kita dapat menggunakan syntax berikut,

```
$router->get('/user/{id}', function ($id) {
    return 'User Id = ' . $id;
});
```

<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/6a352fe4-c818-4f22-92cd-718c6b1edb9c)
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/35bac238-829b-4d9f-9520-741bfba17a49)

Saat menambahkan parameter pada routes, kita tidak terbatas pada 1 variable saja, namun kita dapat menambahkan sebanyak yang diperlukan seperti kode berikut,

```
$router->get('/post/{postId}/comments/{commentId}', function ($postId, $commentId) {
    return 'Post ID = ' . $postId . ' Comments ID = ' . $commentId;
});
```

<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/2b25d58e-2f40-4cd7-85bb-11d9a46ae8b3)
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/2288c7fa-b25c-4b5d-97ff-d29e7b7796f1)

Pada dynamic routes kita juga bisa menambahkan optional routes, yang mana optional routes tidak mengharuskan kita untuk memberi variable pada endpoint kita, namun saat kita memanggil endpoint, dapat menggunakan parameter variable ataupun tidak, seperti pada kode dibawah ini,

```
$router->get('/users[/{userId}]', function ($userId = null) {
    return $userId === null ? 'Data semua users' : 'Data user dengan id ' . $userId;
});
```

<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d7b74858-9bc5-43de-84b9-2b5954ed677f)
<br>hasil ketika mengakses endpoint /users
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/e0659e72-5605-4d62-88e3-530ee2d47047)
<br>hasil ketika mengakses endpoint /users/1
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/05833885-cd20-435b-abc2-8a4149c142e2)

### Aliases Route

Aliases Route digunakan untuk memberi nama pada route yang telah kita buat, hal ini dapat membantu kita, saat kita ingin memanggil route tersebut pada aplikasi kita. Berikut syntax untuk menambahkan aliases route

```
$router->get('/auth/login', ['as' => 'route.auth.login', function ($a) {
    return 'Login page';
}]);
...
$router->get('/profile', function (Request $request) {
    if ($request->isLoggedIn) {
        return redirect()->route('route.auth.login');
    }
});
```

<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/c523aace-dab4-4667-a413-a91a8cd25477)
<br>ketika mengakses endpoint /profile, otomatis redirect ke /auth/login (a.k.a route.auth.login)
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/75725ffe-c4bc-4ed2-aca6-16865d352b6d)

### Group Route

Pada lumen, kita juga dapat memberikan grouping pada routes kita agar lebih mudah pada saat penulisan route pada web.php. Kita dapat melakukan grouping dengan menggunakan syntax berikut,

```
$router->group(['prefix' => 'users'], function () use ($router) {
    $router->get('/', function () {         #path memiliki prefix, menjadi /users/<endpoint> secara default
        return "GET /users";
    });
});
```

<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/9eeed9c7-eefb-4bc0-b933-e56b7be31fc5)
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/4f6201d7-4401-497f-8d56-57f52faab354)

Selain dapat mengelompokkan prefix, kita juga dapat mengelompokkan middleware dan
namespace pada kelompok routes kita.

### Middleware

Middleware adalah penengah antara komunikasi aplikasi dan client. Middleware biasanya digunakan untuk membatasi siapa yang dapat berinteraksi dengan aplikasi kita dan semacamnya, kita dapat menambahkan middleware dengan menambahkan file pada folder `app/Http/Middleware`. Pada folder tersebut terdapat file `ExampleMiddleware`, kita dapat mencopy file tersebut untuk membuat middleware baru. Pada praktikum kali ini akan dibuat middleware Age dengan isi,

```
class AgeMiddleware{
    /**
    * Handle an incoming request.
    *
    * @param \Illuminate\Http\Request $request
    * @param \Closure $next
    * @return mixed
    */
    public function handle($request, Closure $next){
        if ($request->age < 17)
            return redirect('/fail');
        return $next($request);
    }
}
```

<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/f1751147-8de8-4dc0-93e5-eece6c4bfdca)

Setelah menambahkan filter pada AgeMiddleware , kita harus mendaftarkan
AgeMiddleware pada aplikasi kita, pada file `bootstrap/app.php` seperti berikut ini,

```
...
1   // $app->middleware([
2   //      App\Http\Middleware\ExampleMiddleware::class
3   // ]);
4
5   // $app->routeMiddleware([
6   //    'auth' => App\Http\Middleware\Authenticate::class,
7   //    'age' => App\Http\Middleware\AgeMiddleware::class
8   // ]);
```

Untuk menambahkan middleware pada aplikasi kita, kita dapat uncomment baris 1-3, kemudian menambahkan age middleware ke dalamnya.
Namun, karena kita hanya ingin menambahkan middleware pada route tertentu, kita akan menghapus comment pada baris 5-8, kemudian menambahkan middleware age di dalamnya.

<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/171f4854-016e-4809-8890-a0213e4ef0e3)

Kita dapat menambahkan middleware pada routes kita dengan menambahkan opsi middleware pada salah satu route, contohnya,

```
$router->get('/admin/home/', ['middleware' => 'age', function () {
    return 'Dewasa';
}]);

$router->get('/fail', function () {
    return 'Dibawah umur';
});
```

<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/8332c23a-7884-4bd7-98f0-52a8a6af7d12)
<br>ketika dalam middleware class, age dalam if diubah menjadi lebih dari 17
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/23655ea9-dfbc-4be6-be68-f4bcd266b61a)
<br>ketika dalam middleware class, age dalam if diubah menjadi kurang dari 17
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/16c3a240-b3a2-44fd-8ae9-8de77a8b4612)
