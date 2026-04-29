# Tech Stack

## Library & Tooling

| Kategori | Library | Catatan |
|---|---|---|
| Framework | Next.js + TypeScript | App Router |
| Styling | Tailwind CSS | Utility only — tidak ada custom CSS kecuali `globals.css` |
| UI Components | Reactbits | https://reactbits.dev/ |
| Build Tool | Vite | |
| State Management | Redux Toolkit | Slice per context |
| Form | React Hook Form | Uncontrolled input — default |
| HTTP Client | Axios | Instance terpusat di `services/api/config.ts` |
| Component Catalog | Storybook | `npm run storybook` |

## Batasan Keras
- **Dilarang install library baru tanpa diskusi.** Catat keputusan di `3_architecture.md` (ADR section Library).
- **Dilarang custom CSS** di luar `globals.css`. Semua styling via Tailwind utility class.
- **Dilarang `any`** di TypeScript. Selalu definisikan type eksplisit.
