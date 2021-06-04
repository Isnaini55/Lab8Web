# Lab8Web

# Praktikum 8

# Pemograman WEB

~~~
Nama  : Isnaini Rizkyana
NIM   : 311910254
Kelas : TI.19.C1
~~~
## Langkah-langkah Praktikum

## Persiapan
Untuk memulai membuat aplikasi CRUD sederhana, yang perlu disiapkan adalah database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan melalui XAMPP.

## Menjalankan MySQL Server
Untuk menjalankan MySQL Server dari menu XAMPP Contol.
![Xampp](https://user-images.githubusercontent.com/81541764/120689653-86defd80-c4ce-11eb-9e17-7737f8fbff17.png)

## Mengakses MySQL Client menggunakan PHP MyAdmin
Pastikan webserver Apache dan MySQL server sudah dijalankan. Kemudian buka melalui browser: http://localhost/phpmyadmin/

## Membuat Database: Studi Kasus Data Barang
![Membuat Database Studi Kasus Data Barang](https://user-images.githubusercontent.com/81541764/120700998-92392580-c4dc-11eb-9ee1-c518731a5ab1.JPG)

## Membuat Database
~~~
CREATE DATABASE latihan1;
~~~
![Membuat Database](https://user-images.githubusercontent.com/81541764/120708688-17750800-c4e6-11eb-9438-4923acca5428.JPG)

## Membuat Tabel
~~~
CREATE TABLE data_barang (
  id_barang int(10) auto_increment Primary Key,
  kategori varchar(30),
  nama varchar(30),
  gambar varchar(100),
  harga_beli decimal(10,0),
  harga_jual decimal(10,0),
  stok int(4)
);
~~~
![Membuat Tabel](https://user-images.githubusercontent.com/81541764/120709012-86eaf780-c4e6-11eb-86a9-39619a505022.JPG)

## Menambahkan Data
~~~
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok) VALUES ('Elektronik', 'HP Samsung Android',
'hp_samsung.jpg', 2000000, 2400000, 5), ('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
~~~
![Menambahkan Data](https://user-images.githubusercontent.com/81541764/120709828-8dc63a00-c4e7-11eb-91c5-6fa5366116be.JPG)

## Membuat Program CRUD
Buat folder lab8_php_database pada root directory web server (d:\xampp\htdocs)

![lab8_database](https://user-images.githubusercontent.com/81541764/120710002-cf56e500-c4e7-11eb-98c1-75c7573a1286.JPG)

Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL:
http://localhost/lab8_php_database/

![localhost](https://user-images.githubusercontent.com/81541764/120710367-42f8f200-c4e8-11eb-9b0d-586eae21e220.JPG)

## Membuat file koneksi database
Buat file baru dengan nama koneksi.php
~~~
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
~~~

Buka melalui browser untuk menguji koneksi database (untuk menyampilkan pesan koneksi berhasil, uncomment pada perintah echo “koneksi berhasil”;
![koneksi](https://user-images.githubusercontent.com/81541764/120833109-9bcd9680-c58b-11eb-8551-70a0b52c46bf.JPG)

## Membuat file index untuk menampilkan data (Read)
Buat file baru dengan nama index.php
~~~
<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <div class="main">
            <table>
            <tr>
                <th>Gambar</th>
                <th>Nama Barang</th>
                <th>Katagori</th>
                <th>Harga Jual</th>
                <th>Harga Beli</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>
            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?>
            <tr>
                <td><img src="gambar/<?= $row['gambar'];?>" alt="<?=
$row['nama'];?>"></td>
                <td><?= $row['nama'];?></td>
                <td><?= $row['kategori'];?></td>
                <td><?= $row['harga_beli'];?></td>
                <td><?= $row['harga_jual'];?></td>
                <td><?= $row['stok'];?></td>
                <td><?= $row['id_barang'];?></td>
            </tr>
            <?php endwhile; else: ?>
            <tr>
                <td colspan="7">Belum ada data</td>
            </tr>
            <?php endif; ?>
            </table>
        </div>
    </div>
</body>
</html>
~~~

![index](https://user-images.githubusercontent.com/81541764/120837370-cff78600-c590-11eb-867b-9c07a29fea99.JPG)


