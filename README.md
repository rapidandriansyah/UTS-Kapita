# Penerapan Kontrol Histeresis dengan Variasi Deadband untuk Menekan Frekuensi Switching dan Menjaga Rentang Kelembapan serta Suhu pada Kandang Tikus Putih Berbasis IoT

Repositori ini berisi berkas dan dokumentasi tugas mata kuliah **Kapita Selekta / Proposal Tugas Akhir** mahasiswa Program Studi Teknik Informatika, Fakultas Ilmu Komputer, Universitas Esa Unggul.

---

## 📌 Identitas Peneliti

- **Nama**: Rapid Andriansyah
- **NIM**: 20230801129
- **Program Studi**: Teknik Informatika
- **Fakultas**: Fakultas Ilmu Komputer
- **Perguruan Tinggi**: Universitas Esa Unggul, Tangerang
- **Tahun**: 2026

---

## 📖 Ringkasan Proyek

Pemeliharaan koloni tikus putih (*Rattus norvegicus*) pada kandang tertutup menuntut kestabilan iklim mikro yang ketat:
- **Rentang Suhu Ideal**: 19°C – 26°C
- **Rentang Kelembapan Relatif (RH) Ideal**: 30% – 70% RH

Sistem otomasi ventilasi konvensional berbasis ambang tunggal (*single threshold*) sering mengalami masalah **relay chattering** (pensaklaran relay berulang kali dalam frekuensi tinggi akibat derau fluktuasi sensor). Hal ini memicu keausan mekanis relay (*contact arcing*), lonjakan arus asut (*inrush current*), dan instabilitas sistem.

Penelitian ini merancang sistem kendali loop tertutup (*closed-loop*) berbasis **Internet of Things (IoT)** menggunakan:
1. **Mikrokontroler ESP32** sebagai pengendali utama dan pengirim telemetri nirkabel.
2. **Sensor DHT22 (AM2302)** untuk akuisisi suhu dan kelembapan secara akurat.
3. **Algoritma Kontrol Histeresis Ganda** yang memisahkan titik ambang ON dan OFF melalui pita mati (*deadband*).
4. **Waktu Tunda Verifikasi (*Time Delay $\tau_d$*)** untuk memvalidasi fluktuasi transien sebelum tindakan sakelar dieksekusi.
5. **Modul Relay Optocoupler & Kipas Ventilasi DC 12V (*Exhaust*)** sebagai aktuator sirkulasi udara kandang.

---

## 🛠️ Arsitektur & Spesifikasi Sistem

### 1. Perangkat Keras (Hardware)
| No | Komponen | Spesifikasi Teknis | Fungsi |
|---|---|---|---|
| 1 | **ESP32-WROOM-32** | Dual-Core 32-bit, 240 MHz, Wi-Fi 2.4 GHz | Pengolah data kendali utama & transmisi MQTT |
| 2 | **Sensor DHT22 (AM2302)** | Resolusi 0,1°C & 0,1% RH; Akurasi ±0,5°C & ±2% RH | Pembacaan suhu dan kelembapan kandang |
| 3 | **Modul Relay 1-Kanal** | Pemicu 5V DC dengan optocoupler isolasi | Sakelar beban daya kipas DC |
| 4 | **Kipas Ventilasi DC 12V** | *Brushless* DC 12V 0,25A, 2000 RPM | *Exhaust fan* pembuang udara panas & lembap |
| 5 | **Catu Daya & Step-Down** | Adaptor 12V 2A & Modul LM2596 (12V ke 5V DC) | Penyuplai daya modul dan mikrokontroler |
| 6 | **Kandang Tikus Putih** | Kotak polipropilena tertutup (40 × 30 × 20 cm) | Wadah pemeliharaan objek penelitian |

### 2. Skenario Pengujian Algoritma Histeresis & Deadband
| Kode | Skenario Pengujian | Titik Batas Atas (ON) | Titik Batas Bawah (OFF) | Lebar Deadband ($\Delta T$ / $\Delta H$) | Time Delay ($\tau_d$) |
|---|---|---|---|---|---|
| **S0** | Ambang Tunggal (*Baseline*) | 26,0°C / 70,0% RH | 26,0°C / 70,0% RH | 0,0°C / 0,0% RH | 0 detik |
| **S1** | Histeresis Sempit + Time Delay | 26,0°C / 70,0% RH | 25,5°C / 68,0% RH | 0,5°C / 2,0% RH | 3 detik |
| **S2** | Histeresis Sedang + Time Delay | 26,0°C / 70,0% RH | 24,5°C / 65,0% RH | 1,5°C / 5,0% RH | 3 detik |
| **S3** | Histeresis Lebar + Time Delay | 26,0°C / 70,0% RH | 23,0°C / 60,0% RH | 3,0°C / 10,0% RH | 3 detik |

---

## 📁 Struktur Berkas Repositori

```text
├── UTS KAPITA.pdf   # Laporan lengkap Proposal Tugas Akhir (Bab I s/d Bab V)
├── BRD.pdf          # Business Requirements Document (BRD) Sistem Monitoring Kandang IoT
├── SLR.pdf          # Systematic Literature Review (SLR) berbasis PRISMA 2020
└── README.md        # Dokumentasi repositori
```

### Rincian Dokumen:
1. **[UTS KAPITA.pdf](file:///D:/kapita/UTS%20KAPITA.pdf)**:
   - **BAB I Pendahuluan**: Latar belakang, rumusan masalah, tujuan, batasan, kerangka berpikir, sistematika.
   - **BAB II Tinjauan Pustaka**: Teori histeresis, IoT mikroklimat kandang, perbandingan 10 penelitian terdahulu, research gap.
   - **BAB III Metodologi Penelitian**: Arsitektur sistem, skematik pengawatan, alur diagram (flowchart) histeresis ber-time delay, skenario pengujian.
   - **BAB IV Hasil yang Diharapkan**: Target indikator capaian, peredaman chattering hingga 100%, efisiensi frekuensi switching > 75%, target publikasi.
   - **BAB V Penutup & Daftar Pustaka**: Kesimpulan konseptual, saran penelitian ke depan, dan pustaka acuan.
2. **[BRD.pdf](file:///D:/kapita/BRD.pdf)**:
   - Dokumen *Business Requirements Document* yang menjabarkan kebutuhan fungsional (FR) dan non-fungsional (NFR), analisis *stakeholder*, matriks penelusuran (*traceability matrix*), proses bisnis *As-Is* vs *To-Be*, serta *risk register*.
3. **[SLR.pdf](file:///D:/kapita/SLR.pdf)**:
   - Dokumen protokol *Systematic Literature Review* berdasarkan pedoman PRISMA 2020, sintesis tematik multi-kandidat dari Scopus AI/database ilmiah, dan evaluasi kualitas penelitian.

---

## 🚀 Cara Menjalankan Git / Clone Repositori

```bash
# Melakukan kloning repositori
git clone https://github.com/rapidandriansyah/UTS-Kapita.git

# Masuk ke direktori
cd UTS-Kapita
```

---
*Dibuat untuk memenuhi tugas mata kuliah Kapita Selekta / Proposal Tugas Akhir - Universitas Esa Unggul.*
