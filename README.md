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

![Topologi](/topologi.png)

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

> Yang lainnya tidak perlu karena sudah terhubung dengan router yang lain. Contoh : Sein sudah terhubung dengan Heiter, sehingga tidak perlu di routing lagi. Begitu juga dengan GrabForest dan TurkRegion.


# Soal

1. Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE.

  - Aura
```bash
ETH0_IP=$(ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')

iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $ETH0_IP
```

Karena kita menggunakan IP DHCP sebagai sebagai jalur keluar ke internet, maka kita harus mengambil IP dari eth0 aura sebagai jalur keluar. `ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'` akan mereturn IP dari eth0 Aura

Selanjutnya, untuk menyambungkan ke nat, maka kita masukkan ke NAT Table dengan POSTROUTING chain : `iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $ETH0_IP`


2. Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.

  - GrobeForest, Stark, and Sein
```bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

apt-get update
apt-get install netcat -y

# Menolak semua koneksi TCP kecuali port 8080
iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -j DROP

# Menolak semua koneksi UDP
iptables -A INPUT -p udp -j DROP
```



3. Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop.

  - Revolte & Richter (DHCP server & DNS Server)
```bash
# Allow established and related connections
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Limit ICMP connections to 3 per second
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

- `--connlimit-above 3` adalah parameter menentukan batas koneksi yang terhubung
- `--connlimit-mask 0` adalah parameter menentukan mask


4. Lakukan pembatasan sehingga koneksi SSH pada Web Server hanya dapat dilakukan oleh masyarakat yang berada pada GrobeForest.

  - Sein & Stark (Web Server)
```bash
iptables -A INPUT -p tcp --dport 22 -s 192.240.4.0/22 -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j DROP
```


5. Selain itu, akses menuju WebServer hanya diperbolehkan saat jam kerja yaitu Senin-Jumat pada pukul 08.00-16.00.
  - Sein & Stark (Web Server)
```bash
# Izinkan akses ke Web Server pada senin-jumat pukul 08:00-16:00
iptables -A INPUT -p tcp --dport 80 -m time --timestart 08:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
```

6. Lalu, karena ternyata terdapat beberapa waktu di mana network administrator dari WebServer tidak bisa stand by, sehingga perlu ditambahkan rule bahwa akses pada hari Senin - Kamis pada jam 12.00 - 13.00 dilarang (istirahat maksi cuy) dan akses di hari Jumat pada jam 11.00 - 13.00 juga dilarang (maklum, Jumatan rek).

  - Konfigurasi Sein & Stark (Web Server)
```bash
# Larangan Akses pada hari Senin-Kamis jam 12:00 - 13:00
iptables -A INPUT -p tcp --dport 80 -m time --timestart 12:00 --timestop 13:00 --weekdays Mon,Tue,Wed,Thu -j DROP

# Larangan akses pada hari jumat pada 11:00 - 13:00
iptables -A INPUT -p tcp --dport 80 -m time --timestart 11:00 --timestop 13:00 --weekdays Fri -j DROP
```


7. Karena terdapat 2 WebServer, kalian diminta agar setiap client yang mengakses Sein dengan Port 80 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan dan request dari client yang mengakses Stark dengan port 443 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan.
  - Sein
```bash
# Soal 7
iptables -A PREROUTING -t nat -p tcp -d 192.240.4.2 --dport 80 -m statistics --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.240.4.2:80

iptables -A PREROUTING -t nat -p tcp -d 192.240.4.2 --dport 80 -j DNAT --to-destination 192.240.0.14:80
```

  - Stark
```bash
# Soal 7
iptables -A PREROUTING -t nat -p tcp -d 192.240.0.4 --dport 443 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.240.4.2:443

iptables -A PREROUTING -t nat tcp -d 192.240.0.4 --dport 443 -j DNAT --to-destination 192.240.0.14:443
```

8. Karena berbeda koalisi politik, maka subnet dengan masyarakat yang berada pada Revolte dilarang keras mengakses WebServer hingga masa pencoblosan pemilu kepala suku 2024 berakhir. Masa pemilu (hingga pemungutan dan penghitungan suara selesai) kepala suku bersamaan dengan masa pemilu Presiden dan Wakil Presiden Indonesia 2024.

  - Sein
```bash
### apabila ingin meng-drop TCP 
iptables -A INPUT -p tcp -s 192.240.0.2 --dport 80 -m time --datestart 2023-10-19T00:00 --datestop 2024-02-15T00:00 -j DROP

### namun ingin drop semua, bisa digunakan :
iptables -A INPUT -s 192.240.0.2 -m time --datestart 2023-10-19T00:00 --datestop 2024-02-15T00:00 -j DROP

```

9. Sadar akan adanya potensial saling serang antar kubu politik, maka WebServer harus dapat secara otomatis memblokir  alamat IP yang melakukan scanning port dalam jumlah banyak (maksimal 20 scan port) di dalam selang waktu 10 menit. (clue: test dengan nmap)

  - Sein
```bash
# Soal No 9
iptables -A INPUT -p tcp --syn -m recent --name portscan --set
iptables -A INPUT -p tcp --syn -m recent --name portscan --rcheck --seconds 600 --hitcount  20  -j  DROP
```

  - Stark
```bash
# Soal No 9
iptables -N PORTSCAN
iptables -A PORTSCAN -m recent --set --name portscan
iptables -A PORTSCAN -m recent --update --seconds 600 --hitcount 20 --name portscan -j LOG --log-prefix "Portscan Detected: " --log-level 4
iptables -A PORTSCAN -m recent --update --seconds 600 --hitcount 20 --name portscan -j DROP
iptables -A INPUT -p tcp --tcp-flags SYN,ACK,FIN,RST RST -m limit --limit 2/s -j ACCEPT
iptables -A INPUT -p tcp --tcp-flags SYN,ACK,FIN,RST RST -j PORTSCAN
```

10. Karena kepala suku ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.

logging dapat ditambahkan dengan syntax iptables berikut yang dijalankan di semua node server dan router

```bash
iptables -A INPUT -j LOG --log-level debug --log-prefix "Dropped Packet: " -m limit --limit 1/second --limit-burst 10
```
