# Specific Commands

Perintah CLI yang sering dipakai dalam project ini.

---

## Development

```bash
npm run dev          # Start dev server
npm run build        # Production build
npm run preview      # Preview production build lokal
```

---

## Type & Lint

```bash
npx tsc --noEmit                        # Type check tanpa emit file
npx eslint src/ --ext .ts,.tsx          # Lint semua file TS/TSX
npx eslint src/ --ext .ts,.tsx --fix    # Auto-fix lint issues
```

---

## Testing

```bash
npm run test                  # Jalankan semua test (watch mode)
npm run test -- --run         # Jalankan semua test sekali (CI mode)
npm run test -- NamaFile      # Jalankan test spesifik
npm run test -- --coverage    # Jalankan test + coverage report
```

---

## Storybook

```bash
npm run storybook             # Start Storybook dev server (port 6006)
npm run build-storybook       # Build Storybook static
```

---

## Codebase Search

```bash
# Cari semua penggunaan sebuah component
grep -r "NamaComponent" src/

# Cari semua penggunaan sebuah Redux slice
grep -r "namaSlice" src/

# Cari semua TODO/FIXME/HACK di codebase
grep -rn "TODO:\|FIXME:\|HACK:" src/

# Cari semua penggunaan tipe tertentu
grep -rn "NamaTipe" src/
```

---

## Barrel Export — Verifikasi

```bash
# Pastikan semua export dari index.ts bisa di-resolve
npx tsc --noEmit --moduleResolution bundler
```

---

## Utility Tambahan

```bash
# Cek ukuran bundle setelah build
npm run build -- --analyze     # Jika plugin bundle analyzer terpasang

# Format semua file (jika Prettier dikonfigurasi)
npx prettier --write src/
```

---

## Catatan

- Semua command diasumsikan dijalankan dari root project
- Jika ada script custom di `package.json` yang ditambahkan selama project, dokumentasikan di sini
