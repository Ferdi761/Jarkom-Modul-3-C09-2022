# LAPRES JARKOM C09 MODUL 3 2022 
### Modul 3: DHCP & Proxy Server
**Anggota:**
- 5025201089 	[Andi Muhammad Rafli] 
- 5025201175 	[Adinda Zahra Pamuji]
- 5025201245 	[Achmad Ferdiansyah]

## Edit Konfigurasi Topologi
![messageImage_1668426672010](https://user-images.githubusercontent.com/102727966/201653357-06f8b91b-54dc-4cc3-8ed4-becf92e849ab.jpg)

- Ostania (Router dan DHCP Relay)
```
auto eth0
iface eth0 inet dhcp
auto eth1
iface eth1 inet static
	address 10.14.1.1
	netmask 255.255.255.0
auto eth2
iface eth2 inet static
	address 10.14.2.1
	netmask 255.255.255.0
auto eth3
iface eth3 inet static
	address 10.14.3.1
	netmask 255.255.255.0
```
- SSS (Client)
```
auto eth0
iface eth0 inet dhcp
```

- Garden (Client)
```
auto eth0
iface eth0 inet dhcp
```

- Wise (DNS Server)
```
auto eth0
iface eth0 inet static
	address 10.14.2.2
	netmask 255.255.255.0
	gateway 10.14.2.1
```

- Berlint (Proxy Server)
```
auto eth0
iface eth0 inet static
	address 10.14.2.3
	netmask 255.255.255.0
	gateway 10.14.2.1
```


- Westalis (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 10.14.2.4
	netmask 255.255.255.0
	gateway 10.14.2.1
```

- Eden (Client)
```
auto eth0
iface eth0 inet dhcp
```

- NewstonCastle (Client)
```
auto eth0
iface eth0 inet dhcp
```

- KemonoPark (Client)
```
auto eth0
iface eth0 inet dhcp
```

## Nomor 1 dan 2

### Soal

<img width="606" alt="Screenshot 2022-11-14 at 7 04 47 PM" src="https://user-images.githubusercontent.com/102727966/201655943-af824457-9281-434c-a66e-6f9a5fac64c4.png">

#### Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server (1)
### Cara Pengerjaan

DNS Server (WISE)

WISE sebagai DNS Server perlu menginstall `bind9`.
```
apt-get update
apt-get install bind9 -y
```
DHCP Server (Westalis)

Westalis sebagai DHCP Server perlu menginstall `isc-dhcp-server`.

```
apt-get update
apt-get install isc-dhcp-server -y
```

Proxy Server (Berlint)

Berlint sebagai Proxy Server perlu menginstall `squid`.
```
apt-get update
apt-get install squid -y
```

#### dan Ostania sebagai DHCP Relay (2). Loid dan Franky menyusun peta tersebut dengan hati-hati dan teliti.

DHCP Relay (Ostania)

Ostania sebagai DHCP Relay perlu menginstall `isc-dhcp-relay`.
```
apt-get update
apt-get install isc-dhcp-relay -y
```

## Nomor 3

### Soal

Ada beberapa kriteria yang ingin dibuat oleh Loid dan Franky, yaitu:
Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155!

Pada DHCP server (Westalis) dilakukan konfigurasi server pada file `/etc/dhcp/dhcpd.conf` sebagai berikut

```
echo 'subnet 10.14.2.0 netmask 255.255.255.0 {
    option routers 10.14.2.1;
    }

    subnet 10.14.1.0 netmask 255.255.255.0 {
    range 10.14.1.50 10.14.1.88;
    range 10.14.1.120 10.14.1.155;
    option routers 10.14.1.1;
    option broadcast-address 10.14.1.255;
    option domain-name-servers 10.14.2.2;
    default-lease-time 300;
    max-lease-time 6900;
    }
```

Pada subnet `10.14.1.0` sudah ditentukan range IP dari `10.14.1.50 - 10.14.1.88` dan `10.14.1.120 - 10.14.1.155`

## Nomor 4

### Soal

Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85!

### Cara Pengerjaan

Pada DHCP Server (Westalis) pada file `/etc/dhcp/dhcpd.conf` menambahkan konfigurasi sebagai berikut

```bash
subnet 10.14.3.0 netmask 255.255.255.0 {
    range 10.14.3.10 10.14.3.30;
    range 10.14.3.60 10.14.3.85;
    option routers 10.14.3.1;
    option broadcast-address 10.14.3.255;
    option domain-name-servers 10.14.2.2;
    default-lease-time 600;
    max-lease-time 6900;
    }
}
```

Pada subnet `10.3.1.0` sudah ditentukan range IP dari `10.14.3.10 - 10.14.3.30` dan `10.14.3.60 - 10.14.3.85`

## Nomor 5

### Soal

Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut.

### Cara Pengerjaan

Cara untuk client dapat terhubung dengan internet, pada DNS Server (WISE) pada file `/etc/bind/named.conf.options` lakukan konfigurasi sebagai berikut
- Pada WISE
```
options {
        directory \"/var/cache/bind\";
        forwarders {
        192.168.122.1;
        };
        //dnssec-validation auto;
        allow-query{ any; };
        auth-nxdomain no;
        listen-on-v6 { any; };
    };
```
restart bind9
```
service bind9 restart
```

Lakukan restart pada DHPCP server dan DHCP relay

- Pada Westalis
```
service isc-dhcp-server restart
```
- Pada Ostania
```
service isc-dhcp-relay restart
```
- Pada Client
```
cat /etc/resolv.conf
```

## Nomor 6
### Soal
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit!
### Cara Pengerjaan
pada file `/etc/dhcp/dhcpd.conf` subnet interface switch 1 dan 3, tambahkan konfigurasi sebagai berikut
```bash
subnet 10.2.1.0 netmask 255.255.255.0 {
	...
    default-lease-time 300;
    max-lease-time 6900;
}
subnet 10.2.3.0 netmask 255.255.255.0 {
	...
    default-lease-time 600;
    max-lease-time 6900;
}
```

## Nomor 7
### Soal
Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan
alamat IP yang tetap dengan IP [prefix IP].3.13
### Cara Pengerjaan
Edit file `/etc/dhcp/dhcpd.conf` dan tambahkan konfigurasi sebagai berikut
```
host Eden {
    hardware ethernet 4e:bb:1d:6b:1c:17;
    fixed-address 10.14.3.13;
    }
```
Hardware ethernet tersebut didapatkan dari menjalankan command `ip a` pada Eden

![image](https://user-images.githubusercontent.com/89954689/201678705-25e445c8-b8fe-4421-aabe-67e8ad8aee3b.png)

Pada konfigurasi Eden tambahkan
```
hwaddress ether 4e:bb:1d:6b:1c:17
```

## Squid
## Nomor 1
### Soal
Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses
24 jam penuh)
### Cara Pengerjaan
Masukkan MTWHF 08:00-17:00 sebagai WORKHOUR dan semua akses akan diblock selama WORKHOUR
```
echo '
acl WORKHOUR time MTWHF 08:00-17:00
' > /etc/squid/acl.conf

echo '
include /etc/squid/acl.conf

http_port 8080
http_access deny all WORKHOUR
http_access allow all !WORKHOUR
visible_hostname Berlint
' > /etc/squid/squid.conf
```
### Kendala
- Belum bisa memblokir akses website saat workhour

![image](https://user-images.githubusercontent.com/89954689/201679793-d98aa080-758d-47fe-ac10-8e22e1e88034.png)


## Nomor 2
### Soal
Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP
tujuan domain dibebaskan)
### Cara Pengerjaan
```
echo '
loid-work.com
franky-work.com
' > /etc/squid/work-sites.acl

echo '
include /etc/squid/acl.conf
http_port 8080
visible_hostname Berlint

acl SITES dstdomain "/etc/squid/work-sites.acl"
http_access deny all WORKHOUR
http_access allow SITES WORKHOUR
http_access allow all !WORKHOUR
' > /etc/squid/squid.conf
```
### Kendala
- Belum bisa mengakses loid-work.com dan franky-work.com

![image](https://user-images.githubusercontent.com/89954689/201681776-1c6cd7ff-978f-4749-b9c8-aa3f30c0c16e.png)


## Nomor 3
### Soal
Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)
### Cara Pengerjaan
Mengganti `http_access allow all !WORKHOUR` dengan `http_access CONNECT deny !SSL_ports !WORKHOUR`

![image](https://user-images.githubusercontent.com/89954689/201684490-41e0f26b-1187-4fc3-9749-08d071e8bae6.png)


## Nomor 4
### Soal
Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit
per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan
speed maksimal yaitu 128 Kbps)
### Cara Pengerjaan
128 Kbps = 16000 bytes
```
echo '
delay_pools 1
delay_class 1 1
delay_access 1 allow all
delay_parameters 1 16000/16000
' > /etc/squid/acl-bandwidth.conf

echo '
include /etc/squid/acl.conf
include /etc/squid/acl-bandwidth.conf

http_port 8080
visible_hostname Berlint

acl sites dstdomain "/etc/squid/work-sites.acl"
http_access allow SITES WORKHOUR
http_access CONNECT deny !SSL_ports !WORKHOUR
' > /etc/squid/squid.conf
```
Ditest dengan membandingkan speedtest sebelum dan sesudah proxy aktif
- Non aktif

![image](https://user-images.githubusercontent.com/89954689/201686407-e97ac9f8-59d8-41b7-b1a9-6dd5ec2b241a.png)

- Aktif

![image](https://user-images.githubusercontent.com/89954689/201686455-57cd1c82-4fe2-45c6-a287-1bce82918fdf.png)

### Kendala
- speedtest tidak bisa dijalankan saat proxy aktif
