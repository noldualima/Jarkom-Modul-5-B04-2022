# Jarkom-Modul-5-B04-2022

## Praktikum Jaringan Komputer Modul 5 - Firewall
NRP | Nama
-----------|---------------------------
5025201025 | Dawamul Fikri Aqil
5025201048 | Afril Muzaqqi Arif
5025201165 | Gabriel Solomon Sitanggang

## PEMBAHASAN SOAL
Setelah kalian mempelajari semua modul yang telah diberikan, Loid ingin meminta bantuan untuk terakhir kalinya kepada kalian. Dan kalian dengan senang hati mau membantu Loid.

### Soal (A)
Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Loid,

**Keterangan :** Eden adalah DNS Server; WISE adalah DHCP Server; Garden dan SSS adalah Web Server; Jumlah Host pada Forger adalah 62 host; Jumlah Host pada Desmond adalah 700 host; Jumlah Host pada Blackbell adalah 255 host; Jumlah Host pada Briar adalah 200 host

**Hasil Topologi yang dibuat :**
![Gambar 2](/src/topologi_hasil.png)


### Soal (B)
Untuk menjaga perdamaian dunia, Loid ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM setelah melakukan subnetting.

**Jawaban :**
- **Tahap 1** Penentuan Subnet
  ![Gambar 3](/src/subneting.png)
  pada gambar di atas dapat terlihat terdapat 8 subnet di dalam topologi. Dengan menggunakan teknik classful setiap subnet akan memiliki netmask **/21** karena semua subnet memiliki jumlah host di bawah **2048**. Sehingga pembagian IP yang memungkinkan untuk topologi di atas adalah sebagai berikut.
- **Tahap 2** Pembagian Topologi

| Subnet Name | NeededSize | AlocatedSize | Address | Mask | Dec Mask | Assignable Range (Start) | Assignable Range (End) | Broadcast |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A2 | 701 | 1022 | 10.5.0.0 | /22 | 255.255.252.0 | 10.5.0.1 | 10.5.3.254 | 10.5.3.255 |
| A6 | 256 | 510 | 10.5.4.0 | /23 | 255.255.254.0 | 10.5.4.1 | 10.5.5.254 | 10.5.5.255 |
| A7 | 201 | 254 | 10.5.6.0 | /24 | 255.255.255.0 | 10.5.6.1 | 10.5.6.254 | 10.5.7.127 |
| A3 | 63 | 126 | 10.5.7.0 | /25 | 255.255.255.128 | 10.5.7.1 | 10.5.7.126 | 10.5.7.127 |
| A1 | 3 | 6 | 10.5.7.128 | /29 | 255.255.255.248 | 10.5.7.129 | 10.5.7.134 | 10.5.7.135 |
| A8 | 3 | 6 | 10.5.7.136 | /29 | 255.255.255.248 | 10.5.7.137 | 10.5.7.142 | 10.5.7.143 |
| A4 | 2 | 2 | 10.5.7.144 | /30 | 255.255.255.252 | 10.5.7.145 | 10.5.7.146 | 10.5.7.147 |
| A5 | 2 | 2 | 10.5.7.148 | /30 | 255.255.255.252 | 10.5.7.149 | 10.5.7.150 | 10.5.7.151 |

- **Tahap 3** Subneting
![Gambar 4](/src/diagram_vlsm.png)


### Soal (C)
Anya, putri pertama Loid, juga berpesan kepada anda agar melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

**Jawaban :**
Proses konfigurasi dilakukan pada setiap node dengan konfigurasi sebagai berikut :
- Strix 
    ```
    auto lo
    iface lo inet loopback

    auto eth0
    iface eth0 inet dhcp
    hwaddress ether b6:23:6b:b5:5d:33

    auto eth1
    iface eth1 inet static
    address 10.5.7.149
    netmask 255.255.255.252

    auto eth2
    iface eth2 inet static
    address 10.5.7.145
    netmask 255.255.255.252
    ```
- Westalis
    ```
    auto lo
    iface lo inet loopback

    auto eth0
    iface eth0 inet static
    address 10.5.7.146
    netmask 255.255.255.252
    gateway 10.5.7.145

    auto eth1
    iface eth1 inet static
    address 10.5.0.1
    netmask 255.255.252.0

    auto eth2
    iface eth2 inet static
    address 10.5.7.1
    netmask 255.255.255.128

    auto eth3
    iface eth3 inet static
    address 10.5.7.129
    netmask 255.255.255.248
    ```
- Ostania
    ```
    auto lo
    iface lo inet loopback

    auto eth0
    iface eth0 inet static
    address 10.5.7.150
    netmask 255.255.255.252
    gateway 10.5.7.149

    auto eth1
    iface eth1 inet static
    address 10.5.4.1
    netmask 255.255.254.0

    auto eth2
    iface eth2 inet static
    address 10.5.7.137
    netmask 255.255.255.248

    auto eth3
    iface eth3 inet static
    address 10.5.6.1
    netmask 255.255.255.0
    ```
- **Client** : Forer, Desmond, Blackbell, Briar
    ```
    auto lo
    iface lo inet loopback

    auto eth0
    iface eth0 inet dhcp
    ```
- Eden : 
    ``` 
    auto eth0
    iface eth0 inet static
    address 10.5.7.130
    netmask 255.255.255.248
    gateway 10.5.7.129
    ```
- Wise :
    ```
    auto eth0
    iface eth0 inet static
    address 10.5.7.131
    netmask 255.255.255.248
    gateway 10.5.7.129
    ```
- Garden :
    ```
    auto eth0
    iface eth0 inet static
    address 10.5.7.138
    netmask 255.255.255.248
    gateway 10.5.7.137
    ```
- SSS
    ```
    auto eth0
    iface eth0 inet static
    address 10.5.7.139
    netmask 255.255.255.248
    gateway 10.5.7.137
    ```
    

### Soal (D)
Tugas berikutnya adalah memberikan ip pada subnet Forger, Desmond, Blackbell, dan Briar secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.

**Jawaban :**
- Tahap 1 Proses routing pada device router 
    ```sh
    // Strix
    route add -net 10.5.7.128 netmask 255.255.255.248 gw 10.5.7.146 
    route add -net 10.5.7.0 netmask 255.255.255.128 gw 10.5.7.146 
    route add -net 10.5.0.0 netmask 255.255.252.0 gw 10.5.7.146

    route add -net 10.5.4.0 netmask 255.255.254.0 gw 10.5.7.150 
    route add -net 10.5.6.0 netmask 255.255.255.0 gw 10.5.7.150 
    route add -net 10.5.7.136 netmask 255.255.255.248 gw 10.5.7.150
    ```

    ```sh
    // Pada eden untuk memberi akses pada Garden dan SSS

- Melakukan setup pada DNS Server **( Eden )**
    - Melakukan instalasi pada bind9 -y dengan syntax 
        ```sh
        apt update
        apt install bind9 -y
        ```
    - melakukan konfigurasi pada file `/etc/bind/named.conf.options`
        ```sh
        echo '
        options {
                directory "/var/cache/bind";
                forwarders {
                        192.168.122.1;
                };
                allow-query { any; };
                auth-nxdomain no;    # conform to RFC1035
                listen-on-v6 { any; };
        };` > /etc/bind/named.conf.options
        ```
    - melakukan restart dengan 
        ```
        service bind9 restart
        ```
- Melakukan setup pada DHCP Server **( Eden )**
  - Melakukan instalasi dhcp server dengan syntax 
    ```sh
    apt update
    apt install isc-dhcp-server -y  
    ```
  - melakukan konfigurasi pada file `/etc/default/isc-dhcp-server`, dengan nilai sebagai berikut :
    ```sh
    echo '
    INTERFACES="eth0"' > /etc/default/isc-dhcp-server
    ```
  - melakukan konfigurasi pada file ` /etc/dhcp/dhcpd.conf`, dengan nilai sebagai berikut :
    ```
    #Forger
    subnet 10.5.7.0 netmask 255.255.255.128 {
        range 10.5.7.2 10.5.7.126;
        option routers 10.5.7.1;
        option broadcast-address 10.5.7.127; option domain-name-servers 10.5.7.130;
        default-lease-time 600;
        max-lease-time 7200;
    }
    #Desmond
    subnet 10.5.0.0 netmask 255.255.252.0 { 
        range 10.5.0.2 10.5.3.254;
        option routers 10.5.0.1;
        option broadcast-address 10.5.3.255; option domain-name-servers 10.5.7.130;
        default-lease-time 600; max-lease-time 7200;
    }

    #Blackbell
    subnet 10.5.4.0 netmask 255.255.254.0 { 
        range 10.5.4.2 10.5.5.254;
        option routers 10.5.4.1;
        option broadcast-address 10.5.5.255;
        option domain-name-servers 10.5.7.130;
        default-lease-time 600;
        max-lease-time 7200;
    }

    #Briar
    subnet 10.5.6.0 netmask 255.255.255.0 { 
        range 10.5.6.2 10.5.6.254;
        option routers 10.5.6.1;
        option broadcast-address 10.5.6.255; 
        option domain-name-servers 10.5.7.130;
        default-lease-time 600; max-lease-time 7200;
    }

    subnet 10.5.7.128 netmask 255.255.255.248 {
        option routers 10.5.7.129;
    }

    ```
  - Melakukan restart server dengan syntax :
    ```
    service isc-dhcp-server restart
    ```
- Melakukan setup pada DHCP RELAY **( Westalist dan ostania )**
  - melakukan instalasi dhcp-relay dengan syntax :
    ```
    apt update
    apt install isc-dhcp-relay -y
    ```
  - melakukan konfigurasi pada `/etc/default/isc-dhcp-relay`, dengan syntax :
    ```sh
    echo ` 
    SERVERS="10.5.7.131"
    INTERFACES="eth0 eth1 eth2 eth3"
    OPTIONS=""
    ` > /etc/default/isc-dhcp-relay
    ```
  - Melakukan restart dhcp-relay dengan syntax
    ```
    service isc-dhcp-relay restart
    ```
- Melakukan setup pada Web Server **( Garden dan SSS )**, dengan instalasi Apache2 dengan syntax :
    ```sh
    apt update
    apt install apache2 -y

    mkdir var cd var
    mkdir www
    cd www mkdir html
    cd html
    touch index.html
    cd /var/www/html/
    ```


### Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Strix menggunakan iptables, tetapi Loid tidak ingin menggunakan MASQUERADE.

**Jawaban :**

Melakukan konfigurasi pada **Strix**, dengan syntax :
```
iptables -t nat -A POSTROUTING -s 10.5.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.213
```

**Testing**
![tesing1](/src/testing1.png)


### Soal 2
Kalian diminta untuk melakukan drop semua TCP dan UDP dari luar Topologi kalian pada server yang merupakan DHCP Server demi menjaga keamanan.

**Jawaban :**

Melakuakn konfiguransi pada **WISE**, dengan syntax :
```sh
iptables -A FORWARD -d 10.5.7.128/29 -i eth0 -p tcp --dport 80 -j DROP
iptables -A FORWARD -d 10.5.7.128/29 -i eth0 -p udp --dport 80 -j DROP
```

**Testing**
![tesing2](/src/testing2.png)


### Soal 3
Loid meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 2 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

**Jawaban :**

Pada DHCP Server **(WISE)** dilakukan konvigurasi dengan syntax berikut :
``` sh
iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -$
```
konfigurasi diatas akan melakukan reject bila terdapat ping lebih dari 3

**Testing**
- DHCP SERVER (WISE)
    ![3.1](/src/3.1.png)
    ![3.2](/src/3.2.png)
    ![3.3](/src/3.3.png)
- DNS SERVER (EDEN)
    ![3.4](/src/3.4.png)
    ![3.5](/src/3.5.png)
    ![3.6](/src/3.6.png)


### Soal 4
Akses menuju Web Server hanya diperbolehkan disaat jam kerja yaitu Senin sampai Jumat pada pukul 07.00 - 16.00.

**Jawaban :**

Melakukan pembatasan pada **Web Server**, yakni Garden dan SSS dengan menggunakan syntax berikut pada 
- DNS Server (Eden):
```sh
iptables -A INPUT -s 10.5.7.136/29 -m time --timestart 07:00 --timestop 16:0$ 
iptables -A INPUT -s 10.5.7.136/29 -j REJECT
```

- Node Garden dan SSS (Web Server)
```
iptables -A INPUT -m time --timestart 07:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -j REJECT
```

**Testing**
![4.1](/src/4.1.png)


### Soal 5
Karena kita memiliki 2 Web Server, Loid ingin Ostania diatur sehingga setiap request dari client yang mengakses Garden dengan port 80 akan didistribusikan secara bergantian pada SSS dan Garden secara berurutan dan request dari client yang mengakses SSS dengan port 443 akan didistribusikan secara bergantian pada Garden dan SSS secara berurutan.

**Jawaban :**

Melakukan konfigurasi berikut pada DNS Server **(Eden)** : 
```
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.5.7.138 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.5.7.138:80
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.5.7.138 -j DNAT --to-destination 10.5.7.139:80
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.5.7.139 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.5.7.139:443
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.5.7.139 -j DNAT --to-destination 10.5.7.138:443
```

**Setting**
- /var/www/html/index.html
```
# Garden
Garden

# SSS
SSS
```

- /etc/apache2/ports.conf
```
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80
Listen 443

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

**Testing**

![5.1](/src/5.1.png)


### Soal 6
Karena Loid ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.

**Jawaban :**
- Melakukan konfigurasi berikut pada DNS Server **(Eden)** dan DHCP SERVER **(Wise)**:
    ```sh
    iptables -N LOGGING
    iptables -A INPUT -p icmp -m connlimit --connlimit-above 2 --connlimit-mask 0 -$
    iptables -A LOGGING - LOG --log-prefix "IPTables-Dropped: "
    iptables -A LOGGING -j DROP

    ```

- Melakuakn restart `rsyslog` pada **(Eden)** dan DHCP SERVER **(Wise)**, menggunakan syntax :
    ```
    service rsyslog restart
    ```

**Testing**

 ![6.1](/src/6.1.png)

### Kendala
-
