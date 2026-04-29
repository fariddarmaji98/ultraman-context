# File & Component Placement

## Decision Tree — Di Mana Menaruh File Baru?

```
File baru → apa jenisnya?

├── Component UI
│   ├── Terkecil, standalone, tidak bergantung component lain  → atoms/
│   ├── Gabungan atoms, reusable di banyak tempat              → molecules/
│   ├── Gabungan molecules, context-specific                   → organisms/
│   │     Child-nya hanya dipakai organism ini?
│   │       ├── Ya  → nested di folder organism (tidak didaftarkan ke COMPONENTS.md)
│   │       └── Tidak → naikkan ke molecules/
│   └── Layout wrapper halaman, no business logic             → templates/
│
├── API endpoint                   → services/api/[context].ts
├── localStorage/sessionStorage    → services/storage/
├── Redux state                    → store/slices/[context].ts
├── Custom hook                    → hooks/[useNamaHook].ts
├── Class-based util               → utils/[NamaClass].ts
├── Helper function                → utils/[namaHelper].ts
├── Type API response              → types/api/[context].types.ts
├── Type dipakai >3 tempat         → types/common/shared.types.ts
├── Type khusus 1 component        → [Component]/[Component].types.ts
├── Route constant                 → constants/routes.ts
├── API endpoint constant          → constants/apiEndpoints.ts
└── Redux/Axios middleware         → middleware/[namanya].ts
```

---

## Component Registry — Wajib Cek & Update

**Sebelum membuat component baru:**
1. Cek `COMPONENTS.md` — component serupa mungkin sudah ada
2. Cek Storybook (`npm run storybook`) untuk lihat semua component visual

**Setelah membuat component baru:**
1. Daftarkan ke `COMPONENTS.md` (kecuali child component yang nested)
2. Buat story di `NamaComponent.stories.tsx`

---

## Aturan Level Component

| Kondisi | Level |
|---|---|
| Standalone, tidak butuh component lain | Atom |
| Gabungan atoms, bisa dipakai lintas context | Molecule |
| Context-specific, gabungan molecules | Organism |
| Wrapper layout halaman | Template |
| Hanya dipakai satu parent | Child — nested, tidak didaftarkan |

---

## Aturan Nested vs Flat

```
molecules/LoginButton/
  ├── index.tsx             ← Main (default export)
  ├── LoginButton.types.ts
  ├── LoggedInButton.tsx    ← Child: hanya dipakai LoginButton
  └── LoggedOutButton.tsx   ← Child: hanya dipakai LoginButton
```

Child component **tidak perlu** `index.ts` sendiri dan **tidak didaftarkan** ke `COMPONENTS.md`.
Jika child ternyata dibutuhkan component lain → naikkan levelnya ke atoms/molecules.

---

## File Wajib per Component Folder

| File | Wajib? | Keterangan |
|---|---|---|
| `index.tsx` | ✅ Wajib | Main component, default export |
| `NamaComponent.types.ts` | ✅ Wajib | Props types |
| `NamaComponent.test.tsx` | ✅ Wajib (atoms & molecules) | Unit test |
| `NamaComponent.stories.tsx` | ✅ Wajib (atoms & molecules) | Storybook |
| Child `.tsx` files | Kondisional | Hanya jika ada child yang nested |
