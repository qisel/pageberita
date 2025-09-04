### 1. Manajemen Konten

Fitur-fitur ini adalah inti dari alur kerja Anda untuk membuat dan mempublikasikan berita.

* **Tulis Berita Baru**
    * **Cara Kerja:** Tombol ini memanggil fungsi `openModal('new-article')`, yang kemudian menjalankan `populateArticleForm()` untuk membuat formulir kosong di dalam jendela pop-up (*modal*).
    * **Manfaat:** Memberikan alur kerja yang terpusat dan efisien untuk membuat konten baru tanpa perlu meninggalkan halaman utama.

* **Generator Teks dengan AI**
    * **Cara Kerja:** Di dalam formulir, tombol "Buat" memanggil fungsi `generateWithGemini()`. Fungsi ini tidak mengirim Kunci API Anda ke browser. Sebaliknya, ia mengirimkan topik berita ke "jembatan aman" di Google Apps Script Anda. Skrip Anda kemudian secara rahasia menggunakan Kunci API yang tersimpan aman untuk menghubungi server AI, lalu mengirimkan hasilnya kembali ke formulir Anda.
    * **Manfaat:** Sangat mempercepat proses penulisan draf berita dan yang terpenting, **menjaga keamanan Kunci API Anda** dengan menyembunyikannya di sisi server.

* **Dukungan Multi-Foto & Tabel**
    * **Cara Kerja (Foto):** JavaScript pada fungsi `displayContent()` menghitung jumlah link foto dan menambahkan atribut `data-count` pada kontainer foto. CSS kemudian menggunakan atribut ini (`.foto-samping[data-count="..."]`) untuk menerapkan tata letak yang sesuai secara otomatis.
    * **Cara Kerja (Tabel):** Fungsi `displayContent()` kini memiliki "penerjemah" cerdas (`processMarkdownTables()`) yang secara otomatis mendeteksi format tabel berbasis teks (Markdown) dari AI dan mengubahnya menjadi tabel HTML yang rapi sebelum ditampilkan.
    * **Manfaat:** Memberikan presentasi visual yang dinamis, profesional, dan otomatis untuk berbagai jenis konten, baik gambar maupun data.

### 2. Manajemen Arsip

Anda memiliki kontrol penuh atas semua berita yang pernah Anda publikasikan, layaknya seorang editor sungguhan.

* **Panel Arsip Berita**
    * **Cara Kerja:** Tombol ini memanggil `openModal('archive')`, yang menjalankan fungsi `populateArchiveModal()`. Fungsi ini kemudian melakukan `fetch` ke Google Script dengan parameter khusus (`?action=list`) untuk meminta daftar semua judul berita yang pernah ada.
    * **Manfaat:** Memberikan akses cepat dan terorganisir ke seluruh riwayat konten Anda di satu tempat.

* **Lihat, Edit, dan Hapus Berita Lama**
    * **Cara Kerja:** Di dalam arsip, setiap tombol memiliki perintah unik:
        * **Lihat:** Mengarahkan browser ke URL dengan ID berita yang spesifik (contoh: `?berita=12345678`), memuat hanya berita tersebut.
        * **Edit:** Memanggil `openModal('edit-article', ID_BERITA)`, yang kemudian mengambil data lengkap dari berita tersebut dan mengisinya kembali ke dalam formulir.
        * **Hapus:** Memanggil `deleteArticle(ID_BERITA)`, yang mengirimkan perintah `article-delete` ke Google Script untuk menghapus baris yang sesuai di Google Sheet.
    * **Manfaat:** Memberikan siklus manajemen konten yang lengkap (Buat, Baca, Perbarui, Hapus) atau *CRUD (Create, Read, Update, Delete)*.

* **Sistem Link Permanen (Permalink)**
    * **Cara Kerja:** Saat halaman dimuat, JavaScript di `loadAllContent()` memeriksa apakah ada parameter `?berita=ID` di URL. Jika ada, ia akan secara spesifik meminta data untuk ID tersebut dari Google Script, bukan hanya berita terakhir.
    * **Manfaat:** Setiap berita bisa dibagikan secara individual dan akan selalu dapat diakses melalui link uniknya, sangat penting untuk media sosial dan referensi.

### 3. Pengaturan & Kustomisasi

Fleksibilitas untuk mengubah tampilan dan perilaku majalah tanpa menyentuh kode sama sekali.

* **Panel Pengaturan Terpusat**
    * **Cara Kerja:** Memanggil `populateSettingsForm()`, yang membuat serangkaian input. Setiap tombol "Terapkan" memanggil `updateSingleSetting('KUNCI', this)`, mengirimkan satu perubahan spesifik ke Google Script.
    * **Manfaat:** Kustomisasi yang terasa instan dan efisien karena setiap perubahan disimpan secara individual tanpa perlu menyimpan ulang seluruh formulir.

* **Kustomisasi Tampilan & Info**
    * **Cara Kerja:** JavaScript pada fungsi `displayContent()` membaca nilai-nilai seperti `Nama_Majalah`, `Deskripsi_Halaman`, `Link_Logo_KUA`, dll., dari data yang dikirim oleh Google Script. Jika sebuah link logo kosong, JavaScript akan secara cerdas menyembunyikan elemen logo tersebut dari tampilan.
    * **Manfaat:** Anda bisa mengubah identitas visual dan informasi kontak majalah Anda kapan saja langsung dari Google Sheet.

* **Kustomisasi Prompt AI**
    * **Cara Kerja:** Teks yang Anda simpan di `AI_System_Prompt` diambil oleh JavaScript dan dikirimkan bersamaan dengan topik berita Anda ke Google Script. Ini berfungsi sebagai "instruksi utama" yang memberi tahu AI bagaimana harus bersikap dan format apa yang harus dihasilkan.
    * **Manfaat:** Anda memiliki kontrol penuh atas "kepribadian" dan gaya penulisan asisten AI Anda.

### 4. Fitur Pembaca & Pengalaman Pengguna

Detail-detail yang membuat majalah Anda terasa modern dan nyaman dibaca.

* **Tampilan Responsif**
    * **Cara Kerja:** Menggunakan aturan `@media (max-width: 650px)` di dalam CSS. Aturan di dalam blok ini secara otomatis diterapkan oleh browser jika lebar layar kurang dari 650 piksel, mengubah tata letak dari multi-kolom menjadi satu kolom agar nyaman dibaca di HP.
    * **Manfaat:** Pengalaman membaca yang optimal di semua jenis perangkat, dari HP hingga komputer.

* **Arsip Berita Berjalan (News Ticker)**
    * **Cara Kerja:** JavaScript mengambil 10 judul berita terakhir dari data yang dikirim Google Script dan menampilkannya di footer. Gerakannya diatur oleh animasi CSS (`@keyframes ticker-scroll`), dan kecepatannya bisa diubah dengan mudah di CSS.
    * **Manfaat:** Mendorong pembaca untuk menjelajahi lebih banyak konten lama dengan cara yang menarik.

* **Pratinjau Link Media Sosial**
    * **Cara Kerja:** Di dalam `<head>`, terdapat *meta tag* khusus seperti `og:title` dan `og:image`. Saat halaman dimuat, fungsi `displayContent()` akan secara dinamis memperbarui isi dari tag-tag ini dengan judul dan gambar dari berita yang sedang ditampilkan.
    * **Manfaat:** Membuat link Anda terlihat jauh lebih profesional dan menarik saat dibagikan di WhatsApp, Facebook, atau platform lainnya.

### 5. Fitur Kolaborasi & Keamanan

Fitur canggih yang menjadikan ini lebih dari sekadar halaman web biasa.

* **Mode Penulis (Sebelumnya Kontributor)**
    * **Cara Kerja:** JavaScript memeriksa parameter `?mode=penulis` di URL. Jika ada, ia akan secara drastis mengubah tampilan `#admin-panel`, hanya menyisakan tombol "Tulis Berita Baru". Saat penulis mengirim berita, nama mereka akan ditambahkan ke konten dan disimpan di `localStorage` browser mereka untuk kemudahan di kemudian hari.
    * **Manfaat:** Memungkinkan Anda untuk berkolaborasi dengan tim atau penulis lain secara aman, tanpa memberikan mereka akses ke panel pengaturan utama.

* **Panel Berbagi Multi-Fungsi**
    * **Cara Kerja:** Tombol "Bagikan" memanggil `openModal('share')` yang kemudian membuat dua URL berbeda: satu dengan `?view=public` untuk pembaca, dan satu lagi dengan `?mode=penulis` untuk rekan penulis.
    * **Manfaat:** Memberikan Anda alat yang tepat untuk setiap
