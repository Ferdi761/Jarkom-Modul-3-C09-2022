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
