# Project: Dashboard & Website Booking — Logic DAMRI

Tujuan:
Buat desain website dashboard dan antarmuka pemesanan penumpang & kargo untuk DAMRI. Aplikasi harus responsif (desktop + mobile), mudah dipakai, dan mendukung dua peran: Penumpang (user) dan Petugas/Admin.

Fitur utama:
1. Auth & Booking cepat:
   - Penumpang dapat mendaftar / login via email + no. HP (OTP).
   - Langsung pilih rute, jadwal, pilih kursi visual (seat map), dan lakukan pembayaran instan (kartu/OVO/GoPay/virtual account).
   - Tampilkan ringkasan pemesanan sebelum checkout (rincian penumpang, kursi, tarif, biaya bagasi jika ada).

2. Cek ketersediaan kursi & cargo:
   - Halaman rute menampilkan ketersediaan kursi real-time (tersedia/terisi/reserved).
   - Modul Cargo/Bagasi: input berat/volume, opsi titipan, dan estimasi tarif. Tampilkan kapasitas bagasi sisa per armada/jadwal.

3. Dashboard Petugas/Admin:
   - Ringkasan KPI di atas: jumlah penumpang hari ini/minggu/bulan, total pendapatan, jumlah cargo/berat terangkut.
   - Grafik tren (bar/line) selectable timeframe (Har, Minggu, Bulan).
   - Tabel rekap harian dengan filter rute, armada, status payment.
   - Ekspor CSV/PDF, print manifest, dan tombol untuk melihat detail booking.

UX / Visual:
- Warna utama merepresentasikan DAMRI (gunakan hijau tua + aksen oranye/kuning), gunakan visual bersih, ikon transportasi.
- Desktop: sidebar kiri (navigasi), header top (search, notif, user), content utama.
- Mobile: bottom nav (Home, Booking, Tracking, Profile).

Elemen UI spesifik:
- Komponen Seat map interaktif (selectable seats, show price per seat).
- Komponen Cargo volume input (dimensi + berat -> kalkulasi tarif).
- Chart area: line chart untuk tren penumpang, pie chart jenis tiket (reguler/vip/cargo).
- Notifikasi real-time (websocket) untuk booking baru & pembayaran sukses.

Deliverables:
- High-fidelity screens: Landing, Auth, Search Rute, Seat Selection, Checkout, Booking Confirm, Cargo Form, User Profile, Admin Dashboard (KPI + Rekap + Detail Booking).
- Interactive prototype (Figma) + style guide (colors, typography, spacing).
- Hand-off notes untuk dev (komponen, states, responsive breakpoints).
