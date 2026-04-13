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
DNS (Domain Name System) adalah protokol krusial di internet yang berfungsi sebagai "buku telepon", menerjemahkan nama host yang mudah diingat (seperti `mit.edu`) menjadi alamat IP yang dipahami oleh mesin. Praktikum ini berfokus pada sisi klien, di mana klien mengirimkan permintaan ke resolver (server DNS lokal) dan menerima jawaban tanpa perlu memproses hirarki DNS yang kompleks secara manual.

---

## C. Hasil dan Pembahasan

### 1. Nslookup
Perintah `nslookup` digunakan untuk melakukan query ke DNS server. Alat ini sangat berguna untuk mendiagnosis masalah resolusi nama dan melihat informasi record tertentu pada sebuah domain.

#### a. nslookup www.mit.edu
![nslookup www.mit.edu](Asset/1.png)
**Analisis:**
- Permintaan ditangani oleh server DNS lokal dengan alamat `fe80::1` (gpon.net).
- Hasil menunjukkan bahwa `www.mit.edu` adalah sebuah alias (CNAME) yang merujuk pada `www.mit.edu.edgekey.net` dan kemudian ke `e9566.dscb.akamaiedge.net`.
- Alamat IPv4 yang didapatkan untuk host tersebut adalah **23.0.173.210**.

#### b. nslookup –type=NS mit.edu
![nslookup –type=NS mit.edu](Asset/2.png)
**Analisis:**
- Perintah ini secara spesifik meminta **Name Server (NS)** record dari MIT.
- Terdapat beberapa server otoritatif yang mengelola domain ini, seperti `asia1.akam.net`, `ns1-173.akam.net`, dan lainnya. Status "Non-authoritative answer" menunjukkan data diambil dari cache resolver lokal.

#### c. Query ke DNS Server Spesifik (Google DNS)
![Query ke DNS Server Tertentu](Asset/3.png)
**Analisis:**
- Pada perintah `nslookup mit.edu 8.8.8.8`, query dipaksa untuk dikirim ke server Google (`8.8.8.8`), bukan ke server DNS default.
- Hasil menunjukkan alamat IP MIT dari perspektif server Google adalah **23.15.150.186**.

#### d. Query Alamat IP Server Web di Asia (SNU)
*(Tertera pada bagian bawah Screenshot 2026-04-13 205059.png)*
**Analisis:**
- Melakukan query untuk `en.snu.ac.kr` (Seoul National University).
- Hostname tersebut memiliki alias `new.snu.ac.kr` dengan alamat IPv4 **147.46.10.129**.

#### e. Query DNS Otoritatif (NS Record - ox.ac.uk)
![Query DNS Otoritatif](Asset/5.png)
**Analisis:**
- Mencari record NS untuk University of Oxford (`ox.ac.uk`).
- Domain ini dikelola oleh 6 nameserver otoritatif, mulai dari `dns0.ox.ac.uk` hingga `auth6.dns.ox.ac.uk`.

#### f. Query MX Record (Mail Server - ox.ac.uk)
![Query MX Record](Asset/6.png)
**Analisis:**
- Parameter `-type=MX` digunakan untuk mencari server penanganan email.
- Didapatkan server mail `oxforduni.in.tmes.trendmicro.eu` dengan **preference = 4**. Angka ini menunjukkan tingkat prioritas server tersebut dalam menerima email masuk.

---

### 2. Ipconfig
Perintah `ipconfig` memberikan gambaran mengenai konfigurasi jaringan yang terpasang pada komputer klien.

#### a. ipconfig /all
![ipconfig /all](Asset/7.png)
**Analisis:**
- **Host Name:** Laptop bernama `Owen`.
- **Physical Address (MAC):** `08-8F-C3-46-69-B2`.
- **IPv4 Address:** `192.168.56.1` (VirtualBox Adapter).
- Informasi ini penting untuk mengetahui konfigurasi default gateway dan server DNS yang digunakan oleh sistem saat ini.

---

### 3. Tracing DNS dengan Wireshark

#### a. Tracing DNS Dasar (www.mit.edu)
![Tracing DNS 1](Asset/15.png)
![Tracing DNS 2](Asset/16.png)

**Pertanyaan dan Jawaban:**
1. **Pesan dikirim melalui UDP atau TCP?**
   > Pesan dikirim melalui protokol **UDP**, yang merupakan protokol standar untuk DNS karena lebih cepat dan efisien untuk query singkat.
2. **Apa port tujuan pada pesan permintaan? Port sumber pada pesan balasan?**
   > Port tujuan permintaan adalah **53**. Port sumber pada balasan juga adalah **53**.
3. **Apa alamat IP tujuannya? Apakah sama dengan DNS server lokal?**
   > IP tujuan adalah **192.168.110.1**. Ya, ini sesuai dengan default gateway/DNS server lokal pada konfigurasi jaringan yang digunakan (192.168.110.6 mengirim ke 192.168.110.1).
4. **Apa tipe pesan tersebut? Apakah mengandung jawaban?**
   > Tipe pesan pada Query (Frame 269) adalah **Tipe A** (Host Address). Pesan query tidak mengandung jawaban (Answer RRs: 0).
5. **Berapa banyak jawaban pada pesan balasan? Apa isinya?**
   > Terdapat **3 jawaban** (Frame 307). Isinya adalah dua alias (CNAME) dan satu alamat IPv4 (**23.217.163.122**).

#### b. Query ke DNS Server Spesifik (www.aiit.or.kr via 8.8.8.8)
![Tracing Spesifik 1](Asset/17.png)
![Tracing Spesifik 2](Asset/18.png)

**Analisis Wireshark:**
1. **Ke alamat IP manakah pesan dikirim?**
   > Dikirim ke **8.8.8.8**. Ini bukan DNS lokal default, melainkan DNS publik Google yang dipanggil secara eksplisit melalui nslookup.
2. **Apa tipe pesan dan apakah ada jawaban pada query?**
   > Tipe pesan adalah **A (IPv4)**. Query (Frame 138) tidak membawa jawaban.
3. **Berapa banyak jawaban pada balasan? Apa isinya?**
   > Terdapat **2 jawaban** (Frame 149) berupa record tipe A dengan alamat IP **172.67.152.120** dan **104.21.74.8**.

---

## D. Kesimpulan
Berdasarkan praktikum Modul 4, dapat disimpulkan bahwa DNS bekerja pada lapisan aplikasi menggunakan protokol transport UDP melalui port 53. DNS memungkinkan pengguna mengakses sumber daya internet menggunakan nama yang intuitif. Melalui analisis Wireshark dan nslookup, kita dapat melihat bahwa satu nama domain seringkali memiliki banyak alias (CNAME) dan dikelola oleh beberapa nameserver otoritatif untuk menjaga ketersediaan layanan.

---

## E. Daftar Pustaka
1. Kurose, J.F., & Ross, K.W. (2021). *Computer Networking: A Top-Down Approach*, 8th Edition. Pearson.
2. Universitas Telkom. (2026). *Modul Praktikum Jaringan Komputer Semester Genap 2025/2026*.
