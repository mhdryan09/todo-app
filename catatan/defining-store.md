# Defining and Using Store in Vue dengan Pinia

## Pendahuluan

Dokumentasi ini memberikan rangkuman tentang cara mendefinisikan dan menggunakan store dalam aplikasi Vue.js menggunakan Pinia. Pinia adalah library manajemen state untuk Vue.js yang memungkinkan Anda mengelola state aplikasi secara terpusat.

## Mendefinisikan Store

Untuk mendefinisikan store di Pinia, Anda perlu membuat file baru (misalnya, `task.js`) dan menggunakan fungsi `defineStore` dari Pinia. Berikut adalah contoh cara mendefinisikan store untuk mengelola tugas:

```js
import { defineStore } from "pinia";

export const useTaskStore = defineStore("taskStore", {
	state: () => ({
		tasks: [],
		task: {
			id: 1,
			name: "First Task",
			is_completed: false,
		},
	}),
});
```

## Menggunakan Store

Untuk menggunakan store dalam komponen Vue Anda, Anda perlu mengimpor store dan menggunakannya di dalam komponen Anda. Berikut adalah contoh cara menggunakan store dalam komponen Vue:

## Mengimpor dan Menggunakan Store

Memperbarui Store
Anda dapat memperbarui state store menggunakan metode $patch. Berikut adalah contoh cara memperbarui state store:

## Penjelasan dengan Analogi

Bayangkan Anda memiliki sebuah buku catatan (store) yang berisi daftar tugas (tasks). Setiap tugas memiliki detail seperti nama dan status apakah sudah selesai atau belum (is_completed).

- Mendefinisikan Store: Seperti membuat buku catatan baru dan menuliskan format halaman pertama yang berisi daftar tugas.
- Menggunakan Store: Seperti membuka buku catatan tersebut dan melihat daftar tugas yang ada di dalamnya.
- Memperbarui Store: Seperti menulis atau mengedit tugas di dalam buku catatan tersebut.

## Rangkuman

- Mendefinisikan Store: Gunakan defineStore untuk membuat store dengan state awal.
- Menggunakan Store: Impor store dalam komponen Anda dan gunakan untuk mengelola state.
- Memperbarui Store: Gunakan metode $patch untuk memperbarui state store.
