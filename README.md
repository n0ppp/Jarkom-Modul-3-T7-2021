# Jarkom-Modul-3-T7-2021      
### Laporan Resmi Pengerjaan Sesi Lab Jaringan Komputer     
        
#### Nama Anggota Kelompok :      
1. Naufal Aprilian (05311940000007)     
2. Bryan Yehuda Mannuel (05311940000021)      
3. Mulki Kusumah    

## Jawaban Modul 
  
### SOAL 4        
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50  

### Jawaban Soal 4    
Lakukan konfigurasi untuk rentang IP yang akan diberikan pada file  `/etc/dhcp/dhcpd.conf` dengan cara menambahkan konfigurasi berikut ini 
```
subnet 10.45.3.0 netmask 255.255.255.0 {
    range  10.45.3.30 10.45.3.50;
    option routers 10.45.3.1;
    option broadcast-address 10.45.3.255;
    option domain-name-servers 10.45.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}
```
Dengan begitu kita telah menentukan ip range  dengan menambahkan `range  10.45.3.30 10.45.3.50;`pada subnet interface switch 3 yang terhubung ke fosha pada eth3


### SOAL 5
Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

### Jawaban Soal 5
Untuk client mendapatkan DNS dari EniesLobby diperlukan konfigurasi pada file `/etc/dhcp/dhcpd.conf` dengan `option domain-name-servers 10.45.2.2;`

Supaya semua client dapat terhubung internet pada EniesLobby diberikan konfigurasi pada `file /etc/bind/named.conf.options` dengan
```
options {
        directory \"/var/cache/bind\";

        forwarders {
                8.8.8.8;
                8.8.8.4;
        };

        // dnssec-validation auto;
        allow-query { any; };
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```
#### TESTING
Dengan mengkonfigurasi DHCP server dan DHCP Relay seleuruh Client yang berada pada subnet interface switch 1 dan switch 3 akan otomatis mendapatkan IP pada rentang yang telah dikonfigurasi. Untuk contohnya adalah sebagai berikut:

**Loguetown**     
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/loguetown-5.png?raw=true)       

**Alabasta**     
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/alabasta-5.png?raw=true)     

**TottoLand**
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/Totoland-5.png?raw=true)

**Skypie**     
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/skypie-5.png?raw=true)

Semua Client juga akan bisa connect ke internet

**Loguetown**
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/loguetown-test.png?raw=true)

**Alabasta**
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/alabasta-test.png?raw=true)

**TottoLand**
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/Totoland-test.png?raw=true)

**Skypie**
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/skypie-test.png?raw=true)

### SOAL 6
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

#### Jawaban Soal 6
Pada subnet interface switch 1 dan 3 ditambahkan konfigurasi berikut pada file `/etc/dhcp/dhcpd.conf`
```
subnet 10.45.1.0 netmask 255.255.255.0 {
    ...
    default-lease-time 360; 
    max-lease-time 7200;
    ...
}
subnet 10.45.3.0 netmask 255.255.255.0 {
    ...
    default-lease-time 720;
    max-lease-time 7200;
    ...
}
```

### SOAL 7
Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan `IP [prefix IP].3.69`

#### Jawaban Soal 7
Menambahkan konfigurasi untuk fixed address pada `/etc/dhcp/dhcpd.conf`
```
host Skypie {
    hardware ethernet be:c0:ff:37:bb:09;
    fixed-address 10.45.3.69;
}
```
Setelah itu tidak lupa untuk mengganti konfigurasi pada file `/etc/network/interfaces` dengan 
```
auto eth0
iface eth0 inet dhcp
hwaddress ether be:c0:ff:37:bb:09
```
Maka Skypie akan mendapatkan IP `10.45.3.69`
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/testing-fix-ip-skypie.png?raw=true)

### SOAL 8
Loguetown digunakan sebagai client Proxy agar transaksi jual beli dapat terjamin keamanannya, juga untuk mencegah kebocoran data transaksi.

Pada Loguetown, proxy harus bisa diakses dengan nama `jualbelikapal.yyy.com` dengan port yang digunakan adalah `5000`
#### Jawaban Soal 8
Melakukan Export env http_proxy dengan`export http_proxy="http://jualbelikapal.t07.com:5000" `
#### TESTING

![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/testing-nomor-8-1.png?raw=true)

Ketika dikases akan tetap bisa menggunakan proxy
![](https://github.com/n0ppp/Jarkom-Modul-3-T7-2021/blob/main/image/testing-nomor-8-2.png?raw=true)