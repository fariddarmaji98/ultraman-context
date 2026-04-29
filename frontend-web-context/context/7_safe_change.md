# Safe Change

Checklist ini mencegah perubahan yang merusak hal lain tanpa disadari.
Jalankan secara berurutan sebelum setiap commit.

---

## Checklist — Setiap Commit

### 1. Type Check
```bash
npx tsc --noEmit
```
Tidak boleh ada error. Warning boleh — tapi catat jika perlu ditangani.

### 2. Lint
```bash
npx eslint src/ --ext .ts,.tsx
```
Tidak boleh ada error. Warning perlu dievaluasi.

### 3. Test
```bash
npm run test
```
Semua test harus pass. Tidak boleh ada test yang di-skip tanpa alasan.

### 4. Build Check (sebelum PR/deploy)
```bash
npm run build
```
Build harus sukses. Periksa bundle size jika ada perubahan dependency.

---

## Checklist — Membuat Component Baru

- [ ] Cek `COMPONENTS.md` — pastikan tidak duplikasi
- [ ] Tentukan level yang tepat via decision tree di `6_file_and_component_placement.md`
- [ ] Buat `index.tsx`, `*.types.ts`, `*.test.tsx`, `*.stories.tsx`
- [ ] Daftarkan ke `COMPONENTS.md`
- [ ] Export via barrel `index.ts` folder parent

## Checklist — Mengubah Component yang Sudah Ada

- [ ] Cari semua tempat component dipakai: `grep -r "NamaComponent" src/`
- [ ] Jika mengubah props interface → update semua caller
- [ ] Jika mengubah behavior → update test yang relevan
- [ ] Jika mengubah export name → update semua import

## Checklist — Mengubah Redux Slice

- [ ] Cari semua selector yang bergantung: `grep -r "sliceName" src/`
- [ ] Pastikan shape state tidak breaking jika ada persisted state
- [ ] Update type di slice jika ada perubahan struktur data

## Checklist — Mengubah API Service

- [ ] Konfirmasi contract dengan backend sebelum ubah endpoint/payload
- [ ] Update type di `/types/api/[context].types.ts`
- [ ] Cek semua tempat service function dipanggil

## Checklist — Install Library Baru

- [ ] Diskusi dulu — lihat `2_tech_stack.md`
- [ ] Catat keputusan di ADR (`3_architecture.md`, section Library)
- [ ] Tambahkan ke tabel di `2_tech_stack.md`

---

## Penyimpangan dari Pola Default

Jika perubahan **menyimpang dari pola di dokumen ini** (misal: pakai controlled input, fetch di CSR, dll):

1. Tambahkan comment `// NOTE:` atau `// HACK:` di kode dengan alasan singkat
2. Catat sebagai ADR baru di `3_architecture.md`
3. Update checklist atau aturan jika penyimpangan ini akan jadi pola baru

---

## Yang Tidak Boleh Ada di Commit

| Larangan | Alasan |
|---|---|
| `any` di TypeScript | Menghilangkan type safety |
| `console.log` aktif | Debug artifact, noise di production |
| Commented-out code | Gunakan git history, bukan komentar |
| Import path absolut tanpa alias `@/` | Tidak konsisten, susah refactor |
| Library baru tanpa ADR | Tidak terdokumentasi |
| Component baru tanpa entry di `COMPONENTS.md` | Rawan duplikasi |
