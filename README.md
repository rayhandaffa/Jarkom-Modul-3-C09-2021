# Jaringan Komputer C-09 (2021)
Laporan Resmi Jaringan Komputer Modul 3 - DHCP dan Proxy

NRP              | Nama
-----------------|-----------
05111940000014   | Ega Prabu Pamungkas
05111940000178   | Muhammad Rizqullah Akbar
05111940000227   | Rayhan Daffa Alhafish


# Nomor 11
### Soal
*Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie (11).*
### Jawaban
Untuk melakukan redirect website google.com ke super.franky.C09.com, maka dilakukan tambahan konfigurasi pada *proxy server* water7 pada file */etc/squid/squid.conf* dengan barisan kode berikut.
```
acl google dstdomain .google.com
http_access deny google
deny_info http://super.franky.C09.com google
```
![11](img/?)

# Nomor 12
### Soal
*Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps (12).*

### Jawaban
Untuk membatasi bandwith pada user Luffy maka dilakukan penambahan konfigurasi. Konfigurasi bisa dilakukan pada file */etc/squid/squid.conf* langsung atau menambahkan file baru dan di-*include* kedalam */etc/squid/squid.conf*. Untuk bisa mengidentifikasi user luffy maka digunakan `proxy_auth` dengan ini kita bisa mengetahui user yang melakukan login melalui proxy. untuk kode secara keseluruhan sebagai berikut.
```
acl luffy proxy_auth luffybelikapalC09
delay_pools 2
delay_class 1 1
delay_parameters 1 1250/1250
delay_access 1 allow luffy
delay_access 1 deny all
```
Ditambahkan `delay_parameters 1 1250/1250` untuk membatasi bandwith menjadi 10kbps, dimana 10kbps = 1250 Byte per second.

![12](img/?)

# Nomor 13
### Soal
*Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya (13).*
### Jawaban
Sama seperti konfigurasi *bandwith limit* user luffy, tapi tanpa `delay_parameters` karena bandwith tidak dibatasi. Tambahan konfigurasi dengan barisan kode berikut.
```
delay_class 2 1
delay_parameters 2 none
delay_access 2 allow !luffy
```
Dimana kode diatas bermaksud user selain luffy tidak akan dibatasi bandwithnya.

![13](img/?)
