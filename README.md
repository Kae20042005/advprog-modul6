# Module 6 Reflection

## Commit 1 Reflection notes

**Fungsi handle_connection**
- Fungsi ini menggunakan TcpStream sebagai parameter yang merepresentasikan koneksi dengan client
- BufReader::new(&mut stream) membuat buffered reader dari TCP stream sehingga dapat membuat proses membaca baris-perbaris yang lebih efisien
- pada line of code http_request: Vec<_> = buf_reader
   - .lines() berfungsi untuk membaca stream perbaris
   - .map(|result| result.unwrap()) untuk unwrap setiap baris dengan asumsi tidak ada error
   - .take_while(|line| !line.is_empty()) menghentikan proses baca ketika menemukan baris kosong
   - .collect() mengumpulkan baris menjadi Vec<String>

## Commit 2 Reflection notes

![Commit 2 screen capture](/assets/images/commit2.png)

**Perubahan pada fungsi handle_connection baru**
- Tidak seperti sebelumnya yang hanya melakukan print dalam terminal, kini mengembalikan http response.

## Commit 3 Reflection notes

![Commit 3 Screen capture](/assets/images/commit3.png)

**Memisahkan antar response**<br />
Untuk memisahkan antar response dilihat dari request_line terlebih dahulu, kemudian dipisahkan berdasarkan baris pertama 
request_line message, jika Get / HTTP/1.1, maka akan mengembalikan response 200, jika tidak akan mengembalikan response 404.

**Perlunya untuk melakukan refactor**<br />
Refactoring membuat kode jadi lebih mudah dibaca dan dirawat. Kemudian, karena logic hanya dilakukan dalam tempat yang sama
proses debugging akan menjadi lebih cepat.

## Commit 4 Reflection notes

**Masalah yang ditemukan ketika server running secara single thread**<br />
Ketika server yang kita gunakan berjalan secara single thread, request user akan dijalankan secara satu per satu, sehingga,
jika ada request user yang lama (seperti request /sleep), maka user lain harus menunggu request user tersebut selesai.

## Commit 5 Reflection notes

**Cara kerja ThreadPool**<br />
Threadpool merupakan sekelompok thread yang muncul dan menunggu serta siap untuk menangani tugas. Saat program menerima tugas baru,
program akan menetapkan salah satu thread dalam Threadpool ke tugas tersebut dan thread tersebut akan memproses tugas tersebut. Thread
yang tersisa dalam kumpulan tersebut tersedia untuk menangani tugas lain yang masuk saat thread pertama sedang diproses. Saat thread
selesai melakukan tugasnya, thread aan kembali ke Threadpool yang tidak aktif yang siap untuk menerima tugas baru. Implementasi Threadpool
dalam server ini ditandai dengan message worker yang ada pada terminal. Misal kita menjalankan HTTP /sleep worker 0 akan mengerjakan
request tersebut, kemudian ketikan kita mengirim request HTTP / baru, worker 1 yang akan menjalankan request tersebut, sehingga kita
tidak perlu menunggu request /sleep selesai dulu untuk menjalankan request /