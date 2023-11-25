# Relasi One-to-Many dan Many-to-Many

## Langkah Praktikum

> [!NOTE]
> menggunakan aplikasi lumenapi pada modul 4

### Pembuatan Tabel

Tabel yang akan digunakan
<br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/7a06999c-9b3a-4bf9-bef8-fc929a4ece40)

1. Sebelum membuat migrasi database atau membuat tabel pastikan server database aktif kemudian pastikan sudah membuat database dengan nama `lumenpost`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/f54b2394-f838-43e7-b6b8-b9daf2669411)

2. Kemudian ubah konfigurasi database pada file .env menjadi seperti berikut
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/22a2e743-665d-4a65-9be5-f08762671068)

3. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file app.php pada folder bootstrap dan mengubah baris ini
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/a3dffa0a-f24a-4167-bf8a-7a9ec5a2c0a5)

4. Setelah itu jalankan command berikut untuk membuat file migration

   ```
   php artisan make:migration create_posts_table
   php artisan make:migration create_comments_table
   php artisan make:migration create_tags_table
   php artisan make:migration create_post_tag_table
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/a0373036-3886-473b-9569-9197e39f34c0)

5. Ubah fungsi up() pada file migrasi create_posts_table

   ```
   public function up()
   {
   Schema::create('posts', function (Blueprint $table) {
           $table->id();
           $table->timestamps();
           $table->string('content');
       });
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/93f296d5-0c53-4f0a-85d7-b00cce1b8eab)

6. Ubah fungsi up() pada file create_comments_table

   ```
   public function up()
   {
       Schema::create('comments', function (Blueprint $table) {
           $table->id();
           $table->timestamps();
           $table->string('review');
           $table->foreignId('postId')->unsigned();
       });
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/15c1b269-7325-486e-a970-8cc7ad07e4c8)

7. Ubah fungsi up() pada file create_tags_table

   ```
   public function up()
       {
           Schema::create('tags', function (Blueprint $table) {
               $table->id();
               $table->timestamps();
               $table->string('name');
           });
       }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/b47b211f-2e43-4d76-970f-0851bbc62d56)

8. Ubah fungsi up() pada file create_post_tag_table

   ```
   public function up()
       {
           Schema::create('post_tag', function (Blueprint $table) {
               $table->id();
               $table->timestamps();
               $table->foreignId('postId')->unsigned();
               $table->foreignId('tagId')->unsigned();
           });
       }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/9cc9039f-2ce0-4171-8933-abf756df9146)

9. Kemudian jalankan command `php artisan migrate`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/87516976-5a6f-4248-8666-458547ff6040)

### Pembuatan Model

1. Buatlah file dengan nama Post.php dan isi dengan baris kode berikut

   ```
   <?php
   namespace App\Models;
   use Illuminate\Database\Eloquent\Model;
   class Post extends Model
   {
       /**
       * The attributes that are mass assignable.
       *
       * @var string[]
       */
       protected $fillable = [
       'content'
       ];
       /**
       * The attributes excluded from the model's JSON form.
       *
       * @var string[]
       */
       protected $hidden = [];
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/fb962221-79d0-41fa-b64b-14fe0b95ba32)

2. Buatlah file dengan nama Comment.php dan isi dengan baris kode berikut

   ```
   <?php
   namespace App\Models;
   use Illuminate\Database\Eloquent\Model;
   class Comment extends Model
   {
       /**
       * The attributes that are mass assignable.
       *
       * @var string[]
       */
       protected $fillable = [
       'review'
       ];
       /**
       * The attributes excluded from the model's JSON form.
       *
       * @var string[]
       */
       protected $hidden = [];
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/513bd0bb-2fdd-4bec-b794-bb9905059a61)

3. Buatlah file dengan nama Tag.php dan isi dengan baris kode berikut

   ```
   <?php
   namespace App\Models;
   use Illuminate\Database\Eloquent\Model;
   class Tag extends Model
   {
       /**
       * The attributes that are mass assignable.
       *
       * @var string[]
       */
       protected $fillable = [
       'name'
       ];
       /**
       * The attributes excluded from the model's JSON form.
       *
       * @var string[]
       */
       protected $hidden = [];
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/c999a8c4-8202-4691-ad98-f0ed3c0f4118)

### Relasi One-to-Many

1. Tambahkan fungsi `comments()` pada file Post.php

   ```
   <?php
   namespace App\Models;
   use Illuminate\Database\Eloquent\Model;
   class Post extends Model
   {
       ...
       // fungsi comments
       public function comments()
       {
           return $this->hasMany(Comment::class, 'postId');
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/56d1dbe8-accf-4314-baa8-cedf228c8b82)

2. Tambahkan fungsi `post()` dan atribut postId pada $fillable pada file Comment.php

   ```
   <?php
   namespace App\Models;
   use Illuminate\Database\Eloquent\Model;
   class Comment extends Model
   {
       ...
       protected $fillable = [
           'review',
           'postId' // atribut postId
       ];
       /**
       * The attributes excluded from the model's JSON form.
       *
       * @var string[]
       */
       protected $hidden = [];
       public function post()
       {
           return $this->belongsTo(Post::class, 'postId');
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/8e7bac29-3b6c-4ace-b727-40e68370833c)

3. Buatlah file PostController.php dan isilah dengan baris kode berikut

   ```
   <?php
   namespace App\Http\Controllers;
   use App\Models\Post;
   use Illuminate\Http\Request;
   class PostController extends Controller
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
       //
       public function createPost(Request $request)
       {
           $post = Post::create([
               'content' => $request->content,
           ]);
           return response()->json([
               'success' => true,
               'message' => 'New post created',
               'data' => [
               'post' => $post
               ]
           ]);
       }
       public function getPostById(Request $request)
       {
           $post = Post::find($request->id);
           return response()->json([
               'success' => true,
               'message' => 'All post grabbed',
               'data' => [
               'post' => [
               'id' => $post->id,
               'content' => $post->content,
               'comments' => $post->comments,
               ]
               ]
           ]);
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/31b40873-358a-42bb-8177-b270cceb5544)

4. Buatlah file CommentController.php dan isilah dengan baris kode berikut

   ```
   <?php
   namespace App\Http\Controllers;
   use App\Models\Comment;
   use Illuminate\Http\Request;
   class CommentController extends Controller
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
       //
       public function createComment(Request $request)
       {
           $comment = Comment::create([
               'review' => $request->review,
               'postId' => $request->postId,
           ]);
           return response()->json([
               'success' => true,
               'message' => 'New comment created',
               'data' => [
               'comment' => $comment
               ]
           ]);
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/e84dd6f0-5613-47e6-9a1d-c1b7dfd5fb3f)

5. Tambahkan baris berikut pada routes/web.php

   ```
   $router->group(['prefix' => 'posts'], function () use ($router) {
       $router->post('/', ['uses' => 'PostController@createPost']);
       $router->get('/{id}', ['uses' => 'PostController@getPostById']);
   });
   $router->group(['prefix' => 'comments'], function () use ($router) {
       $router->post('/', ['uses' => 'CommentController@createComment']);
   });
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/3dcac07b-8eb3-43f4-885e-c44ffe8c5b1f)

6. Buatlah satu post menggunakan Postman
   POST /posts
   `content:Anak bunda sehat selalu`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/e0215b5f-002f-4c59-9de0-1c0fae601da3)

7. Buatlah satu comment menggunakan Postman
   POST /comments
   `review:MasyaAllah bund`
   `PostId:1`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/edf49b95-9962-4724-b11d-593bf9ad6d20)

8. Tampilkan post menggunakan Postman
   GET /posts/1
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/307a83e6-d807-44f3-a12e-549eda343645)

### Relasi Many-to-Many

1. Tambahkan fungsi tags() pada file Post.php

   ```
   <?php
   namespace App\Models;
   use Illuminate\Database\Eloquent\Model;
   class Post extends Model
   {
       ...
       public function tags()
       {
           return $this->belongsToMany(Tag::class, 'post_tag', 'postId', 'tagId');
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/1cab4d5e-8154-46a3-9b11-8c25a2c290a4)

2. Tambahkan fungsi posts() pada file Tag.php

   ```
   <?php
   namespace App\Models;
   use Illuminate\Database\Eloquent\Model;
   class Tag extends Model
   {
       ...
       public function posts()
       {
           return $this->belongsToMany(Post::class, 'post_tag', 'tagId', 'postId');
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/aa68d356-5439-4160-98cf-cdb69207b66b)

3. Buatlah file TagController.php dan isilah dengan baris kode berikut

   ```
   <?php
   namespace App\Http\Controllers;
   use App\Models\Tag;
   use Illuminate\Http\Request;
   class TagController extends Controller
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
       //
       public function createTag(Request $request)
       {
           $tag = Tag::create([
               'name' => $request->name
           ]);
           return response()->json([
               'success' => true,
               'message' => 'New tag created',
               'data' => [
               'tag' => $tag
               ]
           ]);
       }
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/221f0557-4c2c-4ff1-a9f1-747eaededa8d)

4. Tambahkan fungsi addTag dan response tags pada PostController.php

   ```
   public function getPostById(Request $request)
   {
       $post = Post::find($request->id);
       return response()->json([
           'success' => true,
           'message' => 'All post grabbed',
           'data' => [
           'post' => [
           'id' => $post->id,
           'content' => $post->content,
           'comments' => $post->comments,
           'tags' => $post->tags, //response tags
           ]
           ]
       ]);
   }
   public function addTag(Request $request)
   {
       $post = Post::find($request->id);
       $post->tags()->attach($request->tagId);
       return response()->json([
           'success' => true,
           'message' => 'Tag added to post',
       ]);
   }
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/972f5bf0-60c5-43b9-917d-d1de55a6ee28)

5. Tambahkan baris berikut pada routes/web.php

   ```
   $router->group(['prefix' => 'posts'], function () use ($router) {
       $router->post('/', ['uses' => 'PostController@createPost']);
       $router->get('/{id}', ['uses' => 'PostController@getPostById']);
       $router->put('/{id}/tag/{tagId}', ['uses' => 'PostController@addTag']); //tambahkan
   });
   ...
   $router->group(['prefix' => 'tags'], function () use ($router) {
       $router->post('/', ['uses' => 'TagController@createTag']);
   });
   ```

   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/21e41984-b310-425f-bdf5-211197f4bfb8)

6. Buatlah satu tag menggunakan Postman
   POST /tags
   `name:Kenangan`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/3800e6d8-881e-4154-a30d-8fd81bd82f3d)


7. Tambahkan tag Kenangan pada post “Anak bunda sehat selalu”
   PUT /posts/1/tag/1
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/02507ce9-4c29-4e1a-99a3-a9a08a91f762)

8. Tampilkan post “Anak bunda sehat selalu” menggunakan Postman
   GET /posts/1
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/147d96d7-0bf9-4242-993f-f1984d413c74)

9. Buatlah postingan baru menggunakan Postman
   POST /posts
   `content: Tamasya di Singapore`
   <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d2dfe07f-4705-4cda-89a5-0ca4350a5bc7)

10. Tambahkan tag "kenangan" pada postingan “Tamasya di Singapore”
    PUT /posts/2/tag/1
    <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/47aa2db1-1bfb-43a3-a0d9-6c9af7a49850)

11. Buatlah tag "gembira" menggunakan Postman
    POST /tags
    `name:gembira`
    <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/d4e769ca-93cb-4611-ac2c-ab88ec80a757)

12. Tambahkan tag "gembira” pada postingan "Tamasya di Singapore”
    PUT /posts/2/tag/2
    <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/f448440a-7167-4f40-a12d-939601ecc72b)

13. Tampilkan post pertama
    GET /posts/1
    <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/0ea1e4a0-edf8-46fe-a752-fddb63ef974d)

14. Tampilkan post kedua
    GET /posts/2
    <br>![image](https://github.com/Edran32/Praktikum_Pemin/assets/50135710/54cc5646-0b4e-411c-8bfb-013506e74ed6)
