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
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/005dbe3c-8eef-46bb-a071-f1aca744aa6d" />


