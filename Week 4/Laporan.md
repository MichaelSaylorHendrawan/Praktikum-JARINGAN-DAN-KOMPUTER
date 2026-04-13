# **LAPORAN PRAKTIKUM JARINGAN KOMPUTER - MODUL 4**
## **Domain Name System (DNS)**

---

### **Identitas Mahasiswa**
**Nama:** Zain Ahmad Suraiban  
**NIM:** 103072430001  
**Kelas:** IF - 04 - 01

---

## A. Tujuan Praktikum
1. Mahasiswa mampu menginvestigasi cara kerja protokol DNS menggunakan Wireshark.
2. Mahasiswa memahami struktur pesan DNS (Query & Response) serta berbagai tipe record (A, NS, MX, CNAME).

---

## B. Pengantar
DNS (Domain Name System) adalah protokol lapisan aplikasi yang berfungsi menerjemahkan nama domain (hostname) yang mudah diingat manusia menjadi alamat IP yang dapat diproses oleh mesin. Praktikum ini menganalisis bagaimana klien melakukan permintaan resolusi nama, baik melalui resolver lokal maupun server DNS spesifik, serta bagaimana record tersebut disimpan dalam cache sistem operasi.

---

## C. Hasil dan Pembahasan

### 1. Nslookup
Perintah `nslookup` digunakan untuk melakukan kueri langsung ke server DNS guna mendapatkan informasi pemetaan IP atau record lainnya.

#### a. nslookup www.mit.edu
![nslookup www.mit.edu](Asset/1.png)
**Analisis:**
- Permintaan dilayani oleh server lokal `gpon.net` dengan alamat IPv6 `fe80::1`.
- Hasil menunjukkan `www.mit.edu` adalah alias (CNAME) dari `www.mit.edu.edgekey.net`, yang kemudian merujuk ke `e9566.dscb.akamaiedge.net`.
- Alamat IPv4 akhirnya adalah **23.0.173.210**.

#### b. nslookup –type=NS mit.edu
![nslookup –type=NS mit.edu](Asset/2.png)
**Analisis:**
- Kueri tipe **NS (Name Server)** mencari server otoritatif domain MIT.
- Terdapat banyak nameserver yang mengelola domain ini (misal: `asia1.akam.net`, `ns1-173.akam.net`).

#### c. Query ke DNS Server Tertentu (Google DNS)
![Query DNS Google](Asset/3.png)
**Analisis:**
- Kueri `nslookup mit.edu 8.8.8.8` memaksa permintaan dikirim langsung ke Google Public DNS.
- Terdeteksi alamat IPv4 MIT dari server ini adalah **23.15.150.186**.

#### d. Query Alamat IP Server Web di Asia (SNU)
![Query SNU Asia](Asset/webasia.png)
**Analisis:**
- Melakukan kueri untuk `en.snu.ac.kr`.
- Hostname ini merujuk pada alias `new.snu.ac.kr` dengan alamat IP **147.46.10.129**.

#### e. Query DNS Otoritatif (NS Record - ox.ac.uk)
![Query NS Oxford](Asset/4.png)
**Analisis:**
- Menemukan nameserver otoritatif University of Oxford, seperti `dns0.ox.ac.uk` hingga `auth6.dns.ox.ac.uk`.

#### f. Query MX Record (Mail Server - ox.ac.uk)
![Query MX Oxford](Asset/5.png)
**Analisis:**
- Kueri tipe **MX** mencari server email. Teridentifikasi server `oxforduni.in.tmes.trendmicro.eu` dengan nilai **preference = 4**.

---

### 2. Ipconfig
`ipconfig` menyajikan detail konfigurasi antarmuka jaringan dan pengelolaan cache DNS pada host.

#### a. ipconfig /all
![ipconfig all 1](Asset/6.png)
![ipconfig all 2](Asset/7.png)
![ipconfig all 3](Asset/8.png)
**Analisis:**
- **Host Name:** Owen.
- **Ethernet Adapter:** Killer E2600.
- **Wi-Fi Adapter:** Memiliki IPv4 **192.168.1.8** dengan DNS Servers **1.1.1.1** dan **1.0.0.1** (Cloudflare DNS).

#### b. ipconfig /displaydns
![displaydns 1](Asset/9.png)
![displaydns 2](Asset/10.png)
![displaydns 3](Asset/11.png)
**Analisis:**
- Perintah ini menampilkan isi cache DNS lokal.
- Terlihat record seperti `telemetry-incoming.r53-2.services.mozilla.com` (Tipe 1/A) dan `Owen.mshome.net` (Tipe 1).
- Gambar 11 menunjukkan **PTR Record** (Tipe 12) untuk reverse lookup dan **AAAA Record** (Tipe 28) untuk alamat IPv6.

---

### 3. Tracing DNS dengan Wireshark

#### a. Tracing DNS Dasar (Cloudflare DNS)
![Wireshark Tracing 1](Asset/12.png)
![Wireshark Tracing 2](Asset/13.png)
**Analisis:**
- Klien (192.168.1.8) mengirim query ke **1.1.1.1** via protokol **UDP Port 53**.
- Gambar 13 menunjukkan rincian respons DNS (Transaction ID: `0x42ab`) untuk `www.google.co.id` tipe AAAA dengan 1 jawaban (Answer RRs: 1).

#### b. Tracing DNS Dasar (mit.edu)
![Wireshark mit 1](Asset/15.png)
![Wireshark mit 2](Asset/16.png)
**Analisis:**
- Klien (192.168.110.6) mengirim kueri ke server lokal **192.168.110.1**.
- Terdapat **3 Jawaban** pada respons (Gambar 16), yang terdiri dari dua CNAME record dan satu record Tipe A (23.217.163.122).

#### c. Query ke DNS Server Spesifik (aiit.or.kr via 8.8.8.8)
![Wireshark aiit 1](Asset/17.png)
![Wireshark aiit 2](Asset/18.png)
**Analisis:**
- Query diarahkan ke **8.8.8.8** (Google DNS).
- Respons (Gambar 18) memberikan **2 Jawaban** record Tipe A: **172.67.152.120** dan **104.21.74.8**.

---

## D. Kesimpulan
DNS merupakan fondasi penting dalam navigasi internet yang memungkinkan pemetaan hostname ke alamat IP secara efisien. Melalui praktikum ini, dapat dibuktikan bahwa DNS menggunakan protokol UDP port 53 untuk kecepatan kueri. Penggunaan `nslookup` mempermudah identifikasi berbagai tipe record (A, CNAME, NS, MX), sementara `ipconfig` membantu melihat konfigurasi DNS yang aktif dan pengelolaan cache pada sisi klien.

---

## E. Daftar Pustaka
1. Kurose, J.F., & Ross, K.W. (2021). *Computer Networking: A Top-Down Approach*, 8th Edition. Pearson.
2. Universitas Telkom. (2026). *Modul Praktikum Jaringan Komputer Semester Genap 2025/2026*.
