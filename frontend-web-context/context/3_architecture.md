# Architecture

## Struktur Folder

```
src/
├── components/
│   ├── atoms/            # Terkecil, standalone, reusable penuh
│   ├── molecules/        # Kombinasi atoms, masih reusable
│   ├── organisms/        # Kombinasi molecules, grouped by context
│   └── templates/        # Layout wrapper, no business logic
│
├── services/
│   ├── api/
│   │   ├── config.ts     # Axios instance, base URL, headers, interceptors
│   │   ├── auth.ts       # Endpoint per context
│   │   ├── user.ts
│   │   └── index.ts      # Barrel export
│   └── storage/          # Wrapper localStorage/sessionStorage
│       └── index.ts
│
├── store/                # Redux Toolkit
│   ├── slices/
│   │   ├── auth.ts
│   │   ├── user.ts
│   │   └── notification.ts  # Global message/toast state
│   ├── hooks.ts          # useAppDispatch, useAppSelector
│   └── index.ts          # Store config + barrel export
│
├── hooks/                # Custom React hooks
│   └── index.ts          # Barrel export
│
├── utils/
│   ├── DateFormatter.ts  # Class-based util
│   ├── QueryBuilder.ts   # Class-based util
│   ├── validation.ts     # Helper functions
│   └── index.ts          # Barrel export
│
├── types/
│   ├── api/              # Type response API per context
│   ├── common/
│   │   └── shared.types.ts  # Type dipakai >3 tempat
│   └── index.ts
│
├── constants/
│   ├── routes.ts
│   ├── apiEndpoints.ts
│   └── index.ts
│
└── middleware/
    └── index.ts
```

---

## Alur Data

### GET — SSR Mode
```
Server Component
  → Axios request
  → props ke Client Component
  → Client Component → Redux
  → Child components baca dari Redux
```

### POST / PUT / DELETE — CSR Mode
```
User action
  → Axios request
  → Redux: simpan { status, message, data? }
  → Toast/Message component reaktif ke Redux state
```

### Distribusi Data Antar Component
| Kondisi | Cara |
|---|---|
| Parent → direct child | Props |
| Dipakai >1 component | Redux |

---

## Canonical Patterns

### State Management
- Global/shared state → **Redux Toolkit**, slice per context
- Local UI state (toggle, loading lokal, input) → **`useState`**
- Jangan reflex menaruh semua state ke Redux

### Form
- **React Hook Form, uncontrolled input** — default wajib
- Ganti ke controlled hanya jika ada alasan kuat → dokumentasikan di ADR

### API Call — Pola Standar
```ts
const fetchUser = async (id: string): Promise<UserResponse> => {
  try {
    const response = await api.get<UserResponse>(`/users/${id}`)
    return response.data
  } catch (error) {
    throw error  // re-throw, handle di caller
  }
}
```
- `try` → return data
- `catch` → throw atau return error — **konsisten dalam satu project**
- Redux simpan: `{ status, message, data? }`

---

## Architecture Decision Records (ADR)

> Catat semua keputusan teknis signifikan di sini.
> **Wajib dicatat:** pemilihan library, penyimpangan dari pola default, workaround, trade-off sadar.
> **Tidak perlu dicatat:** keputusan yang sudah jelas dari dokumen ini, perubahan styling minor.

### Template ADR

```
### [ADR-XXX] Judul

- **Tanggal**: YYYY-MM-DD
- **Status**: Proposed | Accepted | Deprecated | Superseded by ADR-XXX
- **Area**: Architecture | Component | Data Flow | Library | Pattern | Other

#### Konteks
Masalah atau constraint yang memaksa keputusan ini.

#### Opsi yang Dipertimbangkan
| Opsi | Kelebihan | Kekurangan |
|---|---|---|
| A | ... | ... |
| B | ... | ... |

#### Keputusan
**Dipilih: Opsi X** — alasan singkat.

#### Konsekuensi
- ✅ Yang jadi lebih baik
- ⚠️ Trade-off yang diterima
- 🔧 Follow-up (jika ada)
```

---

### [ADR-001] Atomic Design sebagai Struktur Component

- **Tanggal**: YYYY-MM-DD
- **Status**: Accepted
- **Area**: Architecture

#### Konteks
Butuh struktur component scalable, hierarki jelas, mencegah duplikasi.

#### Opsi yang Dipertimbangkan
| Opsi | Kelebihan | Kekurangan |
|---|---|---|
| Flat `/components` | Sederhana | Tidak scalable, rawan duplikasi |
| Feature-based | Modular per fitur | Susah share lintas fitur |
| Atomic Design | Hierarki jelas, reusability terstruktur | Perlu keputusan sadar soal level |

#### Keputusan
**Dipilih: Atomic Design** — hierarki jelas, memudahkan deteksi duplikasi via `COMPONENTS.md`.

#### Konsekuensi
- ✅ Mudah temukan component sebelum buat baru
- ⚠️ Perlu keputusan sadar saat tentukan level component
- 🔧 Selalu cek `6_file_and_component_placement.md` dan `COMPONENTS.md`

---

### [ADR-002] SSR untuk GET, CSR untuk Mutasi

- **Tanggal**: YYYY-MM-DD
- **Status**: Accepted
- **Area**: Architecture

#### Opsi yang Dipertimbangkan
| Opsi | Kelebihan | Kekurangan |
|---|---|---|
| Semua CSR | Satu pola | SEO buruk, loading flash |
| Semua SSR | SEO optimal | Mutasi di server kompleks |
| Hybrid GET→SSR, mutasi→CSR | SEO + interaktivitas | Butuh disiplin |

#### Keputusan
**Dipilih: Hybrid** — GET di server (SEO, no flash). Mutasi di client (feedback real-time).

#### Konsekuensi
- ✅ SEO friendly, UX responsif
- ⚠️ Jangan fetch data di CSR kecuali ada alasan — catat di ADR baru jika menyimpang

---

### [ADR-003] Redux Hanya untuk Shared State

- **Tanggal**: YYYY-MM-DD
- **Status**: Accepted
- **Area**: Data Flow

#### Keputusan
**Dipilih: Hybrid** — `useState` untuk UI state lokal, Redux untuk state dipakai >1 component.

#### Konsekuensi
- ✅ Minim boilerplate, component mudah di-isolasi
- ⚠️ Jika state lokal perlu di-share → refactor ke Redux, catat jika butuh ADR

---

## ADR Index

| ID | Judul | Status | Area | Tanggal |
|---|---|---|---|---|
| ADR-001 | Atomic Design sebagai Struktur Component | Accepted | Architecture | YYYY-MM-DD |
| ADR-002 | SSR untuk GET, CSR untuk Mutasi | Accepted | Architecture | YYYY-MM-DD |
| ADR-003 | Redux Hanya untuk Shared State | Accepted | Data Flow | YYYY-MM-DD |
