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

![Membuat Tabel](https://user-images.githubusercontent.com/81541764/120838475-0da8de80-c592-11eb-89ab-b3bffe7c841b.JPG)


## Menambahkan Data
~~~
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok) VALUES ('Elektronik', 'HP Samsung Android',
'hp_samsung.jpg', 2000000, 2400000, 5), ('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
~~~

![Menambahkan Data](https://user-images.githubusercontent.com/81541764/120838882-85770900-c592-11eb-98a3-09f61df16f80.jpg)


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

![index](https://user-images.githubusercontent.com/81541764/120838943-94f65200-c592-11eb-8613-b4c5440b7cfc.JPG)

## Menambah Data (Create)
Buat file baru dengan nama tambah.php
~~~
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit']))
{
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    if ($file_gambar['error'] == 0)
    {
    $filename = str_replace(' ', '_',$file_gambar['name']);
    $destination = dirname(__FILE__) .'/gambar/' . $filename;
    if(move_uploaded_file($file_gambar['tmp_name'], $destination))
    {
    $gambar = 'gambar/' . $filename;;
    }
    }
    $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
}

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Tambah Barang</title>
</head>
<body>
    <div class="container">
        <h1>Tambah Barang</h1>
        <div class="main">
        <form method="post" action="tambah.php" enctype="multipart/form-data">
    <div class="input">
    <label>Nama Barang</label>
    <input type="text" name="nama" />
    </div>
    <div class="input">
        <label>Kategori</label>
        <select name="kategori">
            <option value="Komputer">Komputer</option>
            <option value="Elektronik">Elektronik</option>
            <option value="Hand Phone">Hand Phone</option>
        </select>
    </div>
    <div class="input">
        <label>Harga Jual</label>
        <input type="text" name="harga_jual" />
    </div>
    <div class="input">
    <label>Harga Beli</label>
    <input type="text" name="harga_beli" />
    </div>
    <div class="input">
        <label>Stok</label>
        <input type="text" name="stok" />
    </div>
    <div class="input">
        <label>File Gambar</label>
        <input type="file" name="file_gambar" />
    </div>
    <div class="submit">
        <input type="submit" name="submit" value="Simpan" />
    </div>
</form>
</div>
</div>
</body>
</html>
~~~

![tambah barang](https://user-images.githubusercontent.com/81541764/120839207-dedf3800-c592-11eb-9e7a-b17a48280024.JPG)

## Mengubah Data (Update)
Buat file baru dengan nama ubah.php
~~~
<?php error_reporting(E_ALL);
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
    
    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
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
    if ($var == $val) return 'selected="selected"';
    return false;
}

?>

<!DOCTYPE html>
<html lang="en">
<head>
        <meta charset="UTF-8">
        <link href="style.css" rel="stylesheet" type="text/css" />
        <title>Ubah Barang</title>
</head>
<body>
<div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">
        <form method="post" action="ubah.php" enctype="multipart/form-data">    
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $data['nama'];?>" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>" />
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>" />
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?php echo $data['stok'];?>" />
            </div>
            <div class="input">
                <label>File Gambar</label> <input type="file" name="file_gambar" />
            </div>
            <div class="submit">
                <input type="hidden" name="id" value="<?php echo $data['id_barang'];?>" />
                <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>
~~~

![ubah](https://user-images.githubusercontent.com/81541764/120856079-7b143980-c5a9-11eb-95f2-4c683b6b38fb.JPG)


## Menghapus Data (Delete)
Buat file baru dengan nama hapus.php
~~~
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);

header('location: index.php');

?>
~~~

![hapus](https://user-images.githubusercontent.com/81541764/120856234-b7479a00-c5a9-11eb-89e3-307c9c3da75a.JPG)

![hapus_2](https://user-images.githubusercontent.com/81541764/120856248-bc0c4e00-c5a9-11eb-9984-cdd1c09149a3.JPG)

