# Jarkom_Modul5_Lapres_T11
Repositori sebagai Laporan Resmi Soal Praktikum Modul 1 Praktikum Komunikasi Data dan Jaringan Komputer 2020\
Disusun oleh :
```
Kelompok    :   T11
Anggota     :   I Gde Made Bhaskara Jala Dhananjaya (05311840000007)
                Robby Irvine Surya                  (05311840000023)
Departemen  :   Teknologi Informasi
```
## Topologi yang Diberikan

![](https://github.com/Bhaskaraa/Jarkom_Modul5_Lapres_T11/blob/main/Modul%205/Screenshot%202020-12-29%20235456.png)

Dengan keterangan pada soal :
- SURABAYA diberikan IP TUNTAP
- MALANG merupakan DNS Server diberikan IP DMZ
- MOJOKERTO merupakan DHCP Server diberikan IP DMZ
- MADIUN dan PROBOLINGGO merupakan WEB Server
- Setiap Server diberikan memory sebesar 128M
- Client dan Router diberikan memori sebesar 96M
- Jumlah host pada subnet SIDOARJO 200 Host
- Jumlah host pada subnet GRESIK 210 Host

## Konfigurasi topologi.sh dan bye.sh
```
# Switch ada 8
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &
uml_switch -unix switch4 > /dev/null < /dev/null &
uml_switch -unix switch5 > /dev/null < /dev/null &
uml_switch -unix switch6 > /dev/null < /dev/null &

# Router
xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch4 eth1=daemon,,,switch2 eth2=daemon,,,switch3 mem=96M &
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.74.73 eth1=daemon,,,switch4 eth2=daemon,,,switch5 mem=96M &
xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch5 eth1=daemon,,,switch1 eth2=daemon,,,switch6 mem=96M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch1 mem=128M &
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch1 mem=128M &

# Klien
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch6 mem=96M &
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch3 mem=96M &
```
- bye.sh

![](https://github.com/Bhaskaraa/Jarkom_Modul5_Lapres_T11/blob/main/Modul%205/bye.png)

## Subnetting dengan Metode VLSM

![](https://github.com/Bhaskaraa/Jarkom_Modul5_Lapres_T11/blob/main/Modul%205/Subnetting.png)

## Pohon VLSM

![](https://github.com/Bhaskaraa/Jarkom_Modul5_Lapres_T11/blob/main/Modul%205/Pohon%20VLSM.png)

## SURABAYA sebagai Router
- Interfaces
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.76.69
netmask 255.255.255.252
gateway 10.151.76.68

auto eth1
iface eth1 inet static
address 192.168.2.1
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.2.5
netmask 255.255.255.252
```
## BATU sebagai Router
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.2.2
netmask 255.255.255.252
gateway 192.168.2.1

auto eth1
iface eth1 inet static
address 10.151.77.138
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.0.1
netmask 255.255.255.0
```
## KEDIRI sebagai Router
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.2.6
netmask 255.255.255.252
gateway 192.168.2.5

auto eth1
iface eth1 inet static
address 192.168.2.9
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.1.1
netmask 255.255.255.0
```
## MALANG sebagai DNS Server
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.139
netmask 255.255.255.248
gateway 10.151.77.138
```
## MOJOKERTO sebagai DHCP Server
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.140
netmask 255.255.255.248
gateway 10.151.77.138
```
## MADIUN sebagai Web Server
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.2.10
netmask 255.255.255.248
gateway 192.168.2.9
```
## PROBOLINGGO sebagai Web Server 
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.2.11
netmask 255.255.255.248
gateway 192.168.2.9
```
## GRESIK sebagai Client
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```
## SIDOARJO sebagai Client
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```
## Melakukan Routing
- Pada UML Surabaya, masukin konfigurasi sebagai berikut :
```
route add -net 192.168.0.0 netmask 255.255.255.0 gw 192.168.2.2 #Subnet SIDOARJO
route add -net 10.151.83.144 netmask 255.255.255.248 gw 192.168.2.2 #Subnet MALANG dan MOJOKERTO
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.2.6 #Subnet GRESIK
route add -net 192.168.2.8 netmask 255.255.255.248 gw 192.168.2.6 #Subnet MADIUN dan PROBOLINGGO
```
## Penginstalan DHCP Relay dan DHCP Server
### DHCP Relay
- Install DHCP relay pada router BATU dan KEDIRI dengan menggunakan perintah `apt-get install isc-dhcp-relay`
- Tuliskan IP MOJOKERTO sebagai destinasi DHCP server. Buka file konfigurasi interface pada router BATU dan KEDIRI dengan menggunakan perintah `nano /etc/default/isc-dhcp-relay`
- Pada router BATU, bagian INTERFACES isikan `eth1 eth2` >> `INTERFACES="eth1 eth2"`
- Pada router KEDIRI, bagian INTERFACES isikan `eth0 eth2` >> `INTERFACES="eth0 eth2"`
DHCP Server

### DHCP Server
- Install DHCP server pada server MOJOKERTO dengan menggunakan perintah `apt-get install isc-dhcp-server`
- Buka file konfigurasi interface pada MOJOKERTO dengan perintah `nano /etc/default/isc-dhcp-server`
- Pada bagian INTERFACESv4 isikan `eth0` >> `INTERFACESv4="eth0"`
- Kemudian, edit file konfigurasi `nano /etc/dhcp/dhcpd.conf` dengan konfigurasi sebagai berikut.
```
#NID DMZ
subnet 10.151.83.144 netmask 255.255.255.248 {}

#Subnet Sidoarjo
subnet 192.168.0.0 netmask 255.255.255.0 {
  range 192.168.0.5 192.168.0.215;
  option routers 192.168.0.1;
  option broadcast-address 192.168.0.255;
  option domain-name-servers 10.151.77.139;
  default-lease-time 300;
  max-lease-time 7200;
 }

#Subnet Gresik
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.5 192.168.1.215;
  option routers 192.168.1.1;
  option broadcast-address 192.168.1.255;
  option domain-name-servers 10.151.77.139;
  default-lease-time 30
  max-lease-time 7200;
}
```
- Kemudian, restart DHCP Server dengan command `service isc-dhcp-server restart`

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan MASQUERADE. Solusi yang dapat berikan adalah melalui syntax berikut. \
`iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o eth0 -j SNAT --to-source 10.151.74.74 #surabaya `
- Testing, dengan melakukan ping google.com di Surabaya

![](https://github.com/Bhaskaraa/Jarkom_Modul5_Lapres_T11/blob/main/Modul%205/nomor1.jpg)

## Soal 2
Kalian diminta untuk mendrop semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada SURABAYA demi menjaga keamanan. Solusi yang dapat diberikan adalah melalui syntax berikut. \
`iptables -A FORWARD -d 10.151.77.137/29 -i eth0 -p tcp --dport 22 -j DROP #surabaya`

## Soal 3
Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP. Solusi yang dapat diberikan adalah sebagai berikut. \
`iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP #malang dan mojokerto`

## Soal 4
Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat. Solusi yang dapat diberikan adalah melalui syntax sebagai berikut. \
`iptables -A INPUT -s 192.168.0.0/24 -m time --timestart 07:00 --timestop 17:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT` \
`iptables -A INPUT -s 192.168.0.0/24 -j REJECT #malang`

## Soal 5
Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. Solusi yang dapat diberikan adalah melalui syntax berikut. \
`iptables -A INPUT -s 192.168.1.0/24 -m time --timestart 06:59 --timestop 16:59 -j REJECT #malang`

## Soal 6
Karena kita memiliki 2 buah WEB Server, Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80. Solusi yang dapat diberikan adalah melalui syntax sebagai berikut.\
```
#surabaya
iptables -t nat -A PREROUTING -p tcp -d 10.151.83.146 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.168.2.11:80
iptables -t nat -A PREROUTING -p tcp -d 10.151.83.146 -j DNAT --to-destination 192.168.2.10:80
```

## Soal 7
Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop. Solusi yang dapat diberikan adalah sebagai berikut.
```
#surabaya
iptables -N LOGGING
iptables -A FORWARD -j LOGGING
iptables -A OUTPUT -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP

#malang
iptables -N LOGGING
iptables -A INPUT -j LOGGING
iptables -A OUTPUT -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP

#mojokerto
iptables -N LOGGING
iptables -A INPUT -j LOGGING
iptables -A OUTPUT -j LOGGING
iptables -A LOGGING -j LOG --log-prefix "IPTables-Dropped: " --log-level 4
iptables -A LOGGING -j DROP
```
