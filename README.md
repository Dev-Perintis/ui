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


# Create a modern responsive web UI for "Logic DAMRI" — passenger booking + cargo + admin dashboard.
- Palette: deep green (#0F6B3E) primary, white, light gray, accent amber.
- Layout: Desktop 3-column header+sidebar+content. Mobile: stacked, bottom nav.
- Pages: Home/Search, Schedule list, Seat map visual (clickable seats), Checkout (payment methods + summary), Booking confirmation, Cargo booking form, Profile, Admin Dashboard.
- Admin Dashboard details: KPI cards (Passengers today, Revenue today, Cargo weight today), interactive line chart (time range: day/week/month), table of bookings with filters (date, route, status), export CSV/PDF.
- Interactions: seat hover tooltip (seat no, price, availability), live badge if payment pending/confirmed, modal for booking detail.
- Provide components: Buttons (primary/secondary), inputs, searchable select (route), date/time picker, data table with pagination, chart placeholders.
Output: Figma file with components, prototypes for booking flow and admin rekap flow, and simple user flows for payment success & payment failure.


# API + data model + acceptance
Project: Logic DAMRI Web App — Technical Spec (minimal viable)

User roles:
- Passenger (user)
- Staff / Petugas (admin)

Endpoints (example):
- POST /auth/register {name, email, phone, password} -> {userId, otpSent}
- POST /auth/verify-otp {phone, otp} -> {token}
- POST /auth/login {phone|email, password} -> {token}
- GET /routes -> [{routeId, origin, destination, duration, schedules[]}]
- GET /schedules/{scheduleId}/availability -> {totalSeats, availableSeats, seatMap}
- POST /booking -> {userId, scheduleId, seats[], cargo?:{weight,dim}, passengerDetails, paymentMethod} -> {bookingId, paymentUrl}
- GET /booking/{bookingId} -> full booking detail + payment status
- POST /payment/webhook -> (handle payment success/failure)
- GET /reports?from=YYYY-MM-DD&to=YYYY-MM-DD&type=daily|weekly|monthly -> {aggregates, bookings[], cargo[]}
- GET /export?from&to&type=csv|pdf -> file

Data models (simplified):
- User {id, name, phone, email, role, createdAt}
- Route {id, origin, destination, distance}
- Schedule {id, routeId, datetime, busId, capacity}
- Seat {id, scheduleId, seatNumber, class, status}
- Booking {id, userId, scheduleId, seats[], cargo:{weight,volume,fee}, priceTotal, status, paymentId, createdAt}
- CargoItem {id, bookingId, weight, dimensions, fee}

Realtime:
- WebSocket channel per schedule for seat/reservation updates and payment notifications.

Acceptance criteria / user stories:
- Penumpang dapat mendaftar & login (OTP). Setelah login, bisa search rute, pilih jadwal, lihat seat map realtime, pilih kursi, checkout dan membayar. Setelah payment sukses, booking tersimpan dan e-ticket/email terkirim.
- Penumpang dapat memasukkan data cargo/bagasi, sistem menghitung tarif berdasarkan berat dan volume, menampilkan ketersediaan cargo limit per jadwal.
- Petugas dapat melihat halaman rekap dengan KPI (jumlah penumpang/hari, pendapatan, berat cargo) untuk rentang waktu yang dipilih; dapat mengekspor CSV dan melihat detail booking (nama, kursi, status payment).
- Sistem melindungi endpoint admin (role-based auth).

Contoh response (availability):
```sh
GET /schedules/123/availability
{
 "scheduleId": 123,
 "totalSeats": 40,
 "availableSeats": 12,
 "seatMap": [
   {"seatNo":"1A","status":"occupied"},
   {"seatNo":"1B","status":"available"},
   ...
 ]
}
```

Security:
- JWT for auth, role-based middleware for /reports and admin routes.
- Rate limit for booking endpoints and idempotency on payment webhook.

Ops:
- Background job to release reserved seats after X minutes if unpaid.
- Cron/daily job to aggregate reports (optional for performance).

Tech notes:
- Frontend: React + TypeScript, Tailwind (or Bootstrap for quick), chart library (recharts), seatmap as SVG/Canvas.
- Backend: Node.js/Express or Laravel, DB: PostgreSQL. Use Redis for seat reservations and websocket pubsub.

