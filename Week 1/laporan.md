# Laporan Praktikum Week 1: Instalasi Python dan Wireshark

## Tujuan Praktikum
Praktikum ini bertujuan untuk:
1. Menginstal dan mengkonfigurasi Python sebagai bahasa pemrograman untuk AI
2. Menginstal dan menggunakan Wireshark untuk analisis network traffic
3. Memahami cara kerja packet capture dan analisis protokol jaringan

---

## 1. Instalasi Python

### Proses Instalasi
Python adalah bahasa pemrograman yang sangat populer dalam bidang Kecerdasan Artifisial dan Data Science. Langkah-langkah instalasi Python:

1. **Download**: Mengunjungi situs resmi python.org dan mengunduh versi terbaru
2. **Setup Wizard**: Menjalankan file installer dan mengikuti wizard instalasi
3. **Konfigurasi Path**: Memastikan Python ditambahkan ke PATH sistem
4. **Verifikasi**: Membuka terminal dan mengetikkan `python --version` untuk memverifikasi instalasi berhasil

Setelah Python berhasil diinstal, Anda dapat menjalankan script Python dan menginstal library tambahan menggunakan `pip` (Python Package Manager).

---

## 2. Instalasi Wireshark

### Tentang Wireshark
Wireshark adalah tool open-source yang digunakan untuk menangkap, filter, dan menganalisis network traffic secara real-time. Tool ini sangat berguna untuk:
- Network troubleshooting
- Protocol analysis
- Security monitoring
- Learning network communication

### Proses Instalasi
Wireshark dapat diunduh dari wireshark.org dan diinstal dengan mengikuti wizard instalasi standar.

---

## 3. Analisis Screenshot Wireshark

### Screenshot 1 (img 1.png): HTTP Traffic Analysis
![HTTP Traffic Capture](Asset/img%201.png)

**Deskripsi:**
- Menampilkan packet capture dengan protokol **HTTP** (Hypertext Transfer Protocol)
- Status baris menunjukkan **"http"** dengan warna hijau (normal state)
- Terlihat berbagai GET request dari source IP **10.217.1.160** ke destination **199.232.214.172**
- Paket menunjukkan response **HTTP/1.1 304 Not Modified**, yang berarti resource tidak berubah sejak request terakhir
- Kolom "Length Info" menampilkan ukuran data dalam setiap paket (261 bytes, 254 bytes, 280 bytes, dll)

**Analisis:**
Capture ini menunjukkan traffic HTTP normal yang terjadi saat browsing website. Server merespons dengan status 304, mengindikasikan bahwa browser sudah memiliki versi terbaru dari resource tersebut.

---

### Screenshot 2 (img 2.png): DNS & TCP Traffic
![DNS and TCP Traffic](Asset/img%202.png)

**Deskripsi:**
- Status bar menunjukkan protokol campuran: **TCP**, **TLSv1.2**, dan **ARP**
- Terlihat **Domain Name System (query)** yang ditandai dengan warna kuning di bagian bawah
- Banyak paket TCP dengan berbagai port, termasuk port 52090 dan port 53 (DNS)
- Query terlihat pada port 57894 (UDP/DNS port)

**Analisis:**
Capture ini menunjukkan proses DNS resolution yang terjadi saat perangkat mencoba menerjemahkan domain name menjadi IP address. Terlihat juga TCP traffic yang digunakan untuk koneksi data.

---

### Screenshot 3 (img 3.png): HTTP Traffic dengan Packet Detail
![HTTP Packet Detail](Asset/img%203.png)

**Deskripsi:**
- Kembali menampilkan protokol **HTTP** dengan status hijau
- Packet ke-96 dipilih (highlighted) untuk menunjukkan detail
- Response **HTTP/1.1 304 Not Modified** (ukuran 370 bytes)
- Jenis protokol yang terlihat: GET request HTTP/1.1
- Detail packet menunjukkan informasi lengkap tentang headers dan payload

**Analisis:**
Screenshot ini menunjukkan fitur detail inspection di Wireshark. Dengan mengklik packet tertentu, kita dapat melihat struktur lengkap dari HTTP request/response, termasuk headers, cookies, user-agent, dan informasi lainnya yang penting untuk debugging dan security analysis.

---

### Screenshot 4 (img 4.png): Network Broadcast & ARP Traffic
![Broadcast and ARP Traffic](Asset/img%204.png)

**Descripsi:**
- Status menunjukkan "**Capturing from Wi-Fi**" dengan indikator status berwarna hijau
- Terlihat berbagai protokol: **TCP**, **ARP**, **ICMP**, dan **DNS**
- Packet menunjukkan **Broadcast traffic** (IP broadcast ke 128.119.245.12)
- **ARP (Address Resolution Protocol)** traffic untuk resolusi MAC address
- Error message di bawah: "*is not a valid protocol or protocol field*" (kemungkinan filter syntax error)

**Analisis:**
Capture ini menunjukkan network layer yang lebih lengkap, termasuk:
- **ARP**: Digunakan untuk menerjemahkan IP address menjadi MAC address di LAN
- **ICMP**: Protokol untuk error reporting dan diagnostik (seperti ping)
- **Broadcast**: Paket yang dikirim ke semua device di network segment

---

## 4. Kesimpulan

Dari praktikum ini, kami telah berhasil:

✅ **Menginstal Python** - Bahasa pemrograman utama untuk AI dan Data Science
✅ **Menginstal Wireshark** - Tool profesional untuk network analysis
✅ **Memahami Network Protocols** - HTTP, TCP, UDP, DNS, ARP, ICMP
✅ **Melakukan Packet Capture** - Menangkap dan menganalisis traffic jaringan real-time
✅ **Interpretasi Packet Data** - Membaca dan memahami informasi yang ditampilkan Wireshark

Kedua aplikasi ini merupakan tools fundamental untuk:
- **Python**: Mengembangkan aplikasi AI dan machine learning
- **Wireshark**: Network troubleshooting, security analysis, dan pembelajaran jaringan

---

## 5. Referensi

- **Python Documentation**: https://docs.python.org/
- **Wireshark Official**: https://www.wireshark.org/
- **HTTP Protocol**: RFC 2616
- **TCP/IP Protocol Suite**: RFC 791, RFC 793

---

**Tanggal Praktikum**: 7 Maret 2026  
**Status**: Selesai ✓
