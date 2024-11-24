<template>
	<div class="relative">
		<input type="text" class="form-control form-control-lg padding-right-lg" placeholder="+ Add new task. Press enter to save." @keydown.enter="addNewTask" />
	</div>
</template>

<script setup>
import { reactive } from "vue";

const newTask = reactive({
	name: "",
	is_completed: false,
});

const emit = defineEmits(["added"]);

const addNewTask = event => {
	// cek apakah inputan tidak kosong
	if (event.target.value.trim()) {
		newTask.name = event.target.value; // ambil nilai input
		event.target.value = ""; // reset nilai input
		emit("added", newTask); // kirim ke parent dan mengirimkan newTask sebgai payload
	}
};
</script>
