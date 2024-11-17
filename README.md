1. Persiapan
Untuk memulai membuat aplikasi CRUD sederhana, yang perlu disiapkan adalah
database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan
melalui XAMPP.
Menjalankan MySQL Server
Untuk menjalankan MySQL Server dari menu XAMPP Contol.
Mengakses MySQL Client menggunakan PHP MyAdmin
Pastikan webserver Apache dan MySQL server sudah dijalankan. Kemudian buka
melalui browser: http://localhost/phpmyadmin/ atau bisa di menekan tombol admin di XAMPP di colum MySQL

![Screenshot 2024-11-17 181420](https://github.com/user-attachments/assets/e9b378cd-66f5-48bf-b947-6036c0e8ce79)

2. Membuat Database: Studi Kasus Data Barang
   
    CREATE TABLE data_barang (
    id_barang int(10) auto_increment Primary Key,
    kategori varchar(30),
    nama varchar(30),
    gambar varchar(100),
    harga_beli decimal(10,0),
    harga_jual decimal(10,0),
    stok int(4)
    );
Membuat Create Table ini bisa di buat di phpMyAdmin atau bisa juga membuat di terminal
![Screenshot 2024-11-17 182210](https://github.com/user-attachments/assets/ed5818d1-00e7-4644-b122-c7f92b019ba7)

![Screenshot 2024-11-16 141950](https://github.com/user-attachments/assets/50cb66b1-7c21-40cd-873d-0586cc2b3c0d)

3. Kemudian Membuat Item data pada data_barang yang sebelumnya sudah dibuat
   Membuatnya bisa melalui phpMyAdmin dan juga bisa dibuat melalui terminal
dibuatnya dengan perintah INSERT INTO
  INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
  VALUES 
  ('Elektronik', 'Laptop MacBook', 'laptop_MacBook.jpg', 2000000, 2400000, 5),
  ('Elektronik', 'Laptop Lenovo', 'laptop_lenovo.jpg', 1000000, 1400000, 5),
  ('Elektronik', 'Laptop Asus ROG', 'laptop_asusrog.jpg', 1800000, 2300000, 5);

4. Kemudian untuk Menjalankan database ini ke sebuah website local Pertama kita harus membuat folder di htdocs caranya

buka File Explorer -> xampp -> htdocs -. buat folder di dalam htdocs dengan lab8_php_database.
![Screenshot 2024-11-16 142100](https://github.com/user-attachments/assets/bb2aba4a-1c35-42a3-9515-0e3adb427f17)

5. ketika sudah membuat folder kemudian kita cek foldernya sudah bisa tersambung dengan localhost kita di broser
Buka browser -> lalu ketik di colom pencarian -> localhost/lab8_php/database/

![Screenshot 2024-11-17 174209](https://github.com/user-attachments/assets/5ad68a27-5ff5-4555-a8a5-bfb3519241ae)

6. setelah bisa di buka localhostnya Kemudian kita harus menyambungkan database dengan website, agar table database bisa muncul di website.
pertama kita perlu memasukkan code koneksi di vscode, kemudian -> buka folder lab8_php_database -> buat file dengan nama koneksi.php

  <?php
  $host = "localhost";
  $user = "root";
  $pass = "";
  $db = "latihan1";
  $conn = mysqli_connect($host, $user, $pass, $db);
  if ($conn == false)
  {
  echo "Koneksi ke server gagal.";
  die();
  } #else echo "Koneksi berhasil";
  ?>

disini perlu di perhatikan di bagian $db = "latihan1" harus sama dengan nama database yang ingin ditampilkan di sebuah website. setelah itu kita perlu test databasenya benar-benar muncul di website dengan cara hapus '#' ini, kemudian kita ke website localhost/lab8_php_database/koneksi.php
  
![Screenshot 2024-11-16 142236](https://github.com/user-attachments/assets/0ef09a9f-8059-48f8-b849-21788aa6a4b7)
![Screenshot 2024-11-16 142615](https://github.com/user-attachments/assets/141b52df-22f7-4637-a854-30154e882594)

7. Setelah itu Jika ingin benar-bener ingin memasukkan table database di website localhost, kita perlu membuat code Html yang di dalamnya perlu di tambhakan code php untuk menyambungkan antara html dengan database
   
  <?php
  include("koneksi.php");
  // query untuk menampilkan data
  $sql = 'SELECT * FROM data_barang';
  $result = mysqli_query($conn, $sql);
  ?>
![Screenshot 2024-11-17 173245](https://github.com/user-attachments/assets/733b71e9-2df6-4813-98f5-0784738f9a38)

8.Seteah itu Jika ingin Menambahkan barang pada database melalui website kita perlu membuat file yang berbeda "tambah.php" yang di dalamnya menambahkan code html dengan cara harus menambahkan code php yang terhubung ke database

  <?php
  error_reporting(E_ALL);
  include_once 'koneksi.php';
  
  if (isset($_POST['submit'])) {
      $nama = $_POST['nama'];
      $kategori = $_POST['kategori'];
      $harga_jual = $_POST['harga_jual'];
      $harga_beli = $_POST['harga_beli'];
      $stok = $_POST['stok'];
      $file_gambar = $_FILES['file_gambar'];
      $gambar = null;
  
      if ($file_gambar['error'] == 0) {
          $filename = str_replace(' ', '_', $file_gambar['name']);
          $destination = dirname(__FILE__) . '/gambar/' . $filename;
          if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
              $gambar = 'gambar/' . $filename;
          }
      }
  
      $sql = 'INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar) ';
      $sql .= "VALUE ('{$nama}', '{$kategori}', '{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";
      $result = mysqli_query($conn, $sql);
      header('location: index.php');
  }
  ?>
  
![Screenshot 2024-11-17 175757](https://github.com/user-attachments/assets/0d1dbaf9-feb9-48a9-a872-dcb70b5d469a)
![Screenshot 2024-11-17 175807](https://github.com/user-attachments/assets/77716a75-6553-4d2f-8f79-c2e68ed0df02)
9. Jika ingin mengubah Data Barang Kita perlu membuat file ubah.php
yang di dalamnya html dan php yang suda terhubung dengan database caranya kita hanya perlu memasukkan code php di dalam html

  <?php
  error_reporting(E_ALL);
  include_once 'koneksi.php';
  
  if (isset($_POST['submit'])) {
      $id = $_POST['id'];
      $nama = $_POST['nama'];
      $kategori = $_POST['kategori'];
      $harga_jual = $_POST['harga_jual'];
      $harga_beli = $_POST['harga_beli'];
      $stok = $_POST['stok'];
      $file_gambar = $_FILES['file_gambar'];
      $gambar = null;
  
      if ($file_gambar['error'] == 0) {
          $filename = str_replace(' ', '_', $file_gambar['name']);
          $destination = dirname(__FILE__) . '/gambar/' . $filename;
          if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
              $gambar = 'gambar/' . $filename;
          }
      }
  
      $sql = 'UPDATE data_barang SET ';
      $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
      $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
      if (!empty($gambar)) $sql .= ", gambar = '{$gambar}' ";
      $sql .= "WHERE id_barang = '{$id}'";
  
      $result = mysqli_query($conn, $sql);
      header('location: index.php');
  }
  
  $id = $_GET['id'];
  $sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
  $result = mysqli_query($conn, $sql);
  if (!$result) die('Error: Data tidak tersedia');
  $data = mysqli_fetch_array($result);
  
  function is_select($var, $val) {
      return $var == $val ? 'selected="selected"' : false;
  }
  ?>

![Screenshot 2024-11-17 174520](https://github.com/user-attachments/assets/cbcac141-4fc2-464d-a3b4-487adbc0f9fb)
![Screenshot 2024-11-17 174531](https://github.com/user-attachments/assets/00b4f5f5-61a8-4c3c-90f2-9a811140bb6a)

10. Terahkir jika ingin menghapus databarang pada halaman website, kita perlu membuat file hapus.php yang didalamnya html dan php yang membuat perintah hapus pada database

  <?php
  include_once 'koneksi.php';
  $id = $_GET['id'];
  $sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
  $result = mysqli_query($conn, $sql);
  header('location: index.php');
  ?>

![Screenshot 2024-11-17 175415](https://github.com/user-attachments/assets/f8d33d82-1766-4a2e-94f4-b62fb2c1ebaa)
![Screenshot 2024-11-17 175424](https://github.com/user-attachments/assets/f9b278d0-7f81-4c34-86ae-9a85846abfd1)
