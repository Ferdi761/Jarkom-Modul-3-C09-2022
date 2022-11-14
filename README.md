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
