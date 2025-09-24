# VLAN-Configuration-UTS-JKT1

Repository ini berisi tugas UTS mata kuliah Jaringan Komputer Terapan 1 yang mencakup implementasi VLAN dan troubleshooting jaringan.

## Deskripsi

Tugas UTS ini terdiri dari dua bagian utama:
1. Pertanyaan teori tentang troubleshooting jaringan dan implementasi VLAN
2. Implementasi praktikum VLAN dengan 5 switch dan 1 router

## Topologi Jaringan

```
        +--------+
        | Router |
        +--------+
            |
            |
        +--------+
        |  SW-1  |
        +--------+
       /    |    \
      /     |     \
 +--------+ +--------+ +--------+
 |  SW-2  | |  SW-3  | |  SW-4  |
 +--------+ +--------+ +--------+
      \     |     /
       \    |    /
        +--------+
        |  SW-5  |
        +--------+
```
<img width="827" height="401" alt="image" src="https://github.com/user-attachments/assets/2653f2b4-c575-42e6-aeb3-27103f0704fe" />

## Pembagian VLAN

| VLAN ID | Nama VLAN    | Switch yang Mengandung VLAN |
|---------|--------------|-----------------------------|
| 10      | Marketing    | SW-1, SW-2, SW-3, SW-5     |
| 20      | Sales        | SW-1, SW-2, SW-3, SW-5     |
| 30      | HRD          | SW-1, SW-2, SW-4           |
| 40      | Finance      | SW-1, SW-3, SW-4           |
| 50      | Research     | SW-1, SW-4, SW-5           |
| 60      | Development  | SW-1, SW-4, SW-5           |

## Konfigurasi Router

```
R-UTS#show running-config

Building configuration...

Current configuration : 2345 bytes
!
version 15.1
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname R-UTS
!
ip dhcp excluded-address 192.168.20.1
ip dhcp excluded-address 192.168.30.1
!
ip dhcp pool VLAN20
   network 192.168.20.0 255.255.255.0
   default-router 192.168.20.1
   dns-server 8.8.8.8
!
ip dhcp pool VLAN30
   network 192.168.30.0 255.255.255.0
   default-router 192.168.30.1
   dns-server 8.8.8.8
!
interface GigabitEthernet0/0
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
!
interface GigabitEthernet0/0.40
 encapsulation dot1Q 40
 ip address 192.168.40.1 255.255.255.0
!
interface GigabitEthernet0/0.50
 encapsulation dot1Q 50
 ip address 192.168.50.1 255.255.255.0
!
interface GigabitEthernet0/0.60
 encapsulation dot1Q 60
 ip address 192.168.60.1 255.255.255.0
!
ip classless
!
line con 0
!
line aux 0
!
line vty 0 4
 login
!
end
```

## Konfigurasi Switch

### SW-1

```
SW-1#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2
10   Marketing                        active    Fa0/3, Fa0/4, Fa0/5, Fa0/6, Fa0/7
20   Sales                            active    Fa0/8, Fa0/9, Fa0/10, Fa0/11, Fa0/12, Fa0/13
30   HRD                              active    Fa0/14, Fa0/15, Fa0/16, Fa0/17, Fa0/18
40   Finance                          active    Fa0/19, Fa0/20, Fa0/21, Fa0/22, Fa0/23, Fa0/24
50   Research                         active    
60   Development                      active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

### SW-2

```
SW-2#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
10   Marketing                        active    Fa0/3, Fa0/4, Fa0/5, Fa0/6, Fa0/7
20   Sales                            active    Fa0/8, Fa0/9, Fa0/10, Fa0/11, Fa0/12, Fa0/13
30   HRD                              active    Fa0/14, Fa0/15, Fa0/16, Fa0/17, Fa0/18
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

### SW-3

```
SW-3#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
10   Marketing                        active    Fa0/3, Fa0/4, Fa0/5, Fa0/6, Fa0/7
20   Sales                            active    Fa0/8, Fa0/9, Fa0/10, Fa0/11, Fa0/12, Fa0/13
40   Finance                          active    Fa0/14, Fa0/15, Fa0/16, Fa0/17, Fa0/18
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

### SW-4

```
SW-4#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2
30   HRD                              active    Fa0/3, Fa0/4, Fa0/5, Fa0/6, Fa0/7
40   Finance                          active    Fa0/8, Fa0/9, Fa0/10, Fa0/11, Fa0/12, Fa0/13
50   Research                         active    Fa0/14, Fa0/15, Fa0/16, Fa0/17, Fa0/18
60   Development                      active    Fa0/19, Fa0/20, Fa0/21, Fa0/22, Fa0/23, Fa0/24
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

### SW-5

```
SW-5#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2
10   Marketing                        active    Fa0/3, Fa0/4, Fa0/5, Fa0/6, Fa0/7
20   Sales                            active    Fa0/8, Fa0/9, Fa0/10, Fa0/11, Fa0/12, Fa0/13
50   Research                         active    Fa0/14, Fa0/15, Fa0/16, Fa0/17, Fa0/18
60   Development                      active    Fa0/19, Fa0/20, Fa0/21, Fa0/22, Fa0/23, Fa0/24
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```

## Hasil Testing

### Testing Antar VLAN yang Sama

| Dari VLAN | Ke VLAN | Dari Switch | Ke Switch | Hasil |
|-----------|---------|-------------|-----------|-------|
| 10        | 10      | SW-1        | SW-2      | Success |
| 10        | 10      | SW-2        | SW-3      | Success |
| 10        | 10      | SW-3        | SW-5      | Success |
| 20        | 20      | SW-1        | SW-2      | Success |
| 20        | 20      | SW-2        | SW-3      | Success |
| 20        | 20      | SW-3        | SW-5      | Success |
| 30        | 30      | SW-1        | SW-2      | Success |
| 30        | 30      | SW-2        | SW-4      | Success |
| 40        | 40      | SW-1        | SW-3      | Success |
| 40        | 40      | SW-3        | SW-4      | Success |
| 50        | 50      | SW-1        | SW-4      | Success |
| 50        | 50      | SW-4        | SW-5      | Success |
| 60        | 60      | SW-1        | SW-4      | Success |
| 60        | 60      | SW-4        | SW-5      | Success |

### Testing Antar VLAN yang Berbeda

| Dari VLAN | Ke VLAN | Dari Switch | Ke Switch | Hasil |
|-----------|---------|-------------|-----------|-------|
| 10        | 20      | SW-1        | SW-1      | Failed |
| 10        | 30      | SW-1        | SW-1      | Failed |
| 10        | 40      | SW-1        | SW-1      | Failed |
| 10        | 50      | SW-1        | SW-1      | Failed |
| 10        | 60      | SW-1        | SW-1      | Failed |
| 20        | 30      | SW-2        | SW-2      | Failed |
| 30        | 40      | SW-4        | SW-4      | Failed |
| 50        | 60      | SW-5        | SW-5      | Failed |

### Testing DHCP

| VLAN     | Device       | Hasil IP          | Gateway          | DNS Server       | Status |
|----------|--------------|-------------------|------------------|------------------|--------|
| VLAN 20  | PC Sales     | 192.168.20.2     | 192.168.20.1     | 8.8.8.8          | Success|
| VLAN 20  | PC Sales     | 192.168.20.3     | 192.168.20.1     | 8.8.8.8          | Success|
| VLAN 30  | PC HRD       | 192.168.30.2     | 192.168.30.1     | 8.8.8.8          | Success|
| VLAN 30  | PC HRD       | 192.168.30.3     | 192.168.30.1     | 8.8.8.8          | Success|

## Jawaban Pertanyaan Teori

### CPMK031

#### 1. Penanganan Loop pada Jaringan

**Langkah-langkah strategis untuk mengidentifikasi dan mengatasi loop:**

a. **Identifikasi Fisik**: Periksa koneksi fisik antar switch untuk memastikan tidak ada koneksi yang tidak disengaja yang dapat menyebabkan loop.

b. **Aktifkan STP**: Implementasikan Spanning Tree Protocol (STP) pada switch untuk secara otomatis mendeteksi dan memblokir jalur redundan yang dapat menyebabkan loop.

c. **Gunakan RSTP**: Jika memungkinkan, gunakan Rapid Spanning Tree Protocol (RSTP) yang memberikan konvergensi lebih cepat daripada STP standar.

d. **Konfigurasi BPDU Guard**: Aktifkan BPDU Guard pada port yang seharusnya tidak terhubung ke switch lain untuk mencegah loop yang disebabkan oleh perangkat yang tidak diinginkan.

e. **Monitor Jaringan**: Gunakan tools monitoring untuk mendeteksi traffic yang tidak normal yang mungkin mengindikasikan loop.

**Dampak loop terhadap model OSI:**

- **Layer 1 (Fisik)**: Loop dapat menyebabkan overload pada media transmisi karena frame yang sama ditransmisikan terus-menerus.
- **Layer 2 (Data Link)**: Loop menyebabkan broadcast storm di mana frame broadcast diproses secara berulang, menghabiskan bandwidth dan sumber daya CPU switch.
- **Layer 3 (Network)**: Performa jaringan menurun drastis karena router tidak dapat berfungsi efektif dalam lingkungan yang penuh dengan broadcast.
- **Layer 4 (Transport)**: Koneksi TCP dapat terganggu karena timeout dan paket yang hilang akibat congestion.
- **Layer 5-7 (Session, Presentation, Application)**: Aplikasi menjadi lambat atau tidak responsif karena jaringan yang tidak stabil.

#### 2. Troubleshooting Koneksi Internet

**Pendekatan troubleshooting berjenjang berdasarkan model OSI:**

a. **Layer 1 (Fisik)**:
   - Periksa kabel dan konektor untuk memastikan tidak ada kerusakan fisik.
   - Verifikasi bahwa semua port pada switch dan router dalam keadaan aktif.
   - Pastikan lampu indikator pada perangkat jaringan menyala normal.

b. **Layer 2 (Data Link)**:
   - Periksa konfigurasi VLAN dan trunking untuk memastikan perangkat berada di VLAN yang benar.
   - Verifikasi bahwa MAC address learning berfungsi dengan baik.
   - Gunakan perintah `show mac address-table` untuk melihat tabel MAC.

c. **Layer 3 (Network)**:
   - Verifikasi konfigurasi IP pada perangkat klien.
   - Periksa tabel routing pada router dengan `show ip route`.
   - Pastikan gateway default terkonfigurasi dengan benar.
   - Uji konektivitas ke gateway dengan `ping`.

d. **Layer 4 (Transport)**:
   - Periksa konfigurasi firewall yang mungkin memblokir traffic.
   - Gunakan `traceroute` untuk mengidentifikasi di mana paket terhenti.
   - Verifikasi bahwa NAT (Network Address Translation) berfungsi dengan benar.

e. **Layer 7 (Aplikasi)**:
   - Periksa konfigurasi DNS untuk memastikan nama domain dapat diselesaikan.
   - Uji konektivitas ke IP eksternal (misalnya `ping 8.8.8.8`).
   - Verifikasi pengaturan proxy jika digunakan.

#### 3. Desain Jaringan dengan VLAN untuk Keamanan

**Konfigurasi switch Layer 2 dan trunking:**

a. **Pembuatan VLAN**:
   - Buat VLAN terpisah untuk setiap departemen (HR = VLAN 10, IT = VLAN 20, Finance = VLAN 30).
   - Berikan nama deskriptif untuk setiap VLAN untuk memudahkan identifikasi.

b. **Port Assignment**:
   - Konfigurasi port yang terhubung ke perangkat klien sebagai access port dan assign ke VLAN yang sesuai.
   - Gunakan perintah `switchport mode access` dan `switchport access vlan [vlan-id]`.

c. **Trunk Configuration**:
   - Konfigurasi port yang menghubungkan switch sebagai trunk port.
   - Gunakan perintah `switchport mode trunk` dan `switchport trunk allowed vlan [vlan-list]`.
   - Implementasikan 802.1Q untuk tagging VLAN antar switch.

d. **Inter-VLAN Routing**:
   - Gunakan router atau Layer 3 switch untuk memfasilitasi komunikasi antar VLAN.
   - Konfigurasi subinterface pada router untuk setiap VLAN dengan encapsulation dot1Q.
   - Terapkan kebijakan akses untuk mengontrol lalu lintas antar VLAN.

**Pengaruh terhadap kinerja jaringan:**

- **Keamanan**: VLAN meningkatkan keamanan dengan memisahkan lalu lintas departemen, mengurangi risiko akses tidak sah.
- **Performa**: Segmentasi jaringan mengurangi broadcast domain, yang dapat meningkatkan performa dengan mengurangi traffic yang tidak perlu.
- **Overhead**: Konfigurasi VLAN menambah sedikit overhead karena proses tagging dan untagging frame.
- **Skalabilitas**: VLAN memudahkan penambahan perangkat baru tanpa perlu perubahan fisik jaringan.
- **Kompleksitas**: Manajemen jaringan menjadi lebih kompleks dengan bertambahnya jumlah VLAN.

#### 4. Troubleshooting Komunikasi Antar VLAN

**Pendekatan identifikasi dan penyelesaian masalah:**

a. **Verifikasi Konfigurasi Trunk**:
   - Periksa konfigurasi trunk pada kedua switch untuk memastikan keduanya dalam mode trunk.
   - Gunakan perintah `show interfaces trunk` untuk melihat status trunk.
   - Pastikan VLAN yang bermasalah termasuk dalam daftar VLAN yang diizinkan pada trunk.

b. **Analisis Frame Data**:
   - Gunakan packet analyzer seperti Wireshark untuk menangkap frame pada link trunk.
   - Periksa header 802.1Q untuk memastikan frame memiliki tag VLAN yang benar.
   - Verifikasi bahwa frame dari VLAN sumber memiliki tag yang sesuai dan tidak diubah saat melintasi trunk.

c. **Pemeriksaan Konfigurasi VLAN**:
   - Pastikan VLAN yang bermasalah ada dan aktif pada kedua switch.
   - Gunakan perintah `show vlan brief` untuk memverifikasi keberadaan VLAN.
   - Periksa apakah ada perbedaan konfigurasi VLAN antara switch.

d. **Verifikasi Forwarding**:
   - Gunakan perintah `show mac address-table` untuk melihat apakah MAC address device tujuan telah dipelajari.
   - Periksa apakah ada port yang dalam status blocked oleh STP.
   - Uji konektivitas dengan `ping` antar device pada VLAN yang sama terlebih dahulu.

**Verifikasi frame pada trunk:**

- Gunakan perintah `show interfaces [interface] trunk` untuk melihat VLAN yang diizinkan dan status trunk.
- Gunakan `show interfaces [interface] counters` untuk melihat statistik frame yang ditransmisikan dan diterima.
- Implementasikan SPAN (Switched Port Analyzer) untuk menyalin traffic trunk ke port monitoring dan analisis dengan Wireshark.
- Periksa log switch untuk error terkait trunk atau VLAN.

## Kesimpulan

Dari hasil pengujian, dapat disimpulkan bahwa:
1. Konfigurasi VLAN telah berhasil diimplementasikan pada semua switch.
2. Perangkat dalam satu VLAN yang sama dapat saling berkomunikasi meskipun berada pada switch yang berbeda.
3. Perangkat dalam VLAN yang berbeda tidak dapat saling berkomunikasi sesuai dengan konsep VLAN.
4. Konfigurasi DHCP untuk VLAN 20 dan VLAN 30 berfungsi dengan baik.
5. Implementasi trunking antar switch berfungsi dengan benar untuk menghubungkan VLAN yang sama di berbagai switch.

---
**luqmanaru**
