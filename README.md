# Jarkom-Modul-3-A13-2023

## Laporan Resmi Pengerjaan Praktikum Jaringan Komputer

| No. | Anggota               | NRP          |
|-----|----------------------------|--------------|
| 1   | Aida Fitrania Prabasati    | 5025211033   |
| 2   | Rule Lulu Damara           | 5025211050   |

## Topologi

![Screenshot 2023-11-19 205644](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/117fd39b-d381-4c9f-a621-f1dc0e7a4fc2)

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
  auto eth0
  iface eth0 inet static
	address 192.175.4.2
	netmask 255.255.255.0
	gateway 192.175.4.11
  
  ```
- Eisen ( Load Balancer )
   ```bash
  auto eth0
  iface eth0 inet static
	address 192.175.4.3
	netmask 255.255.255.0
	gateway 192.175.4.11 
  
  ```
- Frieren ( Laravel Worker )
   ```bash
  auto eth0
  iface eth0 inet static
	address 192.175.4.1
	netmask 255.255.255.0
	gateway  192.175.4.11

  ```
- Flamme ( Laravel Worker )
   ```bash
  auto eth0
  iface eth0 inet static
	address 192.175.4.2
	netmask 255.255.255.0
	gateway  192.175.4.11
  ```
- Fern ( Laravel Worker )
   ```bash
  auto eth0
  iface eth0 inet static
	address 192.175.4.3
	netmask 255.255.255.0
	gateway 192.175.4.11

  ```
- Lawine ( PHP Worker )
   ```bash
  auto eth0
  iface eth0 inet static
	address  192.175.3.1
	netmask 255.255.255.0
	gateway  192.175.3.11

  ```
- Linie ( PHP Worker )
   ```bash
  iface eth0 inet static
	address 192.175.3.2
	netmask 255.255.255.0
	gateway 192.175.3.11

  ```
- Lugner ( PHP Worker )
   ```bash
  auto eth0
  iface eth0 inet static
	address 192.175.3.3
	netmask 255.255.255.0
	gateway 192.175.3.11

  ```
- Revolte, Richter, Sein, dan Stark (Client)
  ```bash
  auto eth0
  iface eth0 inet dhcp
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
  apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl     
  php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y

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
```text
Siapkan konfigurasi topologi dan setup seperti diatas.
```

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
  echo '
  zone "canyon.a13.com" {
  	type master;
  	file "/etc/bind/jarkom/canyon.a13.com";
  };
  
  zone "channel.a13.com"{
  	type master;
  	file "/etc/bind/jarkom/channel.a13.com";
  };'  > /etc/bind/named.conf.local
  
  mkdir -p /etc/bind/jarkom
  cp /etc/bind/db.local /etc/bind/jarkom/canyon.a13.com
  cp /etc/bind/db.local /etc/bind/jarkom/channel.a13.com
  
  echo '
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA    canyon.a13.com. root.canyon.a13.com. (
                                2         ; Serial
                           604800    ; Refresh
                            86400     ; Retry
                          2419200   ; Expire
                           604800 )    ; Negative Cache TTL
  ;
  @       IN      NS     canyon.a13.com.
  @       IN      A       192.175.1.3; 
  riegel  IN      A       192.175.4.1' > /etc/bind/jarkom/canyon.a13.com
  
  echo '
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA    channel.a13.com. root.channel.a13.com. (
                                2         ; Serial
                           604800    ; Refresh
                            86400     ; Retry
                          2419200   ; Expire
                           604800 )    ; Negative Cache TTL
  ;
  @       IN      NS     channel.a13.com.
  @       IN      A       192.175.1.3; 
  granz  IN      A       192.175.3.1' > /etc/bind/jarkom/channel.a13.com
  
  echo 'options {
          directory "/var/cache/bind";
  
          forwarders {
                  192.168.122.1;
          };
  
          // dnssec-validation auto;
          allow-query{any;};
          auth-nxdomain no;    # conform to RFC1035
          listen-on-v6 { any; };
  }; ' >/etc/bind/named.conf.options
  service bind9 start
  
  ```
  
Output

![image](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/b9cddbaa-0417-49c0-b196-fe938b3d2e3f)

![image](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/e4452210-3f35-4e9a-9cdf-518ee8a3becc)

## SOAL 2
```text
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80
```

Lakukan setup pada DHCP Server terlebih dahulu, lalu jalankan command dibawah ini
- DHCP Server
  ```bash
  echo 'subnet 192.175.1.0 netmask 255.255.255.0 {
  }
  
  subnet 192.175.2.0 netmask 255.255.255.0 {
  }
  
  subnet 192.175.3.0  netmask 255.255.255.0 {
      range 192.175.3.16 192.175.3.32;
      range 192.175.3.64 192.175.3.80;
      option routers 192.175.3.0;
      option broadcast-address 192.175.3.255;
      option domain-name-servers 192.175.1.3;
  }'>  /etc/dhcp/dhcpd.conf
  
  service isc-dhcp-server restart
  ```

## SOAL 3
```text
Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168
```

Tambahkan beberapa konfigurasi baru untuk switch4 sesuai dengan command dibawah ini
```bash
echo 'subnet 192.175.1.0 netmask 255.255.255.0 {
}

subnet 192.175.2.0 netmask 255.255.255.0 {
}

subnet 192.175.3.0  netmask 255.255.255.0 {
    range 192.175.3.16 192.175.3.32;
    range 192.175.3.64 192.175.3.80;
    option routers 192.175.3.0;
    option broadcast-address 192.175.3.255;
    option domain-name-servers 192.175.1.3;
}

subnet 192.175.4.0 netmask 255.255.255.0 {
    range 192.175.4.12 192.175.4.20;
    range 192.175.4.160 192.175.4.168;
    option routers 192.175.4.0;
    option broadcast-address 192.175.4.255;
    option domain-name-servers 192.175.1.3;
}'>  /etc/dhcp/dhcpd.conf

```

## SOAL 4
```text
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut
```

Lakukan setup pada DHCP Relay, lalu jalankan command dibawah ini
- DHCP Relay
  ```bash
  echo '# Defaults for isc-dhcp-relay initscript
  # sourced by /etc/init.d/isc-dhcp-relay
  # installed at /etc/default/isc-dhcp-relay by the maintainer scripts
  
  #
  # This is a POSIX shell fragment
  #
  
  # What servers should the DHCP relay forward requests to?
  SERVERS="192.175.1.2" 
  
  # On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
  INTERFACES="eth1 eth2 eth3 eth4"
  
  # Additional options that are passed to the DHCP relay daemon?
  OPTIONS=""' > /etc/default/isc-dhcp-relay
  
  service isc-dhcp-relay start 
  
  ```

Config pada /etc/sysctl.conf juga untuk enable ip4 forwarding dengan cara uncomment net.ipv4.ip_forward=1


## SOAL 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

Menggunakan fungsi default-lease-time dan max-lease-team dimana satuannya adalah detik.

Karena pada switch3 dapat meminjamkan IP selama 3 Menit dan Switch4 dapat meminjamkan IP selama 12 Menit. Sehingga pada Switch3 membutuhkan waktu 180 s dan Switch4 membutuhkan waktu 720 s dan untuk max-lease-time nya adalah 96 menit dimana akan menjadi 5760 s

Tambahkan beberapa konfigurasi baru untuk mengatur leasing time pada switch3 dan switch4 sesuai dengan aturan soal. 

Lalu jalankan command berikut
- DHCP Server
  ```bash
  echo 'subnet 192.175.1.0 netmask 255.255.255.0 {
  }
  
  subnet 192.175.2.0 netmask 255.255.255.0 {
  }
  
  subnet 192.175.3.0  netmask 255.255.255.0 {
      range 192.175.3.16 192.175.3.32;
      range 192.175.3.64 192.175.3.80;
      option routers 192.175.3.0;
      option broadcast-address 192.175.3.255;
      option domain-name-servers 192.175.1.3;
      default-lease-time 180;
      max-lease-time 5760;
  }
  
  subnet 192.175.4.0 netmask 255.255.255.0 {
      range 192.175.4.12 192.175.4.20;
      range 192.175.4.160 192.175.4.168;
      option routers 192.175.4.0;
      option broadcast-address 192.175.4.255;
      option domain-name-servers 192.175.1.3;
      default-lease-time 720;
      max-lease-time 5760;
  }'>  /etc/dhcp/dhcpd.conf
  
  service isc-dhcp-server restart
  
  ```

  ```bash
  echo '
  host Lawine{
  	hardware ethernet 3a:6e:fb:2b:7c:49;
  	fixed-address 192.175.3.1;
  }
  host Linie{
  	hardware ethernet 26:5a:5e:bf:ed:3e;
  	fixed-address 192.175.3.2;
  }
  host Lugner{
  	hardware ethernet 52:5c:79:c8:38:cc;
  	fixed-address 192.175.3.3;
  }
  
  host Frieren{
  	hardware ethernet e6:d8:ec:d9:81:f1;
  	fixed-address 192.175.4.1;
  }
  host Flamme{
  	hardware ethernet 8e:71:84:43:41:c3;
  	fixed-address 192.175.4.2;
  }
  host Fern{
  	hardware ethernet 0e:c9:32:93:28:f1;
  	fixed-address 192.175.4.3;
  }' > /etc/dhcp/dhcpd.conf
  ```

Lalu ubah configuration pada worker sebagai berikut :
auto eth0
iface eth0 inet dhcp
hwaddress ether [address]

Hasil No 2-5

![Screenshot 2023-11-16 082235](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/ad675825-2bd7-4ac1-b899-b85b49024538)

![Screenshot (1715)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/d3f556fa-c709-4a76-a340-2824a182e860)

![Screenshot (1717)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/9ca2384a-5fd8-4e79-9cd6-aaa841ec80b3)

## SOAL 6
```text
Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. 
```
Pertama-tama lakukan setup berikut

- Pada semua worker PHP
```bash
apt update
apt install nginx -y
service nginx start
```

- Pada semua client
```bash
apt update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt install nginx -y
```

Masuk ke dalam folder /var/www dan jalankan berikut
```bash
git clone -b granz https://github.com/ruleluludamara/JarkomPrak3-Dependencies
mv JarkomPrak3-Dependencies /var/www/granz.channel.a02.com
```

Setelah itu melakukan konfigurasi pada nginx pada scrip ```bash no6.sh```
```bash

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.a13.com
ln -s /etc/nginx/sites-available/granz.channel.a13.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/granz.channel.a13.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;  # Sesuaikan versi PHP dan socket
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/granz.channel.a13.com

service nginx restart

```
Output 
Jalankan Perintah lynx localhost pada masing-masing worker dan hasilnya akan sebagai berikut:

![Screenshot1753](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/c5abb0e3-3dea-4a33-9ad5-0a609c22dfe3)

## SOAL 7
```text
Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.
```

Lakukan configurasi Load Balancing pada node Eisen sebagain berikut
- Start kembali Node DNS Server dan arakah domain pada IP Load Balancer Eisen
```bash
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.a13.com. root.riegel.canyon.a13.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.a13.com.
@       IN      A       192.173.2.1     ; 
www     IN      CNAME   riegel.canyon.a13.com.' > /etc/bind/sites/riegel.canyon.a13.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.a13.com. root.granz.channel.a13.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.a13.com.
@       IN      A       192.173.2.1     ; 
www     IN      CNAME   granz.channel.a13.com.' > /etc/bind/sites/granz.channel.a13.com
```

Lalu pada node Eisen lakuakn konfigurasi pada nginx sebagain berikut 
```bash
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
    server 192.175.3.1;
    server 192.175.3.2;
    server 192.175.3.3;
}

server {
    listen 80;
    server_name granz.channel.a13.com www.granz.channel.a13.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart
```

Output 
Jalankan perintah berikut pada client ```bash Ritcher```
```bash
ab -n 1000 -c 100 http://www.granz.channel.a13.com/
```
![Screenshot (1728)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/faf7960d-da75-4ffe-9007-a254a7718636)

![Screenshot (1729)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/765b2358-5ead-48ec-b462-c590bbeb2fc9)


## SOAL 8
```text
Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
Nama Algoritma Load Balancer
Report hasil testing pada Apache Benchmark
Grafik request per second untuk masing masing algoritma. 
Analisis (8)
```

Laporan grimoire terdapat pada link berikut 
https://www.canva.com/design/DAF0VS6hHBk/-gZEQF93OjxBX0b2eMIuqQ/edit?utm_content=DAF0VS6hHBk&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton


Jalankan command berikut pada client Revolte
```bash
ab -n 200 -c 10 http://www.granz.channel.a13.com/ 
```

Output 
Round robin
![Screenshot (1734)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/b3e019ca-65ce-4195-9e90-8122376e0a1c)

Least-connection

![Screenshot (1735)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/ee49e41b-6414-4ec7-8945-ac643e63b8af)

IP Hash

![Screenshot (1736)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/7343538a-fadc-4941-be6f-183ba9139b24)

Generic Hash

![Screenshot (1737)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/1ed671b1-431d-45c2-82f6-97b08e75e8a8)

## SOAL 9
```text
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire. (9)
```
Setelah melakukan setup pada node Eisen sekarang lakukan testing pada load balancer yang telah dibuat sebelumnya. Yang menjadi pembeda adalah kita harus melakukan testing menggunakan 1 worker, 2 worker, dan 3 worker.

Jalankan command berikut pada client Revolte
```bash ab -n 200 -c 10 http://www.granz.channel.a13.com/ ```

Output 
1 Worker

![Screenshot (1742)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/0ece3932-82ef-47fe-8604-106611d2594e)


2 worker

![Screenshot (1747)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/f053c6d4-c056-4b7b-93d6-3e062b4205d0)

3 worker

![Screenshot (1749)](https://github.com/RuleLuluDamara/Jarkom-Modul-3-A13-2023/assets/105763198/6f830986-d77a-4c30-a848-b6bd49bb3987)


