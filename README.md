# Jarkom-Modul-3-A13-2023

## Laporan Resmi Pengerjaan Praktikum Jaringan Komputer

| No. | Anggota               | NRP          |
|-----|----------------------------|--------------|
| 1   | Aida Fitrania Prabasati    | 5025211033   |
| 2   | Rule Lulu Damara           | 5025211050   |

## Topologi

## Network Configuration
- Aura ( DHCP Relay )
  ```bash
  # DHCP config for eth0
  auto eth0
  iface eth0 inet dhcp
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.175.0.0/16
  
  auto eth1
  iface eth1 inet static
  	address 192.175.1.11
  	netmask 255.255.255.0
  
  auto eth2
  iface eth2 inet static
  	address 192.175.2.11
  	netmask 255.255.255.0
  
  
  auto eth3
  iface eth3 inet static
  	address 192.175.3.11
  	netmask 255.255.255.0
  
  auto eth4
  iface eth4 inet static
  	address 192.175.4.11
  	netmask 255.255.255.0

  ```
- Himmel ( DHCP Server )
   ```bash
  auto eth0
  iface eth0 inet static
  	address 192.175.1.2
  	netmask 255.255.255.0
  	gateway 192.175.1.11

  ```
- Heiter ( DNS Server )
   ```bash
  auto eth0
  iface eth0 inet static
  	address 192.175.1.3
  	netmask 255.255.255.0
  	gateway 192.175.1.11
  ```
- Denken ( Database Server )
   ```bash
  
  ```
- Eisen ( Load Balancer )
   ```bash

  ```
- Frieren ( Laravel Worker )
   ```bash

  ```
- Flamme ( Laravel Worker )
   ```bash

  ```
- Fern ( Laravel Worker )
   ```bash

  ```
- Lawine ( PHP Worker )
   ```bash

  ```
- Linie ( PHP Worker )
   ```bash

  ```
- Lugner ( PHP Worker )
   ```bash

  ```
- Revolte, Richter, Sein, dan Stark (Client)
   ```bash

  ```
## Langkah Awal
Inisiasi di setiap node

- DNS Server
  ```bash
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf
  apt-get update
  apt-get install bind9 -y
  ```
- DHCP Server
  ```bash
  echo 'nameserver 192.175.1.2' > /etc/resolv.conf    
  apt-get update
  apt install isc-dhcp-server -y
  ```
- DHCP Relay
  ```bbash
  apt-get update
  apt install isc-dhcp-relay -y
  ```
- Database Server
  ```bash
  echo 'nameserver 192.175.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install mariadb-server -y
  service mysql start
  
  Ganti [bind-address] pada file /etc/mysql/mariadb.conf.d/50-server.cnf menjadi 0.0.0.0
  Restart mysql kembali
  ```
- Load Balancer
  ```bash
  echo 'nameserver 192.175.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install apache2-utils -y
  apt-get install nginx -y
  apt-get install lynx -y

  service nginx start
  ```
- PHP Worker
  ```bash
  echo 'nameserver 192.175.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install nginx -y
  apt-get install wget -y
  apt-get install unzip -y
  apt-get install lynx -y
  apt-get install htop -y
  apt-get install apache2-utils -y
  apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y

  service nginx start
  service php7.3-fpm start
  ```
- Laravel Worker
  ```bash
  echo 'nameserver 192.175.1.2' > /etc/resolv.conf
  apt-get update
  apt-get install lynx -y
  apt-get install mariadb-client -y
  # Test connection from worker to database
  apt-get install -y lsb-release ca-certificates a   apt-transport-https software-properties-common gnupg2
  curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
  sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
  apt-get update
  apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
  apt-get install nginx -y
  
  service nginx start
  service php8.0-fpm start
  ```
- Client
  ```bash
  apt update
  apt install lynx -y
  apt install htop -y
  apt install apache2-utils -y
  apt-get install jq -y
  ```

## SOAL 1
Siapkan konfigurasi topologi dan setup seperti diatas. 

Selanjutnya menambahkan register domain berupa riegel.canyon.a13.com untuk Laravel Worker dan granz.channel.a13.com untuk PHP Worker yang mengarah pada worker dengan IP 192.175.x.1. 

Karena pada konfigurasi topologi sebelumnya seluruh worker sudah menggunakan DHCP, maka modifikasi sedikit pada node Lugner dan Fern seperti dibawah ini
- Lugner ( PHP Worker )
  ```bash
  ```
- Fern ( Laravel Worker )
  ```bash
  ```
Selanjutnya pada DNS Server (Heiter), jalankan command dibawah ini
- Heiter ( DNS Server )
  ```bash
  ```
Hasil 


## SOAL 2
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

Lakukan setup pada DHCP Server terlebih dahulu, lalu jalankan command dibawah ini
- DHCP Server
  ```bash
  ```

## SOAL 3
Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

Tambahkan beberapa konfigurasi baru untuk switch4 sesuai dengan command dibawah ini
```bash
```

## SOAL 4
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

Menambahkan beberapa konfigurasi seperti option broadcast-address dan option domain-name-server agar DNS yang telah disiapkan sebelumnya dapat digunakan
```bash
```

Lakukan setup pada DHCP Relay, lalu jalankan command dibawah ini
- DHCP Relay
  ```bash
  ```

Pada file /etc/sysctl.conf uncomment net.ipv4.ip_forward=1

Restart semua Client

Hasil

## SOAL 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

Menggunakan fungsi default-lease-time dan max-lease-team dimana satuannya adalah detik.

Karena pada switch3 dapat meminjamkan IP selama 3 Menit dan Switch4 dapat meminjamkan IP selama 12 Menit. Sehingga pada Switch3 membutuhkan waktu 180 s dan Switch4 membutuhkan waktu 720 s dan untuk max-lease-time nya adalah 96 menit dimana akan menjadi 5760 s

Tambahkan beberapa konfigurasi baru untuk mengatur leasing time pada switch3 dan switch4 sesuai dengan aturan soal. 

Lalu jalankan command berikut
- DHCP Server
  ```bash
  ```

Hasil

## SOAL 6
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. 


## SOAL 7
Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

