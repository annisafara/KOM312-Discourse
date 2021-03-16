![Discourse_logo](https://user-images.githubusercontent.com/60083980/111193101-63519880-85ec-11eb-9133-f9f6cc20def9.png)

# Sekilas Tentang
__Discourse__ adalah 100% platform diskusi _open source_ yang dibangun untuk dekade Internet berikutnya. Platform ini dapat digunakan sebagai _mailing list_, forum diskusi, ruang obrolan _long form_, dan masih banyak lagi. __Discourse__ adalah aplikasi `JavaScript` yang berjalan di browser web, menggunakan kerangka kerja `Ember.js`. Sisi server __Discourse__ adalah `Ruby on Rails` yang didukung oleh database `Postgres` dan cache `Redis`.
demo : https://try.discourse.org/

# Instalasi
Pastikan sudah setup VM sebagai syarat instalasi awal :
### Setup VM 
Kami menggunakan VM dengan sistem operasi Ubuntu 20.04 LTS  Desktop untuk melakukan proses instalisasi aplikasi. proses install && setup VM sendiri kami lakukan dengan mengikuti langkah langkah pada tutorial berikut :
https://linuxhint.com/install_ubuntu_virtualbox_2004/

Kemudian, karena aplikasi dioperasikan pada local computer, kita menginstall localhost dan php berdasarkan tutorial berikut :
https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-18-04

Lalu, kita melakukan konfigurasi SSH dan port-forwarding dari server ke host windows kita.
pada server, kita install dan enable konfigurasi SSH nya:
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
sudo ufw allow ssh
sudo ufw enable

pada virtual-box, kita terapkan aturan port forwarding sebagai berikut :
*gambar*

Setelahnya kita dapat mengakses server melalui SSH dari windows, dan menginstall aplikasi.
*gambar*

### Install Discourse Dependencies

Terdapat beberapa packages, yang kita butuhkan untuk mempersiapkan Rails development environment, yang nantinya akan digunakan untuk develop aplikasi. Terdiri dari :

*Git 
rbenv 
ruby-build 
Ruby 
Rails 
PostgreSQL 
SQLite 
Redis 
Bundler 
MailCatcher 
ImageMagick*

Masing masing packages diinstall secara berbeda. Namun, disini sudah tersedia script yang berisi seluruh perintah untuk menginstall masing masing seluruh dependencies tersebut : https://github.com/techAPJ/install-rails/blob/master/linux .

Untuk lebih memudahkan menginstallnya, kita cukup menjalankan perintah berikut dari terminal :
bash <(wget -qO- https://raw.githubusercontent.com/techAPJ/install-rails/master/linux)

Perintah Skrip ini disesuaikan untuk Discourse, dan mencakup semua paket yang diperlukan untuk instalasi Discourse.
Sekarang kita telah menginstal dependensi Discourse, mari kita lanjutkan untuk menginstal Discourse itu sendiri.

### 1. Clone Discourse ###
git clone https://github.com/discourse/discourse.git 

### 2.Setup Database ###
Karena aplikasi berbasis / menggunakan database dalam proses development nya, maka kita perlu membuat sebuah role terlebih dahulu. Disini, kita menggunakan username yang sama dengan ubuntu kita agar lebih mudah.

Cek username ubuntu :
whoami

Create role :
sudo -u postgres createuser -s "$USERNAME_KITA"

### 3.Build && Install ###
Pertama, kita tentu perlu masuk ke directory tempat kita cloning repository aplikasi tadi.
cd discourse


### 4. Install bundle ###
bundle install merupakan sebuah perintah yang kita gunakan untuk menginstall dependencies secara spesifik pada gemfile berbasis bahasa Ruby yang kita butuhkan pada projek ini.
source ~/.bashrc
bundle install


Setelah kita telah berhasil menginstall seluruh dependencies menggunakan bundle, kita lanjut menyelesaikan konfigurasi koneksi database dengan perintah berikut :
bundle exec rake db:create 
bundle exec rake db:migrate
RAILS_ENV=test bundle exec rake db:create db:migrate


### 5. Make an Account ###
Untuk dapat menggunakan aplikasi, tentu kita memerlukan sebuah account terlebih dahulu. Disini kita dapat membuat sebuah akun admin, yang akan kita gunakan untuk login aplikasi.
RAILS_ENV=development bundle exec rake admin:create

Kita akan diminta untuk menginput email & password ( min 10 karakter ).


### 6. Running the Application ###
Setelah semua persiapan dilakukan dengan benar, kita dapat menjalankan aplikasi pada server / localhost kita.
bundle exec rails server

Pada localhost, secara default aplikasi akan dijalankan pada port 3000, sehingga kita dapat langsung mengakses http://localhost:3000 .
Untuk memberhentikan service yang sedang berjalan cukup ctrl + z dari terminal, lalu close terminal, dan service akan berhenti running.
*note :
tiap ingin run aplikasi, pastikan sudah masuk ke directory aplikasi, lalu kembali run servicenya. baru akses kembali http://localhost:3000 
cd alamatdirektori/direktori
source ~/.bashrc
bundle exec rails server

### 7. Create New Admin ###

Untuk membuat new admin, jalankan program ini:

RAILS_ENV=development bundle exec rake admin:create

konfigurasi email :
Run MailCatcher:

mailcatcher --http-ip 0.0.0.0

selesai




__Discourse__ dirancang untuk internet 10 tahun ke depan, sehingga persyaratan browser minimumnya tinggi. Platform ini mendukung rilis terbaru dan stabil dari semua browser dan platform utama:
* Microsoft Edge
* Google Chrome
* Mozilla Firefox
* Apple Safari

Discourse dibangun dengan mempertimbangkan perangkat sentuh resolusi tinggi, dan secara otomatis beralih ke tata letak seluler untuk layar kecil.
* Mobile Safari, iOS 10.3+
* Chrome Seluler, Android 4.4+

*Langkah instalasi dalam CLI.*

# Konfigurasi (opsional)
Setting server tambahan yang diperlukan untuk meningkatkan fungsi dan kinerja aplikasi, misalnya:
* batas upload file
* batas memori
* dll

Plugin untuk fungsi tambahan
* login dengan Google/Facebook
* editor Markdown
* dll

# Maintenance (opsional)
Setting tambahan untuk maintenance secara periodik, misalnya:
* buat backup database tiap pekan
* hapus direktori sampah tiap hari
* dll

# Otomatisasi (opsional)
Skrip shell untuk otomatisasi instalasi, konfigurasi, dan maintenance.

# Cara Pemakaian
* Tampilan aplikasi web
* Fungsi-fungsi utama
* Isi dengan data real/dummy (jangan kosongan) dan sertakan beberapa screenshot

# Pembahasan
* Pendapat anda tentang aplikasi web ini
  * kelebihan
  * kekurangan
* Bandingkan dengan aplikasi web lain yang sejenis

# Referensi
[What is Discourse?](https://www.discourse.org/about) - Discourse
