## 1. Karakteristik Memori dan Akses Data: Array vs. Singly Linked List

Perbedaan kecepatan akses antara Array dan Singly Linked List berakar pada **bagaimana elemen-elemen tersebut disimpan di dalam memori fisik (RAM)**.

### Array: Akses Konstan O(1)
Array dialokasikan pada blok memori yang **kontinu (berurutan)**. Setiap elemen memiliki ukuran data yang sama (misalnya, 4 byte untuk integer). Karena strukturnya yang berurutan, komputer tidak perlu menelusuri elemen satu per satu untuk menemukan data di indeks tertentu. 

Komputer menggunakan rumus matematika sederhana berikut untuk langsung menghitung lokasi memori elemen yang dicari:
Alamat Elemen = Alamat Dasar + (Indeks x Ukuran Elemen)

Proses kalkulasi ini terjadi secara instan, sehingga kompleksitas waktunya adalah **O(1)**.

### Singly Linked List: Akses Linear O(n)
Sebaliknya, Linked List dialokasikan secara **non-kontinu (terpencar)** di memori. Setiap elemen (node) bisa berada di mana saja di dalam RAM. Agar tetap terhubung, setiap node harus menyimpan alamat memori node berikutnya (pointer next).

Karena lokasi memori yang acak ini, komputer tidak bisa menghitung langsung alamat memori dari node ke-n. Untuk mengakses elemen di tengah atau di akhir, kita wajib memulai dari node pertama (head) dan menelusuri pointer satu per satu hingga sampai ke elemen yang dituju. Proses penelusuran (traversal) inilah yang membuat kompleksitas waktunya menjadi **O(n)**.

---

## 2. Analisis Efisiensi Operasi Manipulasi (Insertion & Deletion)

Linked List akan lebih diunggulkan dibandingkan Array untuk operasi penyisipan (insertion) dan penghapusan (deletion) dalam kondisi: **Operasi dilakukan di awal list (head) ATAU kita sudah memegang referensi/pointer ke node tempat manipulasi akan dilakukan.**

### Alasan Teoritis:
* **Pada Array O(n):** Ketika Anda menyisipkan atau menghapus elemen di awal atau tengah Array, elemen-elemen setelahnya **harus digeser (shifted)** satu per satu di dalam memori untuk menjaga sifat kontinunya. Proses pergeseran memori inilah yang memakan waktu linear O(n).
* **Pada Linked List O(1):** Linked List tidak memerlukan pergeseran memori fisik. Operasi penyisipan dan penghapusan hanyalah operasi **pemutusan dan penyambungan ulang pointer** (rewiring links). Kita hanya perlu mengubah tujuan pointer dari node terkait. Jika lokasi node sudah diketahui, proses ini selesai dalam waktu konstan O(1).

---

## 3. Konsep Doubly Linked List

### Anatomi Node Doubly Linked List
Sebuah node pada Doubly Linked List memiliki struktur anatomi yang terdiri dari tiga bagian utama yang saling berdampingan:
* **Pointer Prev (Previous):** Bagian yang menyimpan alamat memori dari node sebelumnya.
* **Data / Value:** Bagian tengah yang menyimpan nilai atau informasi aktual dari elemen tersebut.
* **Pointer Next:** Bagian yang menyimpan alamat memori dari node setelahnya.

### Dampak Penggunaan Memori dan Fleksibilitas
* **Dampak Memori (Kompleksitas Ruang):** Keberadaan pointer prev meningkatkan penggunaan memori (overhead). Pada arsitektur sistem 64-bit, sebuah pointer memakan 8 byte. Artinya, setiap node pada Doubly Linked List membutuhkan tambahan 8 byte memori dibandingkan Singly Linked List untuk menyimpan pointer ekstra tersebut.
* **Fleksibilitas Penelusuran:** Fleksibilitas meningkat drastis. Singly Linked List hanya bisa berjalan satu arah (maju). Sementara Doubly Linked List memungkinkan penelusuran **dua arah (maju dan mundur)**. Hal ini mempermudah operasi seperti navigasi backtrack, penghapusan node tertentu tanpa perlu mencari pointer pendahulu dari head, serta implementasi struktur data lain seperti Deque (Double-ended Queue).

---

## 4. Mekanisme Circular Linked List

### Perbedaan Teoritis
Pada Linked List biasa (Singly/Doubly), node terakhir selalu menunjuk ke NULL (atau None di Python), yang menandakan ujung dari struktur data tersebut. 

Pada **Circular Linked List**, pointer next dari node terakhir **tidak menunjuk ke NULL, melainkan menunjuk kembali ke node pertama (head)**. Hal ini menciptakan sebuah struktur cincin atau siklus tanpa ujung yang statis.

### Skenario Penggunaan (Use Case)
Struktur melingkar ini sangat efektif digunakan pada sistem **Round-Robin Scheduling** (Penjadwalan CPU pada Sistem Operasi). 

Dalam skenario ini, CPU memberikan jatah waktu (time quantum) yang sama kepada beberapa aplikasi/proses secara bergantian. Circular Linked List memungkinkan sistem operasi untuk terus berputar menggilir proses-proses tersebut secara berulang tanpa perlu melakukan reset pointer ke awal list secara manual setiap kali mencapai elemen terakhir.

---

## 5. Array Dinamis di Python (list)

Ketika Anda melakukan append pada Python list dan kapasitas memori internalnya sudah penuh, Python tidak bisa langsung memperluas blok memori tersebut karena area di sekitarnya mungkin sudah diisi data lain. 

Di balik layar, terjadi mekanisme otomatis yang disebut **Resize & Over-allocation**:

1. **Alokasi Memori Baru:** Python akan memesan sebuah blok memori baru yang **jauh lebih besar** (biasanya sekitar ~1.125 hingga 1.5 kali dari kapasitas sebelumnya, ditambah margin konstan kecil). Teknik kelebihan alokasi (over-allocation) ini sengaja dilakukan agar list tidak perlu terlalu sering melakukan proses resize.
2. **Menyalin Data (Copying):** Python menyalin seluruh elemen (atau referensi objek) dari blok memori lama ke blok memori yang baru secara berurutan.
3. **Menambahkan Elemen Baru:** Elemen baru yang di-append dimasukkan ke slot kosong pertama di memori baru tersebut.
4. **Deallokasi:** Blok memori lama yang sudah kosong dihapus atau dibebaskan kembali ke sistem agar RAM tidak bocor.

### Analisis Kompleksitas Amortisasi
Meskipun proses pemindahan data ini memakan waktu linear O(n) saat kapasitas habis, operasi ini **jarang terjadi** karena strategi over-allocation di atas. Sebagian besar operasi append biasa hanya memakan waktu O(1). Oleh karena itu, secara teoritis kompleksitas waktu untuk operasi append pada Array dinamis disebut sebagai **O(1) amortized** (rata-rata konstan).
