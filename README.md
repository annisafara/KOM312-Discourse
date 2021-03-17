![Discourse_logo](https://user-images.githubusercontent.com/60083980/111193101-63519880-85ec-11eb-9133-f9f6cc20def9.png)

[Sekilas Tentang](#sekilas-tentang) | [Instalasi](#instalasi) | [Konfigurasi](#konfigurasi) [Cara Pemakaian](#cara-pemakaian) | [Pembahasan](#pembahasan) | [Referensi](#referensi)
:---: | :---: | :---: | :---: | :---: | :---:

# Sekilas Tentang
__Discourse__ adalah 100% platform diskusi _open source_ yang dibangun untuk dekade Internet berikutnya. Platform ini dapat digunakan sebagai _mailing list_, forum diskusi, ruang obrolan _long-form_, dan masih banyak lagi. __Discourse__ adalah aplikasi `JavaScript` yang berjalan di browser web, menggunakan kerangka kerja `Ember.js`. Sisi server __Discourse__ adalah `Ruby on Rails` yang didukung oleh database `Postgres` dan cache `Redis`.
Demo : https://try.discourse.org/


# Instalasi
__Discourse__ dirancang untuk internet 10 tahun ke depan, sehingga persyaratan browser minimumnya tinggi. Platform ini mendukung rilis terbaru dan stabil dari semua browser dan platform utama:
* Microsoft Edge
* Google Chrome
* Mozilla Firefox
* Apple Safari

Discourse dibangun dengan mempertimbangkan perangkat sentuh resolusi tinggi, dan secara otomatis beralih ke tata letak seluler untuk layar kecil.
* Mobile Safari, iOS 10.3+
* Chrome Seluler, Android 4.4+

Sebagai syarat instalasi awal, pastikan sudah melakukan setup VM.
### Install Discourse Dependencies

Terdapat beberapa _packages_ yang dibutuhkan untuk mempersiapkan _Rails development environment_, yang nantinya akan digunakan untuk _develop_ aplikasi. _Packages_ yang dibutuhkan terdiri dari:
* Git
* rbenv
* ruby-build
* Ruby
* Rails
* PostgreSQL
* SQLite
* Redis
* Bundler
* MailCatcher 
* ImageMagick
 
Masing-masing _packages_ diinstall secara berbeda. Namun, disini sudah tersedia script yang berisi seluruh perintah untuk menginstal masing-masing _dependencies_ tersebut: https://github.com/techAPJ/install-rails/blob/master/linux

Untuk lebih memudahkan proses instalasi, cukup dengan menjalankan perintah berikut dari terminal:
```
bash <(wget -qO- https://raw.githubusercontent.com/techAPJ/install-rails/master/linux)
```
Perintah Skrip ini disesuaikan untuk __Discourse__, dan mencakup semua _package_ yang diperlukan untuk instalasi Discourse. Dependensi __Discourse__ sudah selesai di install, kemudian akan dilanjutkan dengan menginstall platform __Discourse__ dengan langkah-langkah sebagai berikut :

#### 1. Clone Discourse
Hal pertama yang perlu dilakukan adalah melakukan cloning repositori aplikasi ke komputer lokal agar seluruh file yang aplikasi butuhkan untuk _development_ sudah ter _clone_ ke komputer kita.
```
git clone https://github.com/discourse/discourse.git 
```

#### 2. Mengatur Database
Karena sebuah aplikasi  menggunakan database dalam proses developmentnya, maka kita perlu membuat role pada PostgreSQL terlebih dahulu. Disini kami menggunakan username yang sama dengan ubuntu supaya lebih memudahkan.

Cek username ubuntu:
```
whoami
```

_Create role_:
```
sudo -u postgres createuser -s "$USERNAME_KITA"
```

#### 3. Build & Install
Pertama, kita perlu masuk ke directory tempat melakukan proses cloning repository aplikasi tadi.
```
cd discourse
```

#### 4. Install bundle
_bundle install_ merupakan sebuah perintah yang digunakan untuk menginstal _dependencies_ secara spesifik pada _gemfile_ berbasis bahasa `Ruby` yang kita butuhkan pada proyek ini.
```
source ~/.bashrc
bundle install
```

Setelah berhasil menginstal seluruh dependencies menggunakan _bundle_, langkah selanjutnya adalah menyelesaikan konfigurasi koneksi database dengan perintah berikut:
```
bundle exec rake db:create 
bundle exec rake db:migrate
RAILS_ENV=test bundle exec rake db:create db:migrate
```

#### 5. Membuat Sebuah Akun
Untuk dapat menggunakan aplikasi, tentu kita memerlukan sebuah account terlebih dahulu. Disini kita dapat membuat sebuah akun admin yang akan digunakan untuk login ke aplikasi.
```
RAILS_ENV=development bundle exec rake admin:create
```
Kita akan diminta untuk menginput email dan password (min 10 karakter).


#### 6. Menjalankan Aplikasi
Setelah semua persiapan dilakukan dengan benar, aplikasi dapat dijalankan pada server/localhost kita.
```
bundle exec rails server
```
Pada localhost secara default aplikasi akan dijalankan pada port 3000, sehingga kita dapat langsung mengakses http://localhost:3000. Untuk memberhentikan service yang sedang berjalan cukup tekan ctrl+z dari terminal, lalu close terminal dan service akan berhenti berjalan.
__Catatan:__ Setiap ingin menjalankan aplikasi, pastikan sudah masuk ke directory aplikasi lalu kembali _run_ servicenya. Setelah itu baru akses kembali http://localhost:3000. 
```
cd alamatdirektori/direktori
source ~/.bashrc
bundle exec rails server
```

#### 7. Create New Admin
Untuk membuat _new admin_, jalankan program di bawah ini:
```
RAILS_ENV=development bundle exec rake admin:create
```
Melakukan konfigurasi email:
```
Run MailCatcher:
mailcatcher --http-ip 0.0.0.0
```

# Konfigurasi 
### Setup VM 
Kami menggunakan VM dengan sistem operasi `Ubuntu 20.04 LTS Desktop` untuk melakukan proses instalisasi aplikasi. Karena aplikasi dioperasikan pada local computer, kami menginstal `localhost` dan `PHP` dengan tahapan berikut,

__Step 1 — Install Apache__
Memperbarui indeks _local package_:
```
$ sudo apt update
```
Install _package_ `apache2`:
```
$ sudo apt install apache2
```
Setelah mengkonfirmasi penginstalan, `apt` akan menginstal _Apache_ dan semua dependensi yang diperlukan.

__Step 2 — Menyesuaikan Firewall__
Selama instalasi, Apache mendaftarkan dirinya dengan UFW untuk menyediakan beberapa profil aplikasi yang dapat digunakan untuk mengaktifkan atau menonaktifkan akses ke Apache melalui firewall.

List profil aplikasi UFW:
```
$ sudo ufw app list
```
Sebaiknya aktifkan profil yang paling ketat yang masih memungkinkan traffic yang telah dikonfigurasikan. Karena disini kami belum mengkonfigurasi SSL untuk server, kami hanya perlu mengizinkan traffic pada port 80:
```
$ sudo ufw allow 'Apache'
```
Verifikasi perubahan:
```
$ sudo ufw status
```
Pada output yang ditampilkan, profil tersebut telah diaktifkan untuk mengizinkan akses ke server web.

Selanjutnya, kami melakukan konfigurasi SSH dan _port-forwarding_ dari server ke host windows. Pada server, kami install dan enable konfigurasi SSH nya:
```
$ sudo apt install openssh-server
$ sudo systemctl enable ssh
$ sudo systemctl start ssh
$ sudo ufw allow ssh
$ sudo ufw enable
```

Pada virtual-box, kita terapkan aturan port forwarding sebagai berikut:
![port-forwarding](https://user-images.githubusercontent.com/60083980/111300227-6b104c00-8683-11eb-9649-68172c05b270.png)

Server sudah dapat diakses melalui SSH dari windows dan menginstal aplikasi.
*gambar*



# Maintenance (opsional)
Setting tambahan untuk maintenance secara periodik, misalnya:
* buat backup database tiap pekan
* hapus direktori sampah tiap hari
* dll

# Otomatisasi (opsional)
Skrip shell untuk otomatisasi instalasi, konfigurasi, dan maintenance.


# Cara Pemakaian
Setelah seluruh tahapan pada bagian __Instalasi__ sudah berhasil dilakukan, aplikasi __Discourse__ sudah bisa dijalankan dan digunakan pada web browser. Cara pemakaian dari aplikasi ini sebagai berikut:

1. Pada saat melakukan akses ke http://localhost:3000, hasil yang akan ditampilkan pertama kali seperti di bawah ini merupakan halaman awal pada saat belum melakukan _login_ ke dalam aplikasi __Discourse__. Untuk melakukan proses _login_ ataupun medaftarkan akun baru, bisa dengan memilih tombol __Sign Up__ atau __Log In__ di kanan atas.
![1_halaman_awal](https://user-images.githubusercontent.com/60083980/111376353-ee588e80-86d1-11eb-8ee4-737bb395ff8a.png)

2. Gambar di bawah merupakan tampilan menu _login_ pada aplikasi __Discourse__. 
![3_login](https://user-images.githubusercontent.com/60083980/111376493-17791f00-86d2-11eb-83ec-91fe1bc94501.png)

3. Jika sudah memiliki akun dan berhasil _login_, maka tampilan utama aplikasi akan seperti gambar di bawah. Kita dapat membuat sebuah topik diskusi baru, dimana topik yang kita buat nanti akan ditampilkan pada halaman ini.
![2_tampilan_utama](https://user-images.githubusercontent.com/60083980/111376365-f3b5d900-86d1-11eb-8b78-e7e737c3b8b2.png)

4. Saat membuat sebuah akun kemudian melakukan _login_, terdapat sebuah pop up pada bagian atas tampilan utama aplikasi yang memiliki tulisan berwarna biru bertuliskan _the setup wizard_. Jika kita klik tulisan tersebut, maka akan diarahkan ke tampilan dimana kita bisa melakukan kustomisasi tampilan dan pengaturan aplikasi __Discourse__.
![4_setup1](https://user-images.githubusercontent.com/60083980/111376524-22cc4a80-86d2-11eb-8e25-3c8b3e9cae8c.png)

5. Gambar di bawah ini merupakan tampilan baru setelah melakukan kustomisasi aplikasi. Pada gambar ini, tampilan yang berbeda hanya ada di bagian atas aplikasi yang berubah menjadi warna hitam.
![5_tampilan_update](https://user-images.githubusercontent.com/60083980/111376535-265fd180-86d2-11eb-9ab8-6af027b37ff6.png)

6. Untuk membuat sebuah topik baru, kita bisa klik pilihan bertuliskan __+ New Topic__ pada tampilan utama aplikasi. Akan muncul tampilan seperti di bawah ini, dimana kita bisa menuliskan isi dari topik diskusi yang ingin dibuat. Jika sudah selesai bisa klik pilihan __+ Create Topic__.
![9_new_topic](https://user-images.githubusercontent.com/60083980/111376540-28c22b80-86d2-11eb-937e-42d5a62cfb28.png)

7. Apabila berhasil membuat topik baru, maka akan muncul tampilan seperti gambar di bawah. Pada tampilan dibawah, topik diskusi yang ada/dibuat dapat diedit, share, dan juga kita dapat me-_reply_ topik diskusi tersebut.
![10_new_topic2](https://user-images.githubusercontent.com/60083980/111376547-29f35880-86d2-11eb-80a0-e5b69f1fa1fd.png)

8. Pada halaman utama aplikasi, kita bisa melakukan _sort_ ataupun _filter_ terhadap topik diskusi yang ingin kita tampilkan. Beberapa cara diantaranya yaitu melakukan _sort_ berdasarkan __Latest__ dan __Top__, dan melakukan _filter_ berdasarkan __Categories__. 
![11_sort_filter](https://user-images.githubusercontent.com/60083980/111376551-2bbd1c00-86d2-11eb-89b1-528ffb54b8cc.png)

9. Aplikasi __Discourse__ juga memiliki beberapa fitur dan menu yang dapat digunakan pada bagian kanan atas tampilan halaman utamanya. Fitur dan menu tersebut yaitu, fitur pencarian yang bisa digunakan untuk mencari topik, _post, user_, atau kategori yang kita inginkan. Selanjutnya ada menu yang berisi beberapa pilihan seperti _Settings, Users, Groups, Categories_. Pada bagian ini juga terdapat menu yang berhubungan dengan akun kita.
![12_search](https://user-images.githubusercontent.com/60083980/111376555-2d86df80-86d2-11eb-9e53-bcd2c2d64c4a.png)
![13_setting](https://user-images.githubusercontent.com/60083980/111376558-2f50a300-86d2-11eb-8b67-886f9fc13612.png)
![14_profile](https://user-images.githubusercontent.com/60083980/111376570-311a6680-86d2-11eb-9ad8-5e6acf848654.png)


# Pembahasan
* Pendapat anda tentang aplikasi web ini
  * kelebihan <p>
  Kelebihan aplikasi platform discourse adalah aplikasi ini dapat melakukan setup secara penuh mulai dari cek traffic dan API, set tampilan (tema, font, logo dll), jadi seluruh   konfigurasi yang kita lakukan dapat memiliki kontrol penuh terhadap aplikasi platform yang dijalankan. </p>
  * kekurangan
  -
* Bandingkan dengan aplikasi web lain yang sejenis

Kelebihan | Kekurangan
--------- | ----------
Content cell 1 | Content cell 2
Content2 cell 1 | Content2 cell 2
Content3 cell 1 | Content3 cell 2

### Discourse VS EpochTalk
Ini contoh perbandingan aplikasi web lain yang sejenis ya.


# Referensi
[What is Discourse?](https://www.discourse.org/about) - Discourse <br>
[Install Ubuntu VirtualBox](https://linuxhint.com/install_ubuntu_virtualbox_2004/) - linuxhint <br>
[Install the Apache Web Server on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04) - DigitalOcean
