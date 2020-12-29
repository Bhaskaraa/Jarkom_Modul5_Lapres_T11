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

![]()

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

## Subnetting dengan Metode VLSM

![]()

## Pohon VLSM

![]()

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
