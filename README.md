📝 Odoo Entry System - Secure
Aplikasi web form pengajuan (request form) serverless yang digunakan untuk mencatat entri data ke dalam ekosistem Odoo. Aplikasi ini memisahkan sisi frontend (di-host secara gratis di GitHub Pages) dan sisi backend (menggunakan Google Apps Script & Google Sheets).
Diperkuat dengan integrasi Google Identity Services (OAuth 2.0 JWT) untuk memastikan hanya pengguna yang sah yang dapat mengirimkan data, dan mencegah manipulasi identitas (email) oleh pihak yang tidak bertanggung jawab.
✨ Fitur Utama
🔐 Google OAuth 2.0: Autentikasi aman menggunakan akun Google (Login with Gmail/Workspace).
🗄️ Google Sheets Database: Data formulir langsung tersimpan ke Google Sheets secara real-time.
📂 Drive File Upload: Mendukung lampiran file (maks. 10MB) yang otomatis diunggah ke Google Drive dan link-nya disimpan ke Sheet.
⚡ Dynamic Dropdown & Caching: Data pilihan (Vendor, Company, dll.) diambil dinamis dari Sheet master dan di-cache di LocalStorage selama 10 menit agar web memuat lebih cepat.
➕ "Add New" Capability: Jika opsi dropdown tidak ada, pengguna dapat menambahkan entitas baru secara langsung.
🌓 Dark Mode: Tampilan antarmuka mendukung mode Terang dan Gelap.
💰 Auto-Formatter: Input nominal (Amount) akan otomatis diformat dengan pemisah ribuan rupiah.
🛠️ Tech Stack
Frontend: HTML5, Tailwind CSS (via CDN), JavaScript Vanilla, Choices.js (untuk UI Dropdown).
Backend (REST API): Google Apps Script (GAS).
Database & Storage: Google Sheets & Google Drive.
Authentication: Google Identity Services (GSI).
Hosting: GitHub Pages.
🚀 Panduan Instalasi & Deployment
Tahap 1: Setup Google Sheets & Drive (Database)
Buat Google Sheets baru dengan sheet bernama Form (atau sesuaikan dengan kebutuhan).
Buat folder di Google Drive khusus untuk menyimpan file lampiran yang diunggah.
Salin Spreadsheet ID (dari URL Sheet) dan Folder ID (dari URL Drive).
Tahap 2: Setup Google Apps Script (Backend)
Buka script.google.com dan buat proyek baru.
Salin seluruh kode dari file Code.gs ke editor.
Sesuaikan variabel di dalam objek CONFIG:
const CONFIG = {
  spreadsheetId: 'YOUR_SPREADSHEET_ID',
  sheetName: 'Form',
  folderId: 'YOUR_FOLDER_ID',
  clientId: 'YOUR_GOOGLE_CLIENT_ID' // Akan didapat di Tahap 3
};


Klik Deploy -> New deployment.
Pilih Web app.
Execute as: Me (email Anda)
Who has access: Anyone (Sangat penting agar API bisa diakses dari GitHub)
Simpan Web App URL yang diberikan.
Tahap 3: Setup Google Cloud Console (Autentikasi)
Buka Google Cloud Console.
Buat Project > Buka APIs & Services > Credentials.
Klik Create Credentials > OAuth client ID > Pilih Web application.
Di bagian Authorized JavaScript origins, masukkan URL GitHub Pages Anda (misal: https://username.github.io).
Simpan dan salin Client ID Anda. Masukkan Client ID ini ke variabel CONFIG di GAS (Tahap 2) dan file HTML (Tahap 4).
Tahap 4: Setup Frontend (GitHub Pages)
Buka file index_odoo.html.
Ubah baris konfigurasi berikut sesuai dengan data yang Anda dapatkan di tahap sebelumnya:
// === KONFIGURASI PENTING ===
const SCRIPT_URL = "[https://script.google.com/macros/s/.../exec](https://script.google.com/macros/s/.../exec)"; // Dari Tahap 2
const GOOGLE_CLIENT_ID = "123456...apps.googleusercontent.com";   // Dari Tahap 3


Commit dan Push ke repositori GitHub Anda.
Masuk ke Settings repository Anda > Pages > Atur branch ke main untuk mengaktifkan GitHub Pages.
⚠️ Troubleshooting Umum
Masalah / Error
Penyebab & Solusi
"Sesi login ditolak / Failed to fetch"
Biasanya karena izin CORS. Pastikan di Google Apps Script saat melakukan Deploy, opsi Who has access diatur ke "Anyone". Jika Anda menggunakan akun Perusahaan (Google Workspace) dan tidak ada opsi "Anyone", minta admin IT untuk membuka akses publik, atau gunakan akun @gmail.com personal untuk mendeploy Script API-nya.
"Exception: Anda tidak memiliki izin untuk memanggil UrlFetchApp.fetch"
Apps Script memblokir fungsi validasi JWT. Solusi: Buat fungsi dummy function tes() { UrlFetchApp.fetch("https://google.com"); }, lalu jalankan fungsi tersebut satu kali dari editor GAS dan klik Allow / Izinkan pada pop-up otorisasi yang muncul.
Tombol Google Sign-in tidak muncul
URL tempat web di-host belum didaftarkan di Authorized JavaScript origins di Google Cloud Console.
Perubahan kode di Apps Script tidak merespon
Setiap mengubah kode Code.gs, Anda WAJIB mendeploy ulang menggunakan mode Manage deployments > Edit > Version: New version. Jangan menggunakan URL dari "Test deployment".

📄 Lisensi
Sistem ini dibangun untuk kebutuhan internal (Private Tool). Silakan sesuaikan kebijakan lisensi jika ingin dipublikasikan.
