# 🚗 ESP32 Stoplamp Running Text

**ESP32 + MAX7219 LED Matrix + Web Interface**

Proyek stoplamp running text berbasis ESP32 menggunakan modul LED Matrix MAX7219 (FC16) dengan antarmuka konfigurasi berbasis web. Firmware ini merupakan hasil porting dari proyek ESP8266 DBOSS Project dengan mempertahankan animasi bawaan dari library MD_MAX72XX, sekaligus menambahkan berbagai fitur konfigurasi yang dapat diakses langsung melalui smartphone.

Pengguna dapat mengatur teks berjalan, memilih animasi, membuat animasi sendiri melalui editor piksel, serta mengatur kecepatan, durasi, kecerahan, dan urutan pemutaran tanpa perlu mengubah kode program.

---

## ✨ Fitur Utama

### 1. Web Interface
- Konfigurasi melalui browser smartphone atau komputer.
- Menggunakan Wi-Fi Access Point dari ESP32.
- Tidak memerlukan koneksi internet.
- Captive portal untuk memudahkan akses halaman konfigurasi.
- Tampilan responsif dengan dukungan mode gelap.

### 2. Custom Running Text
- Mendukung 2 teks berjalan secara terpisah.
- Setiap teks dapat diaktifkan atau dinonaktifkan.
- Maksimal 100 karakter untuk setiap teks.
- Pratinjau teks langsung melalui halaman web.
- Pengaturan kecepatan scrolling.

### 3. Built-in Animations

Tersedia 17 animasi bawaan:

| No. | Nama Animasi |
|---|---|
| 1 | Midline |
| 2 | Scanner |
| 3 | Random |
| 4 | Fade |
| 5 | Spectrum 1 |
| 6 | Heartbeat |
| 7 | Hearts |
| 8 | Eyes |
| 9 | Bounce Ball |
| 10 | Arrow Scroll |
| 11 | Scroller |
| 12 | Wiper |
| 13 | Invader |
| 14 | Pacman |
| 15 | Arrow Rotate |
| 16 | Spectrum 2 |
| 17 | Sine Wave |

### 4. Custom Animation Editor
Tersedia 2 slot animasi buatan sendiri (Animasi A dan B).

Fitur editor:
- Editor gambar berbasis piksel 8 baris.
- Menggambar menggunakan sentuhan atau mouse.
- Maksimal 16 frame untuk setiap animasi.
- Menambah, menghapus, dan menduplikasi frame.
- Mengubah urutan frame.
- Menggeser gambar ke atas, bawah, kiri, dan kanan.
- Membalik warna dan mencerminkan gambar.
- Pratinjau animasi langsung di browser.
- Mencoba frame secara langsung pada LED Matrix.
- Pengaturan waktu setiap frame.

### 5. Playlist System
- Memilih animasi dan teks yang ingin ditampilkan.
- Mengaktifkan atau menonaktifkan setiap item secara individual.
- Pemutaran otomatis berdasarkan urutan playlist.
- Mode pengacakan (Shuffle).
- Teks berjalan hingga selesai sebelum berpindah ke item berikutnya.
- Animasi berpindah berdasarkan durasi yang telah ditentukan.

### 6. Display Configuration
- Durasi setiap animasi: 2–60 detik melalui antarmuka web.
- Kecepatan teks: 10 tingkat.
- Kecerahan LED: 0–15.
- Pengaturan tersimpan secara permanen menggunakan ESP32 Preferences (NVS).

### 7. Brake Light Priority
- Input rem menggunakan GPIO27.
- Aktif LOW dengan INPUT_PULLUP.
- Ketika rem aktif, seluruh LED Matrix menyala dengan kecerahan maksimum.
- Animasi dan preview dihentikan sementara.
- Playlist dilanjutkan setelah rem dilepas.

---

## 🛠️ Hardware Requirements

| Komponen | Keterangan |
|---|---|
| Microcontroller | ESP32 Dev Module |
| LED Matrix | MAX7219 FC16 |
| Jumlah Matrix | 4 modul (default) |
| Brake Input | GPIO27 |
| Power Supply | 5V untuk MAX7219 |
| Optocoupler | Untuk isolasi sinyal rem kendaraan |

Jumlah modul LED Matrix dapat diubah melalui konfigurasi firmware.

---

## 🔌 Wiring Diagram

### ESP32 to MAX7219

| MAX7219 | ESP32 |
|---|---|
| VCC | 5V |
| GND | GND |
| DIN | GPIO23 |
| CLK | GPIO18 |
| CS | GPIO5 |

### Brake Signal Input

| Komponen | ESP32 |
|---|---|
| Output optocoupler | GPIO27 |
| GND sisi ESP32 | GND |

**Penting:** Jangan menghubungkan sinyal rem kendaraan secara langsung ke GPIO ESP32. Sistem kelistrikan kendaraan dapat menghasilkan tegangan dan lonjakan transien yang melebihi batas aman ESP32.

Gunakan optocoupler atau rangkaian proteksi input yang sesuai untuk mengisolasi sinyal kendaraan.

---

## 💻 Software Requirements

- Arduino IDE
- ESP32 Arduino Core 2.x atau 3.x
- Library MD_MAX72XX oleh majicDesigns

### Required Libraries

| Library | Fungsi |
|---|---|
| Arduino.h | Fungsi dasar Arduino |
| MD_MAX72XX | Kontrol LED Matrix |
| SPI.h | Komunikasi SPI |
| WiFi.h | Wi-Fi ESP32 |
| WebServer.h | Web server |
| DNSServer.h | Captive portal |
| Preferences.h | Penyimpanan konfigurasi NVS |

Sebagian besar library tersebut sudah tersedia pada ESP32 Arduino Core. Library MD_MAX72XX perlu diinstal melalui Library Manager.

---

## 📥 Installation

1. Install Arduino IDE.
2. Install ESP32 Board Package melalui Boards Manager.
3. Install library **MD_MAX72XX** melalui Library Manager.
4. Buka file firmware `.ino`.
5. Pilih board `ESP32 Dev Module`.
6. Sesuaikan konfigurasi jumlah LED Matrix dan Wi-Fi jika diperlukan.
7. Hubungkan ESP32 melalui USB.
8. Upload firmware ke ESP32.

### Default Configuration

```cpp
constexpr int JUMLAH_MODUL_LED_MATRIX = 4;

const char *NAMA_WIFI = "myStoplamp";
const char *PASSWORD_WIFI = "12345678";

#define EDITOR_FLIP_Y 0
```

---

## 📱 How to Use

### 1. Connect to Wi-Fi

Setelah ESP32 dinyalakan, hubungkan smartphone atau komputer ke jaringan:

| Parameter | Nilai |
|---|---|
| SSID | myStoplamp |
| Password | 12345678 |
| IP Address | 192.168.4.1 |

### 2. Open Web Interface

Buka browser dan akses:

**http://192.168.4.1**

### 3. Configure Stoplamp

Melalui halaman web, pengguna dapat:
- Mengubah teks berjalan.
- Mengaktifkan atau menonaktifkan teks.
- Memilih animasi yang ingin ditampilkan.
- Membuat animasi sendiri.
- Mengatur kecepatan dan durasi animasi.
- Mengatur kecerahan LED.
- Mengaktifkan mode Shuffle.

### 4. Save Configuration

Tekan tombol **Simpan dan terapkan** untuk menyimpan konfigurasi ke memori NVS ESP32.

Pengaturan akan tetap tersimpan meskipun perangkat dimatikan atau kehilangan daya.

---

## ⚙️ Customization

### Mengubah Jumlah LED Matrix

```cpp
constexpr int JUMLAH_MODUL_LED_MATRIX = 4;
```

Contoh:
- 1 modul = 8 × 8 LED
- 2 modul = 16 × 8 LED
- 4 modul = 32 × 8 LED
- 8 modul = 64 × 8 LED

Pastikan jumlah modul sesuai dengan perangkat keras yang digunakan.

### Mengubah Orientasi Editor

Jika gambar dari editor web terlihat terbalik secara vertikal pada LED Matrix:

```cpp
#define EDITOR_FLIP_Y 1
```

Jika orientasi sudah benar:

```cpp
#define EDITOR_FLIP_Y 0
```

### Mengubah Pin Brake Input

```cpp
#define BRAKE_PIN 27
```

---

## 💾 Data Storage

Proyek ini menggunakan ESP32 Preferences (NVS) sebagai pengganti EEPROM pada versi ESP8266.

Data yang disimpan meliputi:

- Teks 1 dan teks 2.
- Playlist mask.
- Durasi animasi.
- Kecepatan scrolling.
- Kecerahan LED.
- Mode Shuffle.
- Jumlah frame animasi custom.
- Kecepatan animasi custom.
- Data piksel setiap frame.

---

## 📂 Project Structure

```text
ESP32-Stoplamp-Running-Text/
│
├── ESP32_Stoplamp.ino
├── README.md
└── LICENSE
```

---

## ⚠️ Safety Notes

Proyek ini merupakan perangkat elektronik tambahan untuk kendaraan dan masih memerlukan penyesuaian instalasi sesuai kendaraan masing-masing.

Beberapa hal yang perlu diperhatikan:

1. GPIO ESP32 hanya menggunakan logika 3,3V.
2. Jangan menghubungkan tegangan 12V kendaraan langsung ke ESP32.
3. Gunakan optocoupler dan proteksi tegangan pada input rem.
4. Gunakan regulator DC-DC yang stabil untuk suplai ESP32.
5. Pastikan suplai 5V MAX7219 memiliki kapasitas arus yang cukup.
6. Gunakan sekering dan perlindungan terhadap lonjakan tegangan.
7. Pastikan pemasangan tidak mengganggu fungsi lampu rem standar kendaraan.
8. Perhatikan regulasi lalu lintas setempat mengenai warna dan pola lampu kendaraan.

**Catatan:** Firmware ini masih berupa proyek pengembangan dan bukan perangkat keselamatan kendaraan yang telah tersertifikasi. Jangan menjadikannya pengganti sistem lampu rem standar.

---

## 🙏 Credits

- **DBOSS Project** – Referensi proyek awal ESP8266 dan konsep animasi.
- **Marco Colli** – Pengembang contoh animasi dan library MD_MAX72XX.
- **majicDesigns** – Library MD_MAX72XX.

Proyek ini merupakan pengembangan dan porting dari konsep awal ESP8266 menuju ESP32 dengan penambahan sistem konfigurasi berbasis web, playlist, dan editor animasi custom.

---

## 📜 License

Silakan tambahkan jenis lisensi proyek pada file `LICENSE` sesuai ketentuan lisensi kode sumber asli dan izin penggunaan komponen yang menjadi dasar proyek ini.

---

**Developed with ESP32, MAX7219, and Arduino IDE.**  
*DIY Electronics & Embedded Systems Project.*
