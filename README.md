# ProjectCapstone

## Pengaturan Lingkungan Pengembangan di GCP untuk Aplikasi Node.js dan FastAPI

Dokumen ini memberikan panduan langkah demi langkah untuk menyiapkan lingkungan pengembangan di Google Cloud Platform (GCP) Compute Engine, khusus untuk aplikasi Node.js dan FastAPI.

### Prasyarat
* Akun Google Cloud Platform
* Pemahaman dasar tentang perintah Linux dan navigasi terminal

### Langkah-langkah

1. **Buat Instance VM:**
   * Akses Google Cloud Console dan buat instance Compute Engine baru.
   * Pilih tipe mesin, region, dan zone yang sesuai dengan kebutuhan aplikasi Anda.
   * Pastikan instance memiliki sumber daya (CPU, memori) yang cukup.

2. **Sambungkan ke Instance via SSH:**
   * Gunakan perintah SSH yang diberikan untuk terhubung ke instance Anda. Anda akan masuk ke sesi terminal pada instance.

3. **Instal Git:**
   * Update daftar paket: `sudo apt update`
   * Instal Git: `sudo apt install git`

4. **Clone Repository:**
   * Ganti `https://...` dengan URL repository Git Anda yang sebenarnya.
   * Jalankan: `git clone https://...`

5. **Instal Node.js dan npm:**
   * Instal Node.js dan npm menggunakan repository NodeSource:
     ```bash
     curl -sL [https://deb.nodesource.com/setup_18.x](https://deb.nodesource.com/setup_18.x) | sudo -E bash -
     ```
     Ganti `18.x` dengan versi Node.js yang Anda inginkan.
   * Instal Node.js: `sudo apt install nodejs`

6. **Inisialisasi Proyek npm dan Instal Dependensi:**
   * Navigasi ke direktori proyek Anda: `cd your-project-directory`
   * Inisialisasi proyek npm: `npm init -y`
   * Instal dependensi: `npm install docker hapi fastapi pm2 nodemon`

7. **Siapkan Lingkungan Python (untuk FastAPI):**
   * Buat lingkungan virtual: `python3 -m venv .venv`
   * Aktifkan lingkungan virtual: `.venv/bin/activate`
   * Instal FastAPI: `pip install fastapi uvicorn`

8. **Jalankan Docker (jika diperlukan):**
   * Ikuti dokumentasi resmi Docker untuk distribusi Linux Anda untuk menginstal dan mengkonfigurasi Docker.

9. **Jalankan Aplikasi:**

   * **Jalankan Aplikasi Node.js:**
     1. Buat skrip `startup.sh` dengan konten berikut:
        ```bash
        #!/bin/bash
        pm2 resurrect
        sudo docker start retinova
        npm run start
        ```
     2. Berikan izin eksekusi: `chmod +x startup.sh`
     3. Jalankan skrip menggunakan pm2: `pm2 start ./startup.sh --name "api-node"`
     4. jalankan `pm2 save`, `pm2 startup`, `pm2 status`, `pm2 logs api-node` 

   * **Jalankan Aplikasi FastAPI:**
     1. Buat skrip `start_fastapi.sh` dengan konten berikut:
        ```bash
        #!/bin/bash
        source .venv/bin/activate
        fastapi dev main.py
        ```
     2. Berikan izin eksekusi: `chmod +x start_fastapi.sh`
     3. Jalankan skrip menggunakan pm2: `pm2 start ./start_fastapi.sh --name "fastapi-ml"`
     4. jalankan `pm2 save`, `pm2 startup`, `pm2 status`, `pm2 logs fastapi-ml` 

### Catatan Tambahan

* **Aturan Firewall:** Pastikan aturan firewall GCP Anda mengizinkan lalu lintas masuk pada port yang digunakan aplikasi Anda (misalnya, HTTP, HTTPS).
* **Alokasi Sumber Daya:** Sesuaikan sumber daya instance dengan kebutuhan aplikasi Anda.
* **Penanganan Kesalahan:** Periksa output setiap perintah dengan cermat dan lakukan pemecahan masalah jika terjadi kesalahan.
* **Proses Latar Belakang:** pm2 memungkinkan Anda mengelola aplikasi sebagai proses latar belakang, sehingga aplikasi tetap berjalan meskipun Anda memutuskan sambungan SSH.
* **Praktik Terbaik:** Pertimbangkan untuk menggunakan pengaturan proyek yang lebih terstruktur (misalnya, Docker Compose) untuk mengelola beberapa container dan layanan.

### Penjelasan Perintah
* `npm init -y`: Menginisialisasi proyek npm dengan pengaturan default.
* `nodemon`: Secara otomatis memulai ulang server Node.js saat ada perubahan file.
* `uvicorn` atau `fastapi dev main.py`: Implementasi server ASGI untuk menjalankan aplikasi FastAPI.
* `pm2`: Manajer proses untuk aplikasi Node.js, memungkinkan Anda memulai, menghentikan, dan memantau proses.

Dengan mengikuti langkah-langkah di atas, Anda dapat menyiapkan lingkungan pengembangan di instance VM GCP dan menjalankan aplikasi Node.js dan FastAPI Anda.
 
**Peningkatan Tambahan:**

* **Diagram:** Anda bisa menambahkan diagram alur sederhana untuk menggambarkan proses instalasi dan pengaturan.
* **Contoh Kode:** Sertakan contoh kode sederhana untuk menunjukkan cara menggunakan pustaka atau framework tertentu.
* **Troubleshooting:** Sediakan bagian untuk troubleshooting masalah umum yang mungkin ditemui.

Dengan `README.md` yang baik, Anda akan memudahkan orang lain untuk memahami dan berkontribusi pada proyek Anda.
