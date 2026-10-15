# UI Editor Exploration

Eksplorasi komponen **Editor** dari [Nuxt UI](https://ui.nuxt.com/docs/components/editor), sebuah rich text editor berbasis [TipTap](https://tiptap.dev/), di atas [Nuxt 4](https://nuxt.com/).

Project ini mencoba fitur-fitur editor: markdown, perataan teks, highlight, upload gambar lokal, slash command (`/`), drag-and-drop blok, dan custom node.

## Fitur

- **Markdown content**: `v-model` berupa string markdown, dengan panel output live + tombol copy.
- **Toolbar lengkap**: undo/redo, heading, list, blockquote, code block, divider, format teks (bold, italic, underline, strike, code), highlight, text align, link, dan gambar.
- **Text align & highlight**: lewat extension TipTap `@tiptap/extension-text-align` dan `@tiptap/extension-highlight`.
- **Upload gambar lokal**: custom TipTap node (`imageUpload`) yang di-render oleh komponen Vue `<UFileUpload>`, lalu diganti dengan node `Image` biasa setelah file dipilih.
- **Slash command menu**: muncul saat mengetik `/` pada baris kosong.
- **Drag handle**: mengurutkan blok dengan drag-and-drop.

## Tech Stack

- [Nuxt 4](https://nuxt.com/)
- [Nuxt UI 4](https://ui.nuxt.com/)
- [Tailwind CSS 4](https://tailwindcss.com/)
- [TipTap 3](https://tiptap.dev/) (`@tiptap/core`, `@tiptap/vue-3`, extension text-align & highlight)
- TypeScript 5
- Vue 3

## Struktur Folder

```
ui-editor-exploration/
├── app/                              # Kode aplikasi Nuxt (srcDir)
│   ├── app.vue                       # Root komponen; membungkus <UApp> + <NuxtPage>
│   ├── assets/
│   │   └── css/
│   │       └── main.css              # Entry CSS global: impor Tailwind + Nuxt UI
│   ├── components/
│   │   ├── EditorImageUploadExtension.ts   # Custom TipTap node "imageUpload"
│   │   └── EditorImageUploadNode.vue       # Node view Vue untuk node di atas
│   └── pages/
│       └── index.vue                 # Halaman utama: demo/pembahasan <UEditor>
├── public/                           # Aset statis (favicon.ico, robots.txt)
├── nuxt.config.ts                    # Konfigurasi Nuxt (modul, CSS, devServer, Vite)
├── tsconfig.json                     # TS config (mereferensikan .nuxt/tsconfig.*)
├── package.json                      # Dependency & script npm
└── README.md
```

### Penjelasan singkat file penting

- **`app/pages/index.vue`**: inti eksplorasi. Mendefinisikan `content` (v-model markdown), `extensions` TipTap, `customHandlers` (`highlight` & `imageUpload`), `toolbarItems`, dan `suggestionItems` (menu slash command).
- **`app/components/EditorImageUploadExtension.ts`**: mendefinisikan custom node TipTap `imageUpload` (tipe atom + draggable) beserta perintah `insertImageUpload()`, dan merender tampilannya lewat komponen Vue.
- **`app/components/EditorImageUploadNode.vue`**: node view-nya: menampilkan `<UFileUpload>`, membaca file jadi data URL via `FileReader`, lalu mengganti node dengan gambar (`setImage`).
- **`nuxt.config.ts`**: mendaftarkan `@nuxt/ui`, memuat CSS global, dan menyetel `vite.optimizeDeps` untuk ProseMirror (lihat catatan di bawah).

## Menjalankan di Lokal

### Prasyarat

- **Node.js** versi 20 atau lebih baru (disarankan LTS terbaru)
- npm (atau pnpm / yarn / bun)

### Langkah

```bash
# 1. Install dependency
npm install

# 2. Jalankan dev server
npm run dev

# 3. Buka http://localhost:3000
```

### Script yang tersedia

| Perintah            | Fungsi                                        |
| ------------------- | --------------------------------------------- |
| `npm run dev`       | Menjalankan dev server (hot reload)           |
| `npm run build`     | Build untuk produksi                         |
| `npm run generate`  | Generate static site                          |
| `npm run preview`   | Menjalankan hasil build secara lokal          |
| `npm run postinstall` | Menyiapkan Nuxt (`nuxt prepare`) setelah install |

Menggunakan pnpm / yarn / bun? Ganti `npm <script>` menjadi `pnpm <script>` / `yarn <script>` / `bun run <script>`.

## Catatan Konfigurasi

**`vite.optimizeDeps.include` untuk ProseMirror**: memastikan ProseMirror di-pre-bundle sebagai satu instance, mencegah error `"Adding different instances of a keyed plugin"`.

## Referensi

- [Dokumentasi komponen Editor Nuxt UI](https://ui.nuxt.com/docs/components/editor)
- [Dokumentasi Nuxt](https://nuxt.com/docs/getting-started/introduction)
- [Dokumentasi TipTap](https://tiptap.dev/docs)
