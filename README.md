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

![Commit 2 screen capture]!(/assets/images/commit2.png)

**Perubahan pada fungsi handle_connection baru**
- Tidak seperti sebelumnya yang hanya melakukan print dalam terminal, kini mengembalikan http response.