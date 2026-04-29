# Testing & Quality Bar

## Prinsip
Test membuktikan behavior, bukan implementasi internal.
Tulis test dari sudut pandang pengguna component/function tersebut.

---

## Scope & Prioritas

| Prioritas | Target | Tool |
|---|---|---|
| **P1 — Wajib** | Component atoms & molecules (reusable) | React Testing Library |
| **P1 — Wajib** | Utils & class-based helpers | Vitest |
| **P1 — Wajib** | Custom hooks | React Testing Library + `renderHook` |
| **P2 — Disarankan** | Organisms yang punya logic kompleks | React Testing Library |
| **P2 — Disarankan** | API service functions | Vitest + mock Axios |
| **P3 — Opsional** | Templates & layout | Visual test via Storybook |

---

## Naming & Lokasi File

- File test: `NamaKomponen.test.tsx` atau `namaUtil.test.ts`
- Taruh **sebelah file yang di-test** (co-located), bukan di folder `__tests__` terpisah
- Story file Storybook: `NamaKomponen.stories.tsx` (sebelah component)

```
components/atoms/Button/
  ├── index.tsx
  ├── Button.types.ts
  ├── Button.test.tsx       ← unit test
  └── Button.stories.tsx    ← visual test / catalog
```

---

## Standar Penulisan Test

```tsx
// ✅ Test behavior, bukan implementasi
it('menampilkan loading spinner saat isLoading true', () => {
  render(<Button isLoading>Submit</Button>)
  expect(screen.getByRole('progressbar')).toBeInTheDocument()
})

// ✅ Nama test: "[kondisi] → [hasil yang diharapkan]"
it('disabled button tidak trigger onClick', () => {
  const handleClick = vi.fn()
  render(<Button disabled onClick={handleClick}>Klik</Button>)
  userEvent.click(screen.getByRole('button'))
  expect(handleClick).not.toHaveBeenCalled()
})

// ❌ Jangan test implementasi internal
it('memanggil setIsLoading(true)', () => { ... })  // tidak perlu
```

---

## Quality Bar — Sebelum Merge

- [ ] Tidak ada TypeScript error (`tsc --noEmit` clean)
- [ ] Tidak ada ESLint error/warning
- [ ] Test untuk atoms & molecules baru sudah ditulis
- [ ] Test yang sudah ada tidak ada yang patah
- [ ] Storybook story untuk component baru sudah dibuat
- [ ] Tidak ada `any`, `console.log`, atau commented-out code yang tertinggal

---

## Storybook

Setiap atom dan molecule **wajib** punya story.
Story minimal mencakup: default state, semua variant utama, edge case (loading, disabled, error).

```tsx
// Button.stories.tsx
export default { title: 'Atoms/Button', component: Button }

export const Primary = { args: { variant: 'primary', children: 'Submit' } }
export const Loading  = { args: { variant: 'primary', isLoading: true } }
export const Disabled = { args: { variant: 'primary', disabled: true } }
```
