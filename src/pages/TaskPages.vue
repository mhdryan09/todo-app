<template>
	<main style="min-height: 50vh; margin-top: 2rem">
		<div class="container">
			<div class="row">
				<div class="col-md-8 offset-md-2">
					<!-- Add new Task -->
					<NewTask @added="handleAddedTask" />

					<!-- List of tasks -->
					<!-- <Tasks :tasks="tasks" /> -->
					<Tasks :tasks="uncompletedTaks" @updated="handleUpdatedTask" @completed="handleCompletedTask" @removed="handleRemovedTask" />

					<!-- show toggle button -->
					<div class="text-center my-3" v-show="showToggleCompletedBtn">
						<button class="btn btn-sm btn-secondary" @click="$event => (showCompletedTasks = !showCompletedTasks)">
							<span v-if="!showCompletedTasks">Show Completed</span>
							<span v-else>Hide Completed</span>
						</button>
					</div>

					<!-- list of completed task -->
					<Tasks :tasks="completedTaks" :show="completedTaskIsVisible && showCompletedTasks" @updated="handleUpdatedTask" @completed="handleCompletedTask" @removed="handleRemovedTask" />
				</div>
			</div>
		</div>
	</main>
</template>

<script setup>
import Tasks from "@/components/tasks/Tasks.vue";
import NewTask from "@/components/tasks/NewTask.vue";
import { allTask, createTask, updateTask, completeTask, removeTask } from "../http/task-api";
import { onMounted, ref, computed } from "vue";

const tasks = ref([]);

onMounted(async () => {
	const { data } = await allTask();
	tasks.value = data.data;
});

const uncompletedTaks = computed(() => tasks.value.filter(task => !task.is_completed));
const completedTaks = computed(() => tasks.value.filter(task => task.is_completed));

// cek apakah ada task yang belum selesai dan ada task yang sudah selesai
const showToggleCompletedBtn = computed(() => uncompletedTaks.value.length > 0 && completedTaks.value.length > 0);

const completedTaskIsVisible = computed(() => uncompletedTaks.value.length === 0 || completedTaks.value.length > 0);
const showCompletedTasks = ref(false);

const handleAddedTask = async newTask => {
	const { data: createdTask } = await createTask(newTask);
	tasks.value.unshift(createdTask.data); // menambahkan task baru ke awal array tasks
};

const handleUpdatedTask = async task => {
	const { data: updatedTask } = await updateTask(task.id, {
		name: task.name,
	}); // mengupdate task berdasarkan id dan mengirimkan task sebgai payload

	const currentTask = task.value.find(item => item.id === task.id); // mencari task berdasarkan id yang diupdate
	currentTask.name = updatedTask.data.name; // mengganti nama task yang diupdate
};

const handleCompletedTask = async task => {
	const { data: updatedTask } = await completeTask(task.id, {
		is_completed: task.is_completed,
	});

	const currentTask = task.value.find(item => item.id === task.id); // mencari task berdasarkan id yang diupdate
	currentTask.is_completed = updatedTask.data.is_completed; // mengganti is_completed task yang diupdate
};

const handleRemovedTask = async task => {
	await removeTask(task.id);
	const index = tasks.value.findIndex(item => item.id === task.id); // mencari task berdasarkan id yang dihapus dan menghapus task tersebut
	tasks.value.splice(index, 1); // menghapus task dari array tasks
};
</script>
