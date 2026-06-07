# Jawaban Tugas: Struktur Data dan Analisis Kompleksitas

Fokus penjelasan pada konsep dasar struktur data, karakteristik memori, serta analisis kompleksitas waktu dan ruang.

---

## 1. Karakteristik Memori dan Akses Data: Array vs. Singly Linked List

Perbedaan kecepatan akses antara Array dan Singly Linked List berakar pada **bagaimana elemen-elemen tersebut disimpan di dalam memori fisik (RAM)**.

### Array: Akses Konstan $O(1)$
Array dialokasikan pada blok memori yang **kontinu (berurutan)**. Setiap elemen memiliki ukuran data yang sama (misalnya, 4 byte untuk integer). Karena strukturnya yang berurutan, komputer tidak perlu menelusuri elemen satu per satu untuk menemukan data di indeks tertentu. 

Komputer menggunakan rumus matematika sederhana berikut untuk langsung menghitung lokasi memori elemen yang dicari:
$$\text{Alamat Elemen} = \text{Alamat Dasar} + (\text{Indeks} \times \text{Ukuran Elemen})$$

Proses kalkulasi ini terjadi secara instan, sehingga kompleksitas waktunya adalah **$O(1)$**.

### Singly Linked List: Akses Linear $O(n)$
Sebaliknya, Linked List dialokasikan secara **non-kontinu (terpencar)** di memori. Setiap elemen (node) bisa berada di mana saja di dalam RAM. Agar tetap terhubung, setiap node harus menyimpan alamat memori node berikutnya (*pointer next*).

Karena lokasi memori yang acak ini, komputer tidak bisa menghitung langsung alamat memori dari node ke-$n$. Untuk mengakses elemen di tengah atau di akhir, kita wajib memulai dari node pertama (*head*) dan menelusuri pointer satu per satu hingga sampai ke elemen yang dituju. Proses penelusuran (*traversal*) inilah yang membuat kompleksitas waktunya menjadi **$O(n)$**.

---

## 2. Analisis Efisiensi Operasi Manipulasi (Insertion & Deletion)

Linked List akan lebih diunggulkan dibandingkan Array untuk operasi penyisipan (*insertion*) dan penghapusan (*deletion*) dalam kondisi: **Operasi dilakukan di awal list (head) ATAU kita sudah memegang referensi/pointer ke node tempat manipulasi akan dilakukan.**

### Alasan Teoritis:
* **Pada Array ($O(n)$):** Ketika Anda menyisipkan atau menghapus elemen di awal atau tengah Array, elemen-elemen setelahnya **harus digeser (shifted)** satu per satu di dalam memori untuk menjaga sifat kontinunya. Proses pergeseran memori inilah yang memakan waktu linear $O(n)$.
* **Pada Linked List ($O(1)$):** Linked List tidak memerlukan pergeseran memori fisik. Operasi penyisipan dan penghapusan hanyalah operasi **pemutusan dan penyambungan ulang pointer** (*rewiring links*). Kita hanya perlu mengubah tujuan pointer dari node terkait. Jika lokasi node sudah diketahui, proses ini selesai dalam waktu konstan $O(1)$.

---

## 3. Konsep Doubly Linked List

### Anatomi Node Doubly Linked List
Sebuah node pada Doubly Linked List memiliki struktur anatomi yang terdiri dari tiga bagian utama:
1.  **Data:** Menyimpan nilai atau informasi aktual dari elemen tersebut.
2.  **Pointer Next:** Menyimpan alamat memori dari node setelahnya.
3.  **Pointer Prev (Previous):** Menyimpan alamat memori dari node sebelumnya.

