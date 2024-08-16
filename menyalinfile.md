# Ringkasan Cara menyalin file ke docker 

```bash
docker exec -it devreza bash
chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html
```

```bash
docker cp /home/r/github/app-weding/. devreza:/var/www/html
```



Sepertinya file dan direktori dari `/home/r/github/app-weding` sudah berhasil disalin ke dalam container pada jalur `/var/www/html`. Namun, Anda mungkin tidak dapat melihatnya dengan benar di dalam container.

Berikut adalah langkah-langkah yang dapat Anda lakukan untuk memastikan file telah berhasil disalin dan memeriksa apakah ada kesalahan:

### 1. **Verifikasi di Dalam Container**

Coba periksa isi direktori `/var/www/html` di dalam container dengan lebih rinci. Pastikan Anda sudah berada di dalam direktori yang benar:

```bash
docker exec -it devreza bash
cd /var/www/html
ls -la
```

Perintah `ls -la` akan menampilkan daftar lengkap file dan direktori beserta hak akses dan informasi lainnya. Pastikan file Anda muncul di sana.

### 2. **Periksa Hak Akses**

Pastikan hak akses direktori dan file sudah benar. Jika Anda mengalami masalah hak akses, Anda mungkin perlu memperbaikinya. Di dalam container, Anda bisa mengubah hak akses menggunakan perintah `chmod` dan `chown` jika diperlukan:

```bash
docker exec -it devreza bash
chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html
```

`www-data` adalah pengguna default yang biasanya digunakan oleh web server seperti Apache untuk mengakses file.

### 3. **Cek Ukuran File**

Jika Anda tidak melihat file yang diharapkan, pastikan ukuran file yang disalin sesuai dengan ukuran di host. Anda bisa memeriksa ukuran file di host dan di dalam container untuk memastikan kesesuaian.

### 4. **Pastikan Container Tidak Memiliki Cache**

Kadang-kadang cache atau masalah dengan sistem file bisa menyebabkan masalah. Restart container untuk memastikan semuanya diperbarui:

```bash
docker restart devreza
```

Kemudian periksa kembali file:

```bash
docker exec -it devreza bash
cd /var/www/html
ls -la
```

### 5. **Periksa Kesalahan Penyalinan**

Jika file tidak muncul di direktori yang benar, pastikan perintah `docker cp` berhasil. Coba jalankan perintah `docker cp` sekali lagi dan pastikan tidak ada kesalahan:

```bash
docker cp /home/r/github/app-weding/. devreza:/var/www/html
```

Jika setelah mengikuti langkah-langkah ini file masih tidak muncul, pastikan jalur direktori dan nama file sudah benar dan tidak ada masalah lain yang mempengaruhi visibilitas file dalam container.
