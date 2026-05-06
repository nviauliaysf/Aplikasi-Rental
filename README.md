
# 🚗 Aplikasi Manajemen Rental Kendaraan Berbasis Desktop

## 👥 Kelompok 8

1. Ahmad Raid Iftinan (01)
2. Muhamad Faisal Akbar (20)
3. Novi Aulia Yusuf (27)

---

## 📝 Deskripsi Aplikasi
Aplikasi ini bertujuan untuk mempermudah pengelolaan rental kendaraan agar lebih cepat, mudah, dan akurat. Dengan sistem ini, risiko kesalahan manusia dalam perhitungan biaya dan pendataan unit dapat diminimalisir.

---

## 👩🏻‍💻 Peran Pengguna (Roles)

Aplikasi ini memiliki tiga peran utama dengan fungsi spesifik untuk mendukung operasional rental

### 1. Admin
Memiliki kontrol penuh terhadap manajemen data internal sistem.
* **Mengelola data petugas:** Menambah, mengubah, atau menghapus akun petugas
* **Menambah data kendaraan:** Memasukkan unit baru ke dalam sistem.
* **Mengelola data kendaraan:** Melakukan update informasi atau status kendaraan.
* **Melihat Data Pelanggan:** Memantau seluruh basis data pelanggan yang terdaftar.

### 2. Petugas
Bertanggung jawab atas jalannya transaksi harian.
* **Menginput pesanan pelanggan:** Mencatat transaksi penyewaan baru.
* **Cetak data pelanggan:** Menghasilkan dokumen atau laporan data pelanggan.
* **Mengkonfirmasi pemesanan:** Memvalidasi status kendaraan yang akan disewa.
* **Mengkonfirmasi pengembalian:** Memproses penyelesaian sewa dan pengecekan denda.

### 3. Pelanggan
Pihak yang menggunakan jasa layanan.
* **Merental kendaraan:** Melakukan proses penyewaan unit melalui bantuan petugas.

---

## 🗄️ Struktur Database

### 1. Tabel User
Digunakan untuk autentikasi login Admin dan Petugas.
| Field | Tipe Data | Keterangan |
| :--- | :--- | :--- |
| `id_user` | PK | Primary Key |
| `username` | Varchar | Username login |
| `password` | Varchar | Password terenkripsi |
| `role` | Enum | `admin`, `petugas` |

### 2. Tabel Pelanggan
Menyimpan identitas pelanggan yang menyewa kendaraan.
| Field | Tipe Data | Keterangan |
| :--- | :--- | :--- |
| `id_pelanggan` | PK | Primary Key |
| `foto_ktp` | varbinary | Foto KTP |
| `nama_pelanggan` | Varchar | Nama lengkap pelanggan |
| `nomor_hp` | Varchar | Kontak aktif |
| `alamat` | Text | Alamat domisili |

### 3. Tabel Kendaraan
Menyimpan inventaris unit kendaraan yang tersedia.
| Field | Tipe Data | Keterangan |
| :--- | :--- | :--- |
| `id_kendaraan` | PK | Primary Key |
| `nama_kendaraan` | Varchar | Merk/Tipe kendaraan |
| `plat_nomor` | Varchar | Nomor polisi |
| `jenis_kendaraan` | Enum | `mobil`, `motor` |
| `harga_sewa` | Int | Biaya sewa per hari |
| `status` | Enum | `tersedia`, `dipinjam` |

### 4. Tabel Pemesanan
Tabel utama untuk mencatat transaksi penyewaan.
| Field | Tipe Data | Keterangan |
| :--- | :--- | :--- |
| `id_pemesanan` | PK | Primary Key |
| `id_pelanggan` | FK | Relasi ke Tabel Pelanggan |
| `id_kendaraan` | FK | Relasi ke Tabel Kendaraan |
| `id_user(petugas)` | FK | Relasi ke Tabel User |
| `tanggal_sewa` | Date | Tanggal mulai sewa |
| `tanggal_kembali` | Date | Batas waktu pengembalian |
| `total_harga` | Int | Hasil hitung otomatis (Trigger) |
| `status` | Enum | `dirental`, `selesai` |

### 5. Tabel Pengembalian
Mencatat proses penyelesaian transaksi dan denda.
| Field | Tipe Data | Keterangan |
| :--- | :--- | :--- |
| `id_pengembalian` | PK | Primary Key |
| `id_pemesanan` | FK | Relasi ke Tabel Pemesanan |
| `tgl_kembali` | Date | Tanggal aktual pengembalian |
| `denda` | Int | Hasil hitung otomatis (Trigger) |

---

## 🔗 Relasi Data
* Setiap **Pemesanan** hanya memiliki **1 Kendaraan**.
* Setiap **Pemesanan** bisa melakukan **1 pengembalian**.
* Setiap **Pemesanan** memiliki **1 pelanggan**
* **1 user(petugas)** bisa melakukan **1 pemesanan kendaraan**.

---

## ⚙️ Database Triggers
Aplikasi ini menggunakan fitur *trigger* pada database untuk otomatisasi perhitungan:

1.  **Otomatisasi Total Harga:**
    Menghitung `total_harga` pada tabel pemesanan secara otomatis dengan rumus: 
    `Harga Sewa (dari Tabel Kendaraan) x Durasi Hari (Tanggal Kembali - Tanggal Sewa)`.

2.  **Otomatisasi Denda:**
    Menghitung `denda` secara otomatis pada tabel pengembalian jika `tgl_kembali` (aktual) melewati `tanggal_kembali` (jatuh tempo) yang ada pada data pemesanan.

---
*Dibuat untuk dokumentasi sistem Aplikasi Manajemen Rental Kendaraan.*



