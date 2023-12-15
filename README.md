# Modul 4 Jarkom IT14

* Moh. Sulthan Arief Rahmatullah - 5027211045
* Fransiskus Benyamin - 5027211021

## Table of Contents

- [VLSM](#vlsm)

## Pembagian Subnet

![Pembagian Subnet](/pembagiansubnet.png)
Keterangan Soal:

- Richter adalah DNS Server
- Revolte adalah DHCP Server
- Sein dan Stark adalah Web Server
- Jumlah Host pada SchwerMountain adalah 64
- Jumlah Host pada LaubHills adalah 255
- Jumlah Host pada TurkRegion adalah 1022
- Jumlah Host pada GrobeForest adalah 512

## VLSM (Variable Length Subnet Masking)

Menentukan jumlah alamat IP yang dibutuhkan oleh tiap subnet dan melabel netmask berdasarkan jumlah IP yang dibutuhkan.

| **Subnet**  | **Jumlah IP** | **Netmask** |
| :---------: | :-----------: | :---------: |
|     A1      |       2       |     /30     |
|     A2      |     1023      |     /21     |
|     A3      |      514      |     /22     |
|     A4      |       2       |     /30     |
|     A5      |       2       |     /30     |
|     A6      |       2       |     /30     |
|     A7      |      256      |     /23     |
|     A8      |       66      |     /25     |
|     A9      |       2       |     /30     |
|     A10     |       2       |     /30     |
|  **Total**  |   **1871**    |   **/20**   |

## Pohon IP

Subnet besar yang dibentuk memiliki NID 192.240.0.0 dengan netmask /20 sehingga pembagian IP berdasarkan NID dan netmask dihitung sesuai dengan pohon berikut.
![Pembagian Subnet](/vlsmtree.png)

Pada VLSM ini diturunkan sesuai dengan length atasnya sehingga ketika /20 akan diturunkan menjadi /21, dan pembagian IPnya mengikuti tabel. Kemudian jika ada subnet yang bisa diassign, maka kita langsung mengassign. Hal ini dilakukan berulang" sampai semua subnet selesai di assign.

## Subnet + IP

Selanjutnya menyesuaikan pembagian IP sesuai dengan subnet yang telah dibagi berdasarkan pohon diatas pada topologi. Setelah itu akan didapatkan pembagian IP untuk tiap nodenya.

| Subnet | Node           | IP             | Netmask         | Length | NID            | Broadcast      |
| ------ | -------------- | -------------- | --------------- | ------ | -------------- | -------------- |
| A1     | Aura           | 192.240.14.129 | 255.255.255.252 | 30     | 192.240.14.128 | 192.240.14.131 |
|        | Heiter         | 192.240.14.130 | 255.255.255.252 | 30     |                |                |
|        |                |                |                 |        |                |                |
| A2     | Heiter         | 192.240.0.1    | 255.255.248.0   | 21     | 192.240.0.0    | 192.240.7.255  |
|        | TurkRegion     | DHCP           | 255.255.248.0   | 21     |                |                |
|        |                |                |                 |        |                |                |
| A3     | Heiter         | 192.240.8.1    | 255.255.252.0   | 22     | 192.240.8.0    | 192.240.11.255 |
|        | Sein           | 192.240.8.2    | 255.255.252.0   | 22     |                |                |
|        | GrabForest     | DHCP           | 255.255.252.0   | 22     |                |                |
|        |                |                |                 |        |                |                |
| A4     | Aura           | 192.240.14.137 | 255.255.255.252 | 30     | 192.240.14.136 | 192.240.14.139 |
|        | Frieren        | 192.240.14.138 | 255.255.255.252 | 30     |                |                |
|        |                |                |                 |        |                |                |
| A5     | Frieren        | 192.240.14.141 | 255.255.255.252 | 30     | 192.240.14.140 | 192.240.14.143 |
|        | Stark          | 192.240.14.142 | 255.255.255.252 | 30     |                |                |
|        |                |                |                 |        |                |                |
| A6     | Frieren        | 192.240.14.145 | 255.255.255.252 | 30     | 192.240.14.144 | 192.240.14.147 |
|        | Himmel         | 192.240.14.146 | 255.255.255.252 | 30     |                |                |
|        |                |                |                 |        |                |                |
| A7     | Himmel         | 192.240.12.1   | 255.255.254.0   | 23     | 192.240.12.0   | 192.240.13.255 |
|        | LaubHills      | DHCP           | 255.255.254.0   | 23     |                |                |
|        |                |                |                 |        |                |                |
| A8     | Himmel         | 192.240.14.1   | 255.255.255.128 | 25     | 192.240.14.0   | 192.240.14.127 |
|        | Fern           | 192.240.14.2   | 255.255.255.128 | 25     |                |                |
|        | SchwerMountain | DHCP           | 255.255.255.128 | 25     |                |                |
|        |                |                |                 |        |                |                |
| A9     | Fern           | 192.240.14.149 | 255.255.255.252 | 30     | 192.240.14.148 | 192.240.14.151 |
|        | Richter        | 192.240.14.150 | 255.255.255.252 | 30     |                |                |
|        |                |                |                 |        |                |                |
| A10    | Fern           | 192.240.14.133 | 255.255.255.252 | 30     | 192.240.14.132 | 192.240.14.135 |
|        | Revolte        | 192.240.14.134 | 255.255.255.252 | 30     |                |                |
|        |                |                |                 |        |                |                |

## Topologi Pada GNS

![Topologi](/assets/gns.png)

## Konfigurasi Router + IP

### Konfigurasi ETH

- Revolte

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 192.240.14.134
	netmask 255.255.255.252
	gateway 192.240.14.133
	up echo nameserver 192.240.14.150 > /etc/resolv.conf
```

- Richter

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 192.240.14.150
	netmask 255.255.255.252
	gateway 192.240.14.149
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Fern

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 192.240.14.2
	netmask 255.255.255.128
	gateway 192.240.14.1
	up echo nameserver 192.240.14.150 > /etc/resolv.conf

# Static config for eth1
auto eth1
iface eth1 inet static
	address 192.240.14.149
	netmask 255.255.255.252

# Static config for eth2
auto eth2
iface eth2 inet static
	address 192.240.14.133
	netmask 255.255.255.252
```

- SchewerMountain1

```bash
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
```

- LaubHills

```bash
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
```

- Himmel

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
         address 192.240.14.146
         netmask 255.255.255.252
         gateway 192.240.14.145
	 up echo nameserver 192.240.14.150 > /etc/resolv.conf

auto eth1
iface eth1 inet static
         address 192.240.14.1
         netmask 255.255.255.128

auto eth2
iface eth2 inet static
         address 192.240.12.1
         netmask 255.255.254.0
```

- Frieren

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
         address 192.240.14.138
         netmask 255.255.255.252
         gateway 192.240.14.137
	 up echo nameserver 192.240.14.150 > /etc/resolv.conf

auto eth1
iface eth1 inet static
         address 192.240.14.141
         netmask 255.255.255.252

auto eth2
iface eth2 inet static
         address 192.240.14.145
         netmask 255.255.255.252
```

- Stark

```
# Static config for eth0
auto eth0
iface eth0 inet static
         address 192.240.14.142
         netmask 255.255.255.252
         gateway 192.240.14.141
	 up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Aura

```bash
auto eth0
iface eth0 inet static
    address 192.168.122.14
    netmask 255.255.255.0
    gateway 192.168.122.1

auto eth1
iface eth1 inet static
      address 192.240.14.129
      netmask 255.255.255.252

auto eth2
iface eth2 inet static
       address 192.240.14.137
       netmask 255.255.255.252


```

- Heiter

```bash
auto eth0
iface eth0 inet static
         address 192.240.14.130
         netmask 255.255.255.252
         gateway 192.240.14.129
	 up echo nameserver 192.168.122.1 > /etc/resolv.conf

auto eth1
iface eth1 inet static
         address 192.240.8.1
         netmask 255.255.252.0

auto eth2
iface eth2 inet static
         address 192.240.0.1
         netmask 255.255.248.0


```

- TurkRegion

```bash
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
```

- Sein

```bash
# Static config for eth0
auto eth0
iface eth0 inet static
	address 192.240.8.2
	netmask 255.255.252.0
	gateway 192.240.8.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- GrabForest

```bash
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp
```

## Routing

- Himmel

```
route add -net 192.240.14.132 netmask 255.255.255.252 gw 192.240.14.2
route add -net 192.240.14.148 netmask 255.255.255.252 gw 192.240.14.2
```

- Frieren

```
route add -net 192.240.14.132 netmask 255.255.255.252 gw 192.240.14.146
route add -net 192.240.14.148 netmask 255.255.255.252 gw 192.240.14.146
route add -net 192.240.14.0 netmask 255.255.255.128 gw 192.240.14.146
route add -net 192.240.12.0 netmask 255.255.254.0 gw 192.240.14.146
```

- Aura

```
route add -net 192.240.14.132 netmask 255.255.255.252 gw 192.240.14.138
route add -net 192.240.14.148 netmask 255.255.255.252 gw 192.240.14.138
route add -net 192.240.14.0 netmask 255.255.255.128 gw 192.240.14.138
route add -net 192.240.12.0 netmask 255.255.254.0 gw 192.240.14.138
route add -net 192.240.14.144 netmask 255.255.255.252 gw 192.240.14.138
route add -net 192.240.14.140 netmask 255.255.255.252 gw 192.240.14.138

route add -net 192.240.0.0 netmask 255.255.248.0 gw 192.240.14.130
route add -net 192.240.8.0 netmask 255.255.252.0 gw 192.240.14.130
```

> Yang lainya tidak perlu karena sudah terhubung dengan router yang lain. Contoh : Sein sudah terhubung dengan Heiter, sehingga tidak perlu di routing lagi. Begitu juga dengan GrabForest dan TurkRegion.
