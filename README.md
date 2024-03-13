# PBF
# Refina Inayatul Putri
# 220302041

## Welcome to CodeIgniter4
  ### Introduction
  CodeIgniter 4 adalah sebuah framework pengembangan aplikasi web berbasis PHP yang bersifat open source. Framework ini dirancang untuk mempercepat proses pengembangan aplikasi web dengan menyediakan library yang dibutuhkan.
  ### Server Requirements(https://codeigniter.com/user_guide/intro/requirements.html#server-requirements)
  PHP yang diperlukan PHP versi 7.4 atau yang lebih baru, dengan ekstensu PHP diaktifkan:
  - internasional
  - mbstring
  - json
## Installation(https://codeigniter.com/user_guide/installation/index.html#installation)
  ### Composer Installation(https://codeigniter.com/user_guide/installation/installing_composer.html#composer-installation)
  Pastikan composer sudah terinstal sebelumnya
  1. Buka terminal atau command prompt dan navigasi ke direktori tempat peletakan proyek baru
  2. Jalankan perintah berikut untuk membuat project
  ``` shell composer create-project codeigniter4/appstarter nama-project```
    Untuk melakukan update jalankan perintah berikut ini:
    ```shell composer update```
    - Pro
      Instalasi sederhana dan mudah untuk diperbaharui
    - Kontra
     Memperlukan pemeriksaan perubahan file di ruang proyek (root, app, public, writable)
    - Struktur Folder:
      - app (Berisi file yang digunakan pada project)
      - public (Berisi file assets yang dapat diakses secara publik melalui browser)
      - test (Berisi file yang digunakan untuk melakukan pengujian)
      - writable (Berisi file logs, cache, dan file lainnya yang diupload user)
      - system (Berisi file yang digunakan framework)
   ### Instalasi Manual(https://codeigniter.com/user_guide/installation/installing_manual.html#manual-installation)
    1. Buka situs web resmi CodeIgniter: https://www.codeigniter.com/download
    2. Pilih versi CodeIgniter yang ingin Anda instal.
    3. Unduh file ZIP CodeIgniter.
    4. Buka file ZIP yang telah diunduh.
    5. Ekstrak file ZIP ke direktori yang Anda inginkan.
    6. Buka terminal atau command prompt dan navigasi ke direktori tempat peletakan proyek baru
    7. Jalankan perintah berikut untuk membuat project
  ``` shell composer create-project codeigniter4/appstarter nama-project```
    Untuk melakukan update jalankan perintah berikut ini:
    ```shell composer update```
      - Pro
        Hanya perlu mendownload dan menjalankan
      - Kontra
        Memperlukan perubahan file di ruang proyek (root, app, public, tests, writable)
      - Struktur Folder
         - app (Berisi file yang digunakan pada project)
         - public (Berisi file assets yang dapat diakses secara publik melalui browser)
         - test (Berisi file yang digunakan untuk melakukan pengujian)
         - writable (Berisi file logs, cache, dan file lainnya yang diupload user)
         - system (Berisi file yang digunakan framework)
## Build Your First Application(https://codeigniter.com/user_guide/tutorial/index.html#build-your-first-application)
   ### Getting UP and Running
    - Setel CI_ENVIRONMENTke development dalam file .env untuk memanfaatkan alat debugging yang disediakan.
      ```shell
        CI_ENVIRONMENT = development
      ```
      - Running Development Server 
    ```shel
      php spark serve```
   ### Static Pages
   1. Setting Routing Rules
   Buka file di ```shell app/Config/Routes.php ```
     ```shell
        <?php
  
        use CodeIgniter\Router\RouteCollection;
    
       /**
       * @var RouteCollection $routes
       */
      $routes->get('/', 'Home::index');
    ```
  2. Buat Controller
  Buat file di ```shell app/Controllers/Pages.php```.
        ```shell
      <?php
      
      namespace App\Controllers;
      
      class Pages extends BaseController
      {
          public function index()
          {
              return view('welcome_message');
          }
      
          public function view($page = 'home')
          {
              // ...
          }
      }
      ```
3. Buat Views
   Buat header di ``app/Views/templates/header.php``
   ``` shell
       <!doctype html>
    <html>
    <head>
        <title>CodeIgniter Tutorial</title>
    </head>
    <body>
    
        <h1><?= esc($title) ?></h1>
    ```
   4. Halaman Lengkap::view()Metode
             ```shell <?php
        
        namespace App\Controllers;
        
        use CodeIgniter\Exceptions\PageNotFoundException; // Add this line
        
        class Pages extends BaseController
        {
            // ...
        
            public function view($page = 'home')
            {
                if (! is_file(APPPATH . 'Views/pages/' . $page . '.php')) {
                    // Whoops, we don't have a page for that!
                    throw new PageNotFoundException($page);
                }
        
                $data['title'] = ucfirst($page); // Capitalize the first letter
        
                return view('templates/header', $data)
                    . view('pages/' . $page)
                    . view('templates/footer');
            }
        }
        ```
  ### News section(https://codeigniter.com/user_guide/tutorial/news_section.html#news-section)
   1. Create a Database to Work with
      Buatlah database ci4tutorial di MySQL
          ```shell
          CREATE TABLE news (
        id INT UNSIGNED NOT NULL AUTO_INCREMENT,
        title VARCHAR(128) NOT NULL,
        slug VARCHAR(128) NOT NULL,
        body TEXT NOT NULL,
        PRIMARY KEY (id),
        UNIQUE slug (slug)
    );
    ```
    - Catatan Seeds:
    ```shell
    INSERT INTO news VALUES
    (1,'Elvis sighted','elvis-sighted','Elvis was sighted at the Podunk internet cafe. It looked like he was writing a CodeIgniter app.'),
    (2,'Say it isn\'t so!','say-it-isnt-so','Scientists conclude that some programmers have a sense of humor.'),
    (3,'Caffeination, Yes!','caffeination-yes','World\'s largest coffee shop open onsite nested coffee shop for staff only.');
    ```
    2. Connect to Your Database
    Pastikan telah mengkonfigurasi database dengan benar:
    ```shell
        database.default.hostname = localhost
        database.default.database = ci4tutorial
        database.default.username = root
        database.default.password = root
        database.default.DBDriver = MySQLi
    ```
    3. Create NewsModel
       Buka direktori app/Models dan buat file baru bernama NewsModel.php dan tambahkan kode berikut:
       ``` shell
          <?php
          
          namespace App\Models;
          
          use CodeIgniter\Model;
          
          class NewsModel extends Model
          {
              protected $table = 'news';
          }
          ```
   4. Add NewsModel::getNews() Method
      Tambahkan kode berikut ke model Anda:
      ``` shell
      public function getNews($slug = false)
     {
        if ($slug === false) {
       return $this->findAll();
     }
    
       return $this->where(['slug' => $slug])->first();
     }
   ```
  5. Adding Routing Rules
   Ubah file app/Config/Routes.php Anda , sehingga terlihat seperti berikut:
    ``` shell
    <?php
    
    // ...
    
    use App\Controllers\News; // Add this line
    use App\Controllers\Pages;
    
    $routes->get('news', [News::class, 'index']);           // Add this line
    $routes->get('news/(:segment)', [News::class, 'show']); // Add this line
    
    $routes->get('pages', [Pages::class, 'index']);
    $routes->get('(:segment)', [Pages::class, 'view']);
    ```
  6. Create News Controller
  Buat pengontrol baru di app/Controllers/News.php:
    ``` shell
    <?php
    
    namespace App\Controllers;
    
    use App\Models\NewsModel;
    
    class News extends BaseController
    {
        public function index()
        {
            $model = model(NewsModel::class);
    
            $data['news'] = $model->getNews();
        }
    
        public function show($slug = null)
        {
            $model = model(NewsModel::class);
    
            $data['news'] = $model->getNews($slug);
        }
    }
    ```
7. Complete News::index()Method
 Ubah index()metodenya menjadi seperti ini:
    ``` shell
    <?php
    
    namespace App\Controllers;
    
    use App\Models\NewsModel;
    
    class News extends BaseController
    {
        public function index()
        {
            $model = model(NewsModel::class);
    
            $data = [
                'news'  => $model->getNews(),
                'title' => 'News archive',
            ];
    
            return view('templates/header', $data)
                . view('news/index')
                . view('templates/footer');
        }
    
        // ...
    }
    ```
8. Create news/index View File
Buat app/Views/news/index.php dan tambahkan potongan kode berikutnya:
``` shell
<h2><?= esc($title) ?></h2>

<?php if (! empty($news) && is_array($news)): ?>

    <?php foreach ($news as $news_item): ?>

        <h3><?= esc($news_item['title']) ?></h3>

        <div class="main">
            <?= esc($news_item['body']) ?>
        </div>
        <p><a href="/news/<?= esc($news_item['slug'], 'url') ?>">View article</a></p>

    <?php endforeach ?>

<?php else: ?>

    <h3>No News</h3>

    <p>Unable to find any news for you.</p>

<?php endif ?>
```
9. Complete News::show()Method
   Kembali ke Newspengontrol dan perbarui show()metode dengan yang berikut:
   ``` shell
   <?php

namespace App\Controllers;

use App\Models\NewsModel;
use CodeIgniter\Exceptions\PageNotFoundException;

class News extends BaseController
{
    // ...

    public function show($slug = null)
    {
        $model = model(NewsModel::class);

        $data['news'] = $model->getNews($slug);

        if (empty($data['news'])) {
            throw new PageNotFoundException('Cannot find the news item: ' . $slug);
        }

        $data['title'] = $data['news']['title'];

        return view('templates/header', $data)
            . view('news/view')
            . view('templates/footer');
    }
}
```
10. Create news/view View File
Satu-satunya hal yang perlu dilakukan adalah membuat tampilan terkait di app/Views/news/view.php . Letakkan kode berikut di file ini.
``` shell
<h2><?= esc($news['title']) ?></h2>
<p><?= esc($news['body']) ?></p>
```
Arahkan browser Anda ke halaman “news”, yaitu localhost:8080/news , Anda akan melihat daftar item berita, yang masing-masing memiliki link untuk menampilkan satu artikel saja.


### Create a Form
Buat tampilan baru di app/Views/news/create.php :
``` shell
<h2><?= esc($title) ?></h2>

<?= session()->getFlashdata('error') ?>
<?= validation_list_errors() ?>

<form action="/news" method="post">
    <?= csrf_field() ?>

    <label for="title">Title</label>
    <input type="input" name="title" value="<?= set_value('title') ?>">
    <br>

    <label for="body">Text</label>
    <textarea name="body" cols="45" rows="4"><?= set_value('body') ?></textarea>
    <br>

    <input type="submit" name="submit" value="Create news item">
</form>
```
## CodeIgniter4 Overview(https://codeigniter.com/user_guide/concepts/index.html#codeigniter4-overview)
### MVC(https://codeigniter.com/user_guide/concepts/mvc.html#models-views-and-controllers)
MVC (Model-View-Controller) adalah pola arsitektur umum yang digunakan untuk pengembangan aplikasi web.  Ini bertujuan untuk memisahkan berbagai aspek fungsional dari sebuah aplikasi web menjadi tiga komponen utama:
1. Model
Komponen Model menangani logika bisnis aplikasi Anda.
Ia bertanggung jawab untuk mengakses dan memanipulasi data aplikasi, sering kali berinteraksi dengan database atau sumber data lainnya.
Model biasanya tidak berinteraksi langsung dengan antarmuka pengguna.
2. View
Komponen View berfokus pada presentasi data kepada pengguna.
Ini biasanya berupa file template atau kode yang menghasilkan HTML, CSS, dan JavaScript yang ditampilkan di browser pengguna.
View menerima data yang disiapkan oleh Controller dan memformatnya dengan cara yang sesuai untuk tampilan.
3. Controller
Komponen Controller bertindak sebagai perantara antara Model dan View.
Ia menerima permintaan pengguna (misalnya, klik tombol, formulir yang dikirimkan), memprosesnya, dan berinteraksi dengan Model untuk mengambil data yang diperlukan.
Controller kemudian menyiapkan data tersebut untuk diteruskan ke View yang sesuai untuk ditampilkan kepada pengguna.
## General Topics
### Helpers
Helpers seperti namanya, membantu Anda mengerjakan tugas. Setiap file pembantu hanyalah kumpulan fungsi dalam kategori tertentu. Ada Pembantu URL , yang membantu dalam membuat tautan, ada Pembantu Formulir yang membantu Anda membuat elemen formulir, Pembantu Tes melakukan berbagai rutinitas pemformatan teks, Pembantu Cookie mengatur dan membaca cookie, Pembantu Sistem File membantu Anda menangani file, dll.
1. Loading Helpers
   Buat di file name_helper.php .
   ``` shell
   <?php

helper('name');
```
2. Creating Custom Helpers
Misalnya, untuk membuat info helper, Anda akan membuat file bernama app/Helpers/info_helper.php , dan menambahkan fungsi ke file:
``` shell
<?php

// app/Helpers/info_helper.php
use CodeIgniter\CodeIgniter;

/**
 * Returns CodeIgniter's version.
 */
function ci_version(): string
{
    return CodeIgniter::CI_VERSION;
}
```
## Controllers and Routing
Controllers, atau Pengendali dalam bahasa Indonesia, adalah komponen inti dalam kerangka kerja Model-View-Controller (MVC) yang banyak digunakan untuk pengembangan web. Mereka bertindak sebagai perantara antara pengguna dan aplikasi web, menangani permintaan pengguna dan menghasilkan respons yang sesuai.
### Constructor
``` shell
<?php

namespace App\Controllers;

use CodeIgniter\HTTP\RequestInterface;
use CodeIgniter\HTTP\ResponseInterface;
use Psr\Log\LoggerInterface;

class Product extends BaseController
{
    public function initController(
        RequestInterface $request,
        ResponseInterface $response,
        LoggerInterface $logger
    ) {
        parent::initController($request, $response, $logger);

        // Add your code here.
    }

    // ...
}
```
### Helpers
``` shell
<?php

namespace App\Controllers;

class MyController extends BaseController
{
    protected $helpers = ['url', 'form'];
}
```
## Building Responses
### Views
1. Creating a View
   Buat file bernama blog_view.php dan letakkan ini di dalamnya:
   ``` shell <html>
    <head>
        <title>My Blog</title>
    </head>
    <body>
        <h1>Welcome to my Blog!</h1>
    </body>
</html>
```
Kemudian simpan file di direktori app/Views Anda .

2. Menampilkan Tampilan
``` shell
return view('name');
```
3. Sekarang, buat file bernama Blog.php di direktori app/Controllers , dan letakkan ini di dalamnya:
   ``` shell
   <?php

namespace App\Controllers;

class Blog extends BaseController
{
    public function index()
    {
        return view('blog_view');
    }
}
```
3. Buka file perutean yang terletak di app/Config/Routes.php , dan cari “Definisi Rute”. Tambahkan kode berikut:
``` shell
use App\Controllers\Blog;

$routes->get('blog', [Blog::class, 'index']);
```







      
      

