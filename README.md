## 1. Karakteristik Memori dan Akses Data: Array vs. Singly Linked List

Perbedaan kecepatan akses antara Array dan Singly Linked List ditentukan oleh **cara kedua struktur data tersebut dialokasikan di dalam memori fisik (RAM)**.

### Array: Akses Konstan - O(1)
Array dialokasikan pada blok memori yang **kontinu (berurutan)**. Setiap elemen memiliki ukuran data yang seragam (misalnya, 4 byte untuk tipe data integer). Karena strukturnya yang berurutan, komputer tidak perlu menelusuri elemen satu per satu untuk menemukan data pada indeks tertentu. 

Komputer dapat langsung menghitung lokasi memori elemen yang dicari menggunakan rumus matematika sederhana berikut:
Alamat Elemen = Alamat Dasar + (Indeks x Ukuran Elemen)

Proses kalkulasi ini terjadi secara instan, sehingga kompleksitas waktu akses elemen pada Array adalah **O(1)**.

### Singly Linked List: Akses Linear - O(n)
Sebaliknya, Singly Linked List dialokasikan secara **non-kontinu (terpencar)** di dalam memori. Setiap elemen (node) dapat berada di lokasi RAM mana saja secara acak. Agar antar-node tetap terhubung, setiap node wajib menyimpan alamat memori dari node berikutnya menggunakan pointer `next`.

Karena lokasi memorinya tidak berurutan, komputer tidak dapat menghitung langsung alamat memori dari node ke-n. Untuk mengakses elemen di posisi tertentu, proses penelusuran harus dimulai dari node pertama (`head`) dan mengikuti petunjuk pointer `next` satu per satu hingga mencapai node tujuan. Proses penelusuran (*traversal*) inilah yang menyebabkan kompleksitas waktunya menjadi **O(n)**.

---

## 2. Analisis Efisiensi Operasi Manipulasi (Insertion & Deletion)

Secara teoritis, Linked List lebih diunggulkan dibandingkan Array untuk operasi penyisipan (*insertion*) dan penghapusan (*deletion*) data dalam kondisi: **Operasi dilakukan di awal list (`head`) ATAU kita telah memiliki referensi/pointer langsung ke node tempat manipulasi akan dilakukan.**

### Alasan Teoritis:
* **Pada Array - O(n):** Array menuntut elemen-elemennya untuk selalu rapat dan berurutan di dalam memori. Jika dilakukan penyisipan atau penghapusan data di awal atau di tengah array, elemen-elemen setelahnya **harus digeser (shifted)** satu per satu untuk menjaga kontinuitas memori. Proses pergeseran inilah yang membutuhkan waktu linear **O(n)**.
* **Pada Linked List - O(1):** Linked List tidak memerlukan struktur memori yang rapat. Operasi penyisipan dan penghapusan data pada Linked List hanya melibatkan **pemutusan dan penyambungan ulang pointer** (*rewiring links*). Kita hanya perlu mengubah tujuan pointer `next` pada node terkait tanpa harus menggeser data lainnya di dalam memori. Oleh karena itu, operasinya berjalan dalam waktu konstan **O(1)**.

---

## 3. Konsep Doubly Linked List

### Anatomi Node Doubly Linked List
Sebuah node pada Doubly Linked List memiliki struktur anatomi yang terdiri dari tiga bagian utama:
* **Pointer Prev (Previous):** Bagian yang menyimpan alamat memori dari node sebelumnya.
* **Data / Value:** Bagian yang menyimpan nilai atau informasi aktual dari elemen tersebut.
* **Pointer Next:** Bagian yang menyimpan alamat memori dari node setelahnya.

### Dampak Penggunaan Memori dan Fleksibilitas
* **Dampak Memori (Kompleksitas Ruang):** Keberadaan pointer `prev` meningkatkan penggunaan memori (*overhead*). Pada arsitektur sistem 64-bit, sebuah pointer membutuhkan alokasi sebesar 8 byte. Dengan demikian, setiap node pada Doubly Linked List memerlukan tambahan 8 byte memori dibandingkan Singly Linked List untuk menyimpan pointer ekstra tersebut.
* **Fleksibilitas Penelusuran:** Fleksibilitas navigasi meningkat secara signifikan. Jika Singly Linked List hanya dapat ditelusuri satu arah (maju), Doubly Linked List memungkinkan penelusuran **dua arah (maju dan mundur)**. Hal ini mempermudah operasi seperti navigasi mundur (*backtracking*), penghapusan node tertentu tanpa perlu melacak dari `head`, serta efisien untuk implementasi struktur data lain seperti Deque (Double-ended Queue).

---

## 4. Mekanisme Circular Linked List

### Perbedaan Teoritis
Pada Linked List biasa (Singly atau Doubly), node terakhir akan selalu menunjuk ke nilai kosong, yaitu `None` di Python (atau `NULL` pada bahasa pemrograman lain), sebagai penanda akhir dari struktur data. 

Pada **Circular Linked List**, pointer `next` dari node terakhir **tidak menunjuk ke `None`, melainkan menunjuk kembali ke node pertama (`head`)**. Hal ini membentuk sebuah struktur melingkar atau siklus tanpa ujung.

### Skenario Penggunaan (Use Case)
Struktur melingkar ini sangat efektif digunakan pada sistem **Round-Robin Scheduling** (Penjadwalan CPU pada Sistem Operasi). 

Dalam skenario ini, CPU bertugas membagikan jatah waktu (*time quantum*) yang sama kepada beberapa proses secara bergantian. Circular Linked List memungkinkan sistem operasi untuk terus berputar menggilir proses-proses tersebut secara berulang tanpa perlu melakukan reset pointer ke awal list secara manual saat mencapai elemen terakhir.

---

## 5. Array Dinamis di Python (list)

Secara internal, tipe data `list` di Python diimplementasikan sebagai Dynamic Array. Ketika kita melakukan operasi `.append()` namun kapasitas memori internal yang disediakan telah penuh, Python secara otomatis menjalankan mekanisme **Resize & Over-allocation**:

1. **Alokasi Memori Baru:** Python akan memesan sebuah blok memori baru di RAM dengan ukuran yang **jauh lebih besar** (biasanya berkisar antara 1.125 hingga 1.5 kali dari kapasitas sebelumnya, ditambah margin konstan kecil). Strategi kelebihan alokasi (*over-allocation*) ini sengaja dilakukan agar list tidak terlalu sering melakukan proses *resize*.
2. **Menyalin Data (Copying):** Python menyalin seluruh elemen (atau referensi objek) dari blok memori lama ke blok memori yang baru secara berurutan.
3. **Menambahkan Elemen Baru:** Elemen baru yang di-`append` dimasukkan ke slot kosong pertama pada blok memori baru tersebut.
4. **Deallokasi:** Blok memori lama yang sudah tidak digunakan akan dihapus dan dibebaskan kembali ke sistem agar tidak terjadi kebocoran memori (*memory leak*).

### Analisis Kompleksitas Amortisasi
Meskipun proses penyalinan data saat kapasitas habis memakan waktu linear O(n), operasi ini **jarang terjadi** karena adanya strategi *over-allocation*. Mayoritas operasi `.append()` pada waktu lainnya hanya memakan waktu konstan O(1). Oleh karena itu, secara teoritis kompleksitas waktu untuk operasi `.append()` pada Array dinamis dikategorikan sebagai **O(1) amortized** (rata-rata konstan).
