# Aplikasi Web "Grocy"

<div align="center">
<img alt="Logo" height="50" src="https://raw.githubusercontent.com/grocy/grocy/master/public/img/grocy_logo.svg?sanitize=true"/>
<h3>ERP beyond your fridge</h3>
<h5> grocy is a web-based self-hosted groceries & household management solution for your home</h5>
</div>

# Sekilas Tentang

Deskripsi singkat tentang aplikasi tsb.


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


## Cara Pemakaian

- Tampilan aplikasi web
- Fungsi-fungsi utama
- Isi dengan data real/dummy (jangan kosongan) dan sertakan beberapa screenshot


## Pembahasan

- Pendapat anda tentang aplikasi web ini
    - kelebihan
    - kekurangan
- Bandingkan dengan aplikasi web lain yang sejenis


## Referensi

Cantumkan tiap sumber informasi yang anda pakai.
