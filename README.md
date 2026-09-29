# Technical Assignment Vue

Technical Assignment — **Component-Based UI with Vue.js 3 (Composition API)**.

Project ini melanjutkan `dashboard-vue` dari Session 5 dan menambahkan dua fitur utama sesuai assignment.

## Assignment Checklist

### Problem 1 — Detail Toggle on `UserCard.vue`

- `detailTerbuka` dibuat sebagai **local state `ref` di `UserCard.vue`**.
- Membuka satu card tidak memengaruhi card lain.
- Data detail berasal dari props: `phone`, `company`, dan `city`.
- Tombol berubah antara **Lihat Detail** dan **Sembunyikan**.

### Problem 2 — Sort Control (Chained Computed)

- `arahUrutan` adalah `ref` dengan nilai `asc` / `desc`.
- `penggunaTerurut` adalah **computed baru yang mengambil data dari `penggunaTersaring`**, bukan langsung dari `users`.
- Sorting memakai copy array: `[...penggunaTersaring.value]` sebelum `.sort()`.
- `v-for` memakai `penggunaTerurut`.
- Tombol A-Z / Z-A memiliki visual active state yang berbeda.

## Existing Session 5 Features

- Fetch pengguna dari JSONPlaceholder.
- Search tetap menggunakan `queryPencarian` dan `penggunaTersaring`.
- Search mendukung **nama atau username**.
- Loading, error, dan empty state tetap tersedia.

## UI Theme

UI dibuat ulang dengan gaya editorial-tech yang terinspirasi bahasa visual Work Louder: tipografi tegas, panel modular, border kontras, aksen warna cerah, dan tombol tactile. Implementasi ini dibuat khusus untuk project assignment dan tidak menyalin source code atau aset Work Louder.

Pada setiap user card sekarang informasi dipisahkan dengan jelas:

- nama pengguna;
- `@username`;
- email;
- tombol **Lihat Detail** di sisi kanan;
- detail phone/company/city muncul di panel terpisah ketika dibuka.

## Menjalankan Project

```bash
npm install
npm run dev
```

Build production:

```bash
npm run build
```

## Struktur Utama

```text
src/
├── components/
│   └── UserCard.vue
├── App.vue
├── main.js
└── style.css
```
