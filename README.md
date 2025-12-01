Nama  : Navyta Budi Yulia

NIM   : 312410184

Kelas : TI.24.A2

# Lab10Web

## Langkah 1 
- Membuat Folder pada XAMPP :
```
htdocs/
└── lab10_php_oop/
```
Semua file praktikum diletakkan di folder ini.

## Langkah 2
- Membuat Class Sederhana (mobil.php)
```
<?php
class Mobil {
    private $warna;
    private $merk;
    private $harga;

    public function __construct() {
        $this->warna = "Biru";
        $this->merk = "BMW";
        $this->harga = 10000000;
    }

    public function gantiWarna($warnaBaru) {
        $this->warna = $warnaBaru;
    }

    public function tampilWarna() {
        echo "Warna mobilnya: " . $this->warna;
    }
}

$a = new Mobil();
$b = new Mobil();

echo "<b>Mobil pertama</b><br>";
$a->tampilWarna();
echo "<br>Mobil pertama ganti warna<br>";
$a->gantiWarna("Merah");
$a->tampilWarna();

echo "<br><b>Mobil kedua</b><br>";
$b->gantiWarna("Hijau");
$b->tampilWarna();
?>
```
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/70670b24-6a19-40e2-a948-b81764563ac7" />

- class ini memiliki :
    - Constructor → menginisialisasi nilai awal
    - Method gantiWarna() → mengubah warna
    - Method tampilWarna() → menampilkan warna

## Langkah 3
- Membuat Class Library untuk Form (form.php)
```
<?php
class Form {
    private $fields = array();
    private $action;
    private $submit = "Submit Form";
    private $jumField = 0;

    public function __construct($action, $submit) {
        $this->action = $action;
        $this->submit = $submit;
    }

    public function addField($name, $label) {
        $this->fields[$this->jumField]['name'] = $name;
        $this->fields[$this->jumField]['label'] = $label;
        $this->jumField++;
    }

    public function displayForm() {
        echo "<form action='".$this->action."' method='POST'>";
        echo "<table width='100%' border='0'>";
        
        for ($j = 0; $j < count($this->fields); $j++) {
            echo "<tr><td align='right'>".$this->fields[$j]['label']."</td>";
            echo "<td><input type='text' name='".$this->fields[$j]['name']."'></td></tr>";
        }

        echo "<tr><td colspan='2'>";
        echo "<input type='submit' value='".$this->submit."'></td></tr>";
        echo "</table>";
        echo "</form>";
    }
}
?>
```

- Fungsi class ini:
    - Menyimpan daftar input (fields)
    - Menampilkan form otomatis dalam bentuk tabel
    - Mengurangi penulisan kode HTML berulang
- Class memiliki fungsi:
    - addField() → menambahkan input text
    - displayForm() → menampilkan form
- File ini tidak dapat dijalankan langsung, harus dipanggil oleh file lain menggunakan include.

## Langkah 4
- Menampilkan Form dengan form_input.php
- Berfungsi untuk menampilkan form ke browser.
```
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

include "form.php";
?>

<!DOCTYPE html>
<html>
<head>
    <title>Mahasiswa</title>

    <!-- Panggil CSS cute -->
    <link rel="stylesheet" href="style.css">

</head>
<body>

<!-- Wrapper lucu -->
<div class="form-wrapper">

<h3>Silahkan isi form berikut ini ♡</h3>

<?php
// gunakan form seperti biasa
$form = new Form("simpan.php", "Simpan");
$form->addField("txtnim", "Nim");
$form->addField("txtnama", "Nama");
$form->addField("txtalamat", "Alamat");

$form->displayForm();
?>

</div>

</body>
</html>
```
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/a0f20ee7-f10a-412f-8d5c-5a6513188c50" />

- Langkah :
    - Memanggil form.php
    - Membuat instance:
```
$form = new Form("simpan.php", "Simpan");
```
    - Menambah input:
```
$form->addField("txtnim", "Nim");
$form->addField("txtnama", "Nama");
$form->addField("txtalamat", "Alamat");
```
    - Menampilkan form.
    - Form ini nantinya akan mengarahkan data ke simpan.php.

## Langkah 5
- Membuat Koneksi Database Menggunakan Class (database.php)
```
<?php
class Database {
    protected $conn;

    public function __construct() {
        // KONEKSI FIX LANGSUNG (TIDAK PAKAI CONFIG)
        $host = "127.0.0.1";
        $user = "root";
        $pass = "";
        $db   = "lab10web";
        $port = 3307;

        $this->conn = @new mysqli($host, $user, $pass, $db, $port);

        if ($this->conn->connect_error) {
            die("Koneksi database gagal: " . $this->conn->connect_error);
        }
    }

    public function insert($table, $data) {
        $cols = [];
        $vals = [];

        foreach ($data as $key => $val) {
            $cols[] = $key;
            $vals[] = "'$val'";
        }

        $cols = implode(",", $cols);
        $vals = implode(",", $vals);

        $sql = "INSERT INTO $table ($cols) VALUES ($vals)";
        return $this->conn->query($sql);
    }

    public function query($sql) {
        return $this->conn->query($sql);
    }
}
?>
```

- Class ini memiliki method:
    - insert()
    - query()
- Tujuan modularisasi adalah agar seluruh koneksi dilakukan di satu tempat.

## Langkah 6
- Di phpMyAdmin, dibuat database:
```
CREATE DATABASE lab10web;
```
- Lalu tabel:
```
CREATE TABLE mahasiswa (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nim VARCHAR(20),
    nama VARCHAR(100),
    alamat VARCHAR(255)
);
```
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/f55c8d07-4e16-4d6a-8134-41dc312e9a06" />

- Tabel ini menerima data dari form.

## Langkah 7 
- Membuat Halaman simpan.php
```
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

include "database.php";

$db = new Database();

$data = [
    'nim'    => $_POST['txtnim'],
    'nama'   => $_POST['txtnama'],
    'alamat' => $_POST['txtalamat']
];

$insert = $db->insert("mahasiswa", $data);
?>

<!DOCTYPE html>
<html>
<head>
    <title>Hasil Simpan</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>

<div class="form-wrapper">

<?php if ($insert): ?>
    <h3>✨ Data Berhasil Disimpan! ✨</h3>

    <p style="text-align:center; color:#ff62b0; font-size:16px;">
        Terima kasih sudah mengisi data ya ♡
    </p>

    <div style="text-align:center; margin-top:15px;">
        <a href="form_input.php" 
           style="display:inline-block; padding:10px 20px; background:#ff9ad6; color:white; 
                  text-decoration:none; border-radius:12px; font-weight:bold;">
            Kembali ke Form
        </a>
    </div>

<?php else: ?>
    <h3 style="color:red;">Gagal menyimpan data :(</h3>

    <p style="text-align:center; color:#ff62b0; font-size:16px;">
       Silakan periksa kembali database atau koneksinya ya ♡
    </p>

    <div style="text-align:center; margin-top:15px;">
        <a href="form_input.php" 
           style="display:inline-block; padding:10px 20px; background:#ff9ad6; color:white; 
                  text-decoration:none; border-radius:12px; font-weight:bold;">
            Kembali ke Form
        </a>
    </div>

<?php endif; ?>

</div>

</body>
</html>
```
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/68f64cd4-a67e-4148-8df2-0107c07c27f8" />

- File simpan.php bertugas:
    - menerima data POST dari form
    - menyimpan data ke tabel mahasiswa
    - menampilkan pesan berhasil / gagal
    - menggunakan tema CSS lucu pastel



















