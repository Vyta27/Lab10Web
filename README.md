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







