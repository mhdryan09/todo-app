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

```html
<script setup>
	import { ref, onMounted, computed } from "vue";
	import { useTaskStore } from "@/stores/task";
	import { storeToRefs } from "pinia";
	import { allTask } from "@/api/task"; // Misalnya Anda memiliki fungsi API untuk mengambil tugas

	const tasks = ref([]);
	const store = useTaskStore();
	const { task } = storeToRefs(store);

	onMounted(async () => {
		const { data } = await allTask();
		tasks.value = data.data;
		console.log(task.value);
	});

	const uncompletedTasks = computed(() => tasks.value.filter(task => !task.is_completed));
	const completedTasks = computed(() => tasks.value.filter(task => task.is_completed));

	const showToggleCompletedBtn = computed(() => uncompletedTasks.value.length > 0 && completedTasks.value.length > 0);
</script>
```

Memperbarui Store
Anda dapat memperbarui state store menggunakan metode $patch. Berikut adalah contoh cara memperbarui state store:

```html
<script setup>
	const store = useTaskStore();
	store.$patch({
		task: {
			name: "First task updated using $patch",
			is_completed: true,
		},
	});
</script>
```

## Penjelasan dengan Analogi

Bayangkan Anda memiliki sebuah buku catatan (store) yang berisi daftar tugas (tasks). Setiap tugas memiliki detail seperti nama dan status apakah sudah selesai atau belum (is_completed).

- Mendefinisikan Store: Seperti membuat buku catatan baru dan menuliskan format halaman pertama yang berisi daftar tugas.
- Menggunakan Store: Seperti membuka buku catatan tersebut dan melihat daftar tugas yang ada di dalamnya.
- Memperbarui Store: Seperti menulis atau mengedit tugas di dalam buku catatan tersebut.

## Rangkuman

- Mendefinisikan Store: Gunakan defineStore untuk membuat store dengan state awal.
- Menggunakan Store: Impor store dalam komponen Anda dan gunakan untuk mengelola state.
- Memperbarui Store: Gunakan metode $patch untuk memperbarui state store.

```

```
