# Aplikasi Web "Grocy"

<div align="center">
<img alt="Logo" height="50" src="https://raw.githubusercontent.com/grocy/grocy/master/public/img/grocy_logo.svg?sanitize=true"/>
<h3>ERP beyond your fridge</h3>
<h5> grocy is a web-based self-hosted groceries & household management solution for your home</h5>
</div>

# Sekilas Tentang

      Grocy adalah sebuah aplikasi ERP (Enterprise Resource Planning) berbasis web untuk mengatur kebutuhan bahan makanan rumah tangga di rumah dan bersifat Open Source. Grocy dikembangkan oleh Berrnd Bestel dengan motivasinya yaitu manajemen rumah tangga yang lengkap.

# Instalasi

### Kebutuhan Sistem

- Ubuntu 20.04 
- Nginx
- PHP 7.0+
- SQLite

### Langkah instalasi di CLI

1. Akses CLI / buka terminal.
2. Masuk ke root.
```
$ sudo -i
```
3. Instal nginx dan cek statusnya (harus berjalan).
```
$ apt install nginx
$ systemctl status nginx
```
4. Instal sqlite
```
$ apt install sqlite3
```
5. Instal php tanpa Apache dependency
```
$ apt install php-fpm php-sqlite3 php-gd
```
6. Standar untuk www root directory adalah /var/www/html, pengguna nginx di ubah ke 'www-data'.
```
$ cd /var/www/html
$ wget https://releases.grocy.info/latest
$ unzip latest -d /var/www/html && rm latest
$ chown -R www-data:www-data /var/www
$ cp /var/www/html/config-dist.php /var/www/html/data/config.php
$ vi /var/www/html/data/config.php
```
7. Test instalasi nginx dengan membuka http://localhost. Halaman default dari nginx landing page akan tertampilkan.
8. Selanjutnya adalah konfigurasi server dan php.
9. Buat file fastcgi_params di direktori /etc/nginx/conf.d/ untuk setup sistem.
```
$ touch /etc/nginx/conf.d/fastcgi_params
```
10. Buka file fastcgi_params
```
$ editor /etc/nginx/conf.d/fastcgi_params
```
11. Isi file tersebut dengan kode di bawah. Pastikan baris ke-3 memiliki versi PHP yang sama dengan yang terinstal di komputermu. 
```
include fastcgi_params;
fastcgi_split_path_info ^(.+?\.php)(|/.*)$;
fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
```
12. Kemudian membuat web root langkah berikut:
```
$ cd /etc/nginx/sites-available
$ cp default grocy
$ ln -s /etc/nginx/sites-available/grocy /etc/nginx/sites-enabled/grocy
$ rm /etc/nginx/sites-enabled/default
```
13. Buka file /etc/nginx/sites-available/grocy.
```
$ editor /etc/nginx/sites-available/grocy
```
14. Edit file tersebut menjadi seperti di bawah. Pastikan versi PHP sama dengan yang terinstal di komputermu.
```
server {
      root /var/www/html/public;

      # Add index.php to the list if you are using PHP
      index index.html index.htm index.nginx-debian.html index.php;

      server_name _;

      location / {
              # First attempt to serve request as file, then
              # as directory, then fall back to displaying a 404.
              try_files $uri /index.php;
              #try_files $uri $uri/ =404;
      }

      # pass PHP scripts to FastCGI server
      #
      location ~ \.php$ {
              include snippets/fastcgi-php.conf;

              # With php-fpm (or other unix sockets):
              fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;  # need to edit this!

      #       # With php-cgi (or other tcp sockets):
      #       # fastcgi_pass 127.0.0.1:9000:
      }
}
```
15. Restart nginx.
```
$ systemctl restart nginx.service
```
16. Buka localhost di browser. Halaman grocy seharusnya sudah dapat diakses.


## Konfigurasi (opsional)

Setting server tambahan yang diperlukan untuk meningkatkan fungsi dan kinerja aplikasi, misalnya:
- batas upload file
- batas memori
- dll

Plugin untuk fungsi tambahan
- login dengan Google/Facebook
- editor Markdown
- dll


##  Maintenance (opsional)

Setting tambahan untuk maintenance secara periodik, misalnya:
- buat backup database tiap pekan
- hapus direktori sampah tiap hari
- dll


## Otomatisasi (opsional)

Skrip shell untuk otomatisasi instalasi, konfigurasi, dan maintenance.


# Cara Pemakaian

- Tampilan aplikasi web
- Fungsi-fungsi utama
- Isi dengan data real/dummy (jangan kosongan) dan sertakan beberapa screenshot

Bagian ini akan menampilkan aplikasi web grocy yang sudah diinstal. Fitur-fitur yang akan dicoba adalah fitur Login, Shopping List, dan Stock Overview.

## Halaman login
1. Login dengan username dan password yang sesuai.

![1](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/1.png)

## Tampilan awal
2. Setelah login berhasil, akan ditampilkan tampilan awal yaitu bagian Stock Overview.

![2](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/2%20tampilan%20awal.png)

## Shopping List

3. Sebelum melihat Stock Overview, perlu ditambahkan barang-barang dari Shopping List. Untuk menambahkan barang, tekan tombol biru "Add item".

![3](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3%20shopping%20list.png)

### Tambah Item pertama kali

4. Untuk tambah item pertama kali, isi nama produk yang diinginkan, kemudian tekan enter.

![3.3.0](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3.3.0.png)

5. Setelah enter ditekan, akan muncul dialog box/modal seperti gambar di bawah. Pilih "Add a new product" untuk menambah produk baru.

![3.3](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3.3%20Produk%20baru.png)

6. Beri nama produk dan kebutuhan lainnya sesuai keinginan.

![3.4](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3.4%20isi%20keterangan%20produk%20baru.png)

7. Jika sudah selesai, tekan tombol "Save & continue"

![3.5](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3.5%20isi%20keterangan%20produk%20baru.png)

8. Produk sudah terbuat. Isi kebutuhan yang diperlukan. Tekan tombol "Save" untuk menyimpan produk.

![3.6](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3.6%20add%20produk%20baru.png)

9. Produk baru sudah ditambahkan.

![3.7](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3.7%20after%20add.png)

### Tambah item yang sudah terdaftar

10. Jika produk sudah bernah di tambah, tidak perluh mengikuti langkah-langkah di atas lagi. Beri nama produk tersebut, misalnya "Kecap". Tulis Note jika diperlukan. Jika sudah selesai, tekan tobol "Save".

![3.1](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3.1%20add%20item.png)

11. Kecap sudah ditambahkan di Shopping List. Catatan : Produk Kecap sebelumnya sudah pernah dibuat, Jika produk yang ditambahkan belum pernah dibuat maka perlu dibuat dulu untuk pertama kali seperti langkah di atas.

![3.2](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/3.2%20after%20add.png)

## Stock Overview

12. Sebelum melihat barang di Stock Overview, tekan tombol "Add this item to stock" di bagian Shopping List dari produk yang sudah ditambahkan.

![4](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/4%20Shopping%20list%20to%20stock.png)

13. Isi kebutuhan yang diperlukan. Kemudian tekan tombol "OK"

![4.1](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/4.1%20save%20to%20stock.png)

14. Barang dari bagian Shopping List sudah menjadi Stock.

![4.2](https://github.com/pascalpanatagama/Komdat/blob/main/Grocy/4.2%20stock%20display.png)


## Pembahasan

- Pendapat anda tentang aplikasi web ini
    - kelebihan
    - kekurangan
- Bandingkan dengan aplikasi web lain yang sejenis

      

## Referensi

Cantumkan tiap sumber informasi yang anda pakai.

1. [Grocy-ERP beyond your fridge](https://grocy.info/)
2. [Instalasi grocy](https://github.com/grocy/grocy/issues/649)
