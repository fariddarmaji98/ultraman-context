# Coding Convention

## Naming

| Jenis | Convention | Contoh |
|---|---|---|
| Component file | PascalCase | `LoginButton.tsx` |
| Component folder | PascalCase | `/LoginButton/` |
| Props interface | `NamaKomponenProps` | `LoginButtonProps` |
| Class-based util file | PascalCase | `DateFormatter.ts` |
| Class di dalam file | PascalCase | `class DateFormatter` |
| Util/helper function file | camelCase | `validation.ts` |
| Service file | camelCase | `auth.ts` |
| Type definition file | `camelCase.types.ts` | `auth.types.ts` |
| Hook file | camelCase + prefix `use` | `useAuth.ts` |
| Constants file | camelCase | `routes.ts` |
| Test file | `NamaKomponen.test.tsx` | `Button.test.tsx` |

---

## TypeScript

- **Wajib** type di setiap props component
- **Wajib** type di setiap response API
- **Dilarang** `any` — gunakan `unknown` jika tipe tidak diketahui, lalu narrow
- `interface` → object shapes
- `type` → union, intersection, alias primitif

### Penempatan Type

| Kondisi | Lokasi |
|---|---|
| Khusus satu component | `ComponentName.types.ts` sebelah component |
| Response API | `/types/api/[context].types.ts` |
| Dipakai >3 tempat | `/types/common/shared.types.ts` |
| Milik class/util | Di dalam file util itu sendiri, di-export |

### Contoh
```ts
// components/atoms/Button/Button.types.ts
export interface ButtonProps {
  variant: 'primary' | 'secondary' | 'ghost'
  size: 'sm' | 'md' | 'lg'
  isLoading?: boolean
  onClick?: () => void
}

// components/atoms/Button/index.tsx
export { default as Button } from './Button'
export type { ButtonProps } from './Button.types'

// Di component lain:
import type { ButtonProps } from '@/components/atoms/Button'
```

---

## Barrel Exports

Semua folder wajib punya `index.ts`, kecuali child component yang tidak diakses dari luar.

```ts
// utils/index.ts
export { DateFormatter } from './DateFormatter'
export { QueryBuilder } from './QueryBuilder'
export * from './validation'

// Pakai:
import { DateFormatter } from '@/utils'
```

```tsx
// components/organisms/Navigation/index.tsx
import Navigation from './Navigation'
export default Navigation                          // main → default export
export { NavigationButton } from './NavigationButton'  // child → named, hanya jika perlu diakses luar
```

---

## Comments

### Wajib — JSDoc di setiap function & component
```tsx
/**
 * LoginButton — Button adaptif berdasarkan status login user.
 *
 * Tampilkan avatar jika sudah login, tombol "Login" jika belum.
 * Redirect ke redirectUrl setelah login berhasil.
 *
 * @param redirectUrl - URL tujuan setelah login (opsional)
 */
export default function LoginButton({ redirectUrl }: LoginButtonProps) { ... }
```

### Kapan Tambah Inline Comment
| Situasi | Contoh |
|---|---|
| Business logic tidak obvious | `// Diskon 10% untuk member Gold hari Selasa (promo Q1)` |
| Workaround / hack | `// HACK: force re-render, bug react-table v8.1 #1234` |
| API contract kritis | `// API expect { data, meta } — jangan ubah struktur` |
| Magic number | `// 500ms = sweet spot UX vs server load` |
| Asumsi yang bisa salah | `// NOTE: assumes user always has ≥1 address` |

### Tag Comment Standar
```ts
// NOTE:      Info penting yang perlu diperhatikan
// WARNING:   Jangan diubah tanpa testing menyeluruh
// TODO:      Perlu dikerjakan nanti
// FIXME:     Bug yang diketahui
// HACK:      Workaround sementara, perlu refactor
// IMPORTANT: Kritikal untuk flow utama
```

### Dilarang
```tsx
setIsLoading(true)   // ❌ Set loading ke true — redundant
const d = new Date() // ❌ Buat tanggal — gunakan nama descriptive

// ✅ Tidak perlu comment jika nama sudah jelas
setIsLoading(true)
const currentDate = new Date()
```

### Prinsip
- **Code → APA. Comment → MENGAPA.**
- Refactor nama variable/function lebih baik dari tambah comment
- Comment stale (tidak diupdate saat code berubah) lebih berbahaya dari tidak ada comment
