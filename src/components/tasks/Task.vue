<template>
	<li class="list-group-item py-3">
		<div class="d-flex justify-content-start align-items-center">
			<input class="form-check-input mt-0" :class="completedClass" type="checkbox" :checked="task.is_completed" @change="markTaskAsCompleted" />
			<div class="ms-2 flex-grow-1" :class="completedClass" title="Double click the text to edit or remove" @dblclick="$event => (isEdit = true)">
				<div class="relative" v-if="isEdit">
					<input class="editable-task" type="text" @keyup.esc="undo" v-focus @keyup.enter="updateTask" v-model="editingTask" />
				</div>
				<span v-else>{{ task.name }}</span>
			</div>
		</div>

		<TaskActions @edit="$event => (isEdit = true)" v-show="!isEdit" @remove="removeTask" />
	</li>
</template>

<script setup>
import { computed, ref } from "vue";
import TaskActions from "./TaskActions.vue";

const props = defineProps({
	task: Object,
});

const isEdit = ref(false);
const editingTask = ref(props.task.name);

const emit = defineEmits(["updated", "completed", "removed"]);

const completedClass = computed(() => (props.task.is_completed ? "completed" : ""));

const vFocus = {
	mounted: el => el.focus(),
};
// fungsi directive diatas untuk mengaktifkan focus pada input

const updateTask = event => {
	const updatedTask = { ...props.task, name: event.target.value }; // buat objek baru dari props.task dan tambahkan properti name dengan nilai input yang diubah
	isEdit.value = false;
	emit("updated", updatedTask); // kirim ke parent dan mengirimkan updatedTask sebgai payload
};

const undo = () => {
	isEdit.value = false; // jika input diubah, maka isEdit akan diubah menjadi false
	editingTask.value = props.task.name; // mengembalikan nilai props.task.name ke editingTask
};

const markTaskAsCompleted = event => {
	const updatedTask = { ...props.task, is_completed: !props.task.is_completed }; // buat objek baru dari props.task dan tambahkan properti is_completed dengan nilai input yang diubah
	emit("completed", updatedTask);
};

const removeTask = () => {
	if (confirm("Are you sure?")) {
		emit("removed", props.task);
	}
};
</script>
