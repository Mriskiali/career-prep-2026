# Interview Preparation — Mobile Platform Development Intern

## Struktur Wawancara Umum
1. **Screening HR** (10-15 min) — perkenalan, motivasi, jadwal magang
2. **Technical** (30-60 min) — React Native, JS/TS, problem solving
3. **User/Culture** — teamwork, learning attitude

---

## A. Pertanyaan HR / Behavioral

### 1. "Ceritakan tentang diri kamu"
> "Saya Mu'afa Riski Ali, fresh graduate Informatika Universitas Gunadarma dengan IPK 3.76. Fokus saya di pengembangan aplikasi mobile menggunakan React Native dan Expo, tapi saya juga punya latar belakang fullstack. Proyek terakhir saya Motion-fits, fitness tracker dengan Expo Router, dan Bloxboxd yang mengintegrasikan REST API dan database cloud. Saya suka mengantar proyek dari ide sampai bisa dipakai orang."

**Tips:** 60-90 detik max. Selalu akhiri dengan kenapa posisi ini relevan.

### 2. "Kenapa mau magang di posisi Mobile Platform Development?"
> "Karena mobile development itu bidang yang saya paling enjoy — saya suka ngelihat hasil kerja saya langsung bisa dipegang pengguna. Saya sudah pegang React Native dan Expo, dan saya ingin belajar dari tim yang lebih berpengalaman soal best practice, performance optimization, dan cara kerja tim mobile di industri nyata."

### 3. "Apa kelebihan dan kekurangan kamu?"
> **Kelebihan:** "Cepat belajar hal baru — waktu saya butuh bikin model ML dari nol, saya pelajari TensorFlow dan deployment dalam beberapa minggu sampai bisa publik."
> **Kekurangan (jujur tapi konstruktif):** "Saya kadang terlalu perfeksionis soal UI detail, sampai bisa buang waktu. Tapi sekarang saya belajar time-boxing dan prioritaskan fitur berdampak dulu."

### 4. "Kenapa IPK kamu 3.76, bukan 4.0?"
> "Saya alokasikan waktu besar untuk proyek nyata — skripsi, proyek mobile, platform Bloxboxd — supaya belajar hal yang tidak diajarkan di kelas. Nilai saya tetap bagus, dan saya rasa pengalaman praktis itu yang lebih bernilai buat kerja tim."

### 5. "Di mana kamu lihat diri kamu 5 tahun lagi?"
> "Saya lihat diri saya sebagai mobile engineer yang bisa memimpin fitur — mungkin senior atau tech lead di tim mobile. Saya juga tertarik memperdalam arsitektur aplikasi skala besar dan kontribusi open source."

---

## B. Pertanyaan Teknis React Native / Expo

### 6. "Apa perbedaan Expo dan React Native CLI?"
> "Expo itu framework di atas React Native yang provides tooling, modul native siap pakai, dan Expo Go buat testing cepat tanpa Android Studio/Xcode. Cocok untuk cepat. React Native CLI lebih bare-metal — akses penuh ke native code, tapi setup lebih ribet dan butuh build sendiri. Saya pakai Expo karena development speed-nya lebih cepat, dan kalau butuh native module custom bisa pakai EAS Build / development build."

### 7. "Jelaskan Expo Router dan file-based navigation"
> "Expo Router itu router berbasis file. Struktur folder di `app/` otomatis jadi route — `app/index.tsx` jadi halaman awal, `app/(tabs)/_layout.tsx` jadi tab navigator, `app/profile/[id].tsx` jadi dynamic route dengan param `id`. Ini mirip Next.js di web. Keuntungannya: route jadi self-documenting, deep linking otomatis jalan, dan tidak perlu tulis bikin navigator secara manual."

**Contoh struktur:**
```
app/
├── _layout.tsx          # root stack
├── (tabs)/
│   ├── _layout.tsx      # tab navigator
│   ├── index.tsx        # / (Home tab)
│   └── profile.tsx      # /profile
└── details/[id].tsx     # /details/123
```

### 8. "Bagaimana cara ngatur state di React Native?"
> "Tergantung kompleksitas. Untuk state lokal pakai `useState`/`useReducer`. Untuk state yang dipakai banyak komponen, pakai Context API kalau sederhana, atau Zustand/Redux Toolkit kalau kompleks. Di proyek saya, saya pakai Zustand karena API-nya minimalis dan tidak perlu boilerplate banyak kaya Redux. Untuk server data, React Query bagus buat caching dan revalidation."

### 9. "Apa itu `useCallback` dan `useMemo`? Kapan pakai?"
> "`useMemo` menghitung nilai berat dan cache hasilnya, `useCallback` cache function reference supaya tidak bikin ulang tiap render. Pakai kalau: (1) komputasi mahal, (2) dependency di useEffect yang bisa bikin infinite loop, (3) passing callback ke komponen yang di-memo (`React.memo`). Jangan asal pakai — setiap memo punya cost memory, jadi pakai setelah profiling, bukan preventif."

### 10. "Bagaimana optimasi performa FlatList dengan data besar?"
> - `keyExtractor` yang stabil
> - `getItemLayout` kalau tinggi item fixed
> - `React.memo` di item component
> - `initialNumToRender` & `windowSize` disetel
> - `onEndReached` buat pagination/infinite scroll
> - Hindari anonymous function inline di `renderItem` dan `keyExtractor` (bikin ulang tiap render)

### 11. "Bagaimana kamu handle async / API call?"
> "Biasanya pakai `async/await` dalam `useEffect` atau library kaya React Query. Saya selalu handle 3 state: loading, error, success. Penting buat handle race condition — kalau komponen unmount, `setState` tidak boleh dijalankan. React Query handle ini otomatis. Untuk fetch mentah, saya pakai AbortController."

```tsx
useEffect(() => {
  const controller = new AbortController();
  fetchData(controller.signal)
    .then(setData)
    .catch(err => { if (err.name !== 'AbortError') setError(err); });
  return () => controller.abort();
}, []);
```

### 12. "Apa itu EAS Build?"
> "Expo Application Services build — cloud service dari Expo buat compile aplikasi jadi binary native (APK, IPA) tanpa perlu punya Mac/Xcode sendiri. Bisa juga build secara lokal. Konfigurasinya di `eas.json`. Ini yang saya pakai kalau perlu build production atau development build yang ada native module custom."

### 13. "Bagaimana cara kerja navigasi antar screen?"
> "Pakai Expo Router: `router.push('/details/123')` buat masuk, `router.back()` buat balik, `router.replace()` buat ganti tanpa history. Param diambil dengan `useLocalSearchParams()`. Bisa juga pakai `<Link href='...'>` komponen. Setiap screen di-push ke stack, jadi back button otomatis berfungsi."

### 14. "Apa itu StyleSheet? Kenapa tidak pakai inline style?"
> "`StyleSheet.create()` meng-compile style di luar render cycle dan mengirim hanya ID ke native bridge, jadi lebih efisien dari object inline yang dibuat ulang tiap render. Juga memberi validasi error kalau ada property salah. Untuk kondisi dinamis, saya gabungkan: `style={[styles.base, isActive && styles.active]}`."

---

## C. Pertanyaan JavaScript / TypeScript

### 15. "Apa perbedaan `var`, `let`, dan `const`?"
> "`var` function-scoped dan di-hoist; `let` dan `const` block-scoped dengan TDZ (temporal dead zone). `const` tidak bisa di-reassign tapi object/array isinya masih bisa diubah. Saya pakai `const` default, `let` kalau perlu reassign, `var` tidak pernah."

### 16. "Apa itu closure?"
> "Function yang 'mengingat' scope di mana dia dibuat, meskipun scope itu sudah selesai dijalankan."
```js
function counter() {
  let count = 0;
  return () => ++count;  // closure over 'count'
}
const inc = counter();
inc(); // 1
inc(); // 2
```

### 17. "Jelaskan Promise vs async/await"
> "Promise itu object yang merepresentasikan nilai di masa depan — pending, fulfilled, rejected. `async/await` sintaks gula di atas Promise supaya kode async terlihat sinkron. `async` function selalu return Promise, `await` pause eksekusi sampai Promise selesai. Error handling jadi pakai try/catch."

### 18. "Apa itu TypeScript generic?"
> "Tipe yang parametrik — bisa dipakai dengan tipe lain tanpa kehilangan type safety."
```ts
function identity<T>(arg: T): T { return arg; }
const s = identity<string>("hi");  // string
```

### 19. "Perbedaan `interface` dan `type` di TypeScript?"
> "`interface` bisa di-declare merge dan lebih cocok untuk bentuk object. `type` lebih fleksibel — bisa union, intersection, tuple, conditional. Untuk props komponen saya pakai `type`; untuk API contract yang mungkin di-extend, `interface`."

---

## D. Pertanyaan System Design (Junior Level)

### 20. "Bagaimana desain aplikasi yang fetch data dari API dan cache?"
> "Lapisan: UI → state management (React Query) → API client → storage (AsyncStorage/SQLite). Saat fetch: cek cache dulu, kalau stale fetch dari network, tampilkan stale data sambil loading (stale-while-revalidate). Kalau offline: fallback ke cached data + tampilkan indikator offline. Untuk gambar, pakai `expo-image` yang built-in caching."

### 21. "Bagaimana kamu handle authentication di mobile?"
> "Login → server return JWT access token + refresh token. Access token simpan di memory/secure storage (`expo-secure-store` lebih aman dari AsyncStorage). Saat request, sisipkan di header `Authorization: Bearer <token>`. Kalau 401, pakai refresh token buat dapat access token baru; kalau refresh gagal, logout. Jangan simpan token di AsyncStorage kalau aplikasi sensitif."

---

## E. Pertanyaan yang Harus Kamu Tanyakan Balik

Ini nunjukin kamu serius. Pilih 2-3:
- "Teknologi apa yang tim mobile ini pakai sehari-hari? Expo atau bare RN?"
- "Bagaimana proses code review dan deployment di tim ini?"
- "Apa ekspektasi output dari intern selama 3-6 bulan?"
- "Apakah intern bisa dapat mentor yang dedicated?"
- "Setelah magang, ada peluang lanjut jadi full-time?"

---

## F. Red Flags yang Harus Kamu Hindari

- ❌ "Saya tidak tahu" tanpa usaha — lebih baik "Saya belum pernah pakai X, tapi konsepnya mirip dengan Y yang saya paham, dan saya cepat belajar."
- ❌ Mengklaim menguasai teknologi yang tidak kamu pegang (nanti kelihatan di technical test).
- ❌ Menjelek-jelekkan kampus/proyek sebelumnya.
- ❌ Tidak punya pertanyaan balik.

---

## G. Persiapan Pra-Wawancara

**H-3:** Riset perusahaan — produk mereka, tech stack (cek job desc & LinkedIn engineer mereka), berita terbaru.
**H-2:** Siapkan 2-3 cerita proyek dengan format **STAR** (Situation, Task, Action, Result).
**H-1:** Test setup video call (kamera, mic, background, koneksi). Siapkan CV + catatan.
**Hari-H:** Datang 10 menit lebih awal (online). Pakaian rapi. Bawa air. Tenang.

**Format STAR untuk proyek:**
> **S** — Motion-fits dibutuhkan navigasi kompleks dengan banyak nested tab.
> **T** — Saya harus bikin arsitektur navigasi yang scalable dan tidak bikin kode berantakan.
> **A** — Saya pakai Expo Router file-based, pisahkan route jadi grup `(tabs)`, dynamic route `[id]`, dan reusable layout.
> **R** — Hasilnya navigasi clean, deep linking otomatis, dan mudah ditambah screen baru.
