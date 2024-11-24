<template>
	<main style="min-height: 50vh; margin-top: 2rem">
		<div class="container">
			<div class="row">
				<div class="col-md-8 offset-md-2">
					<!-- Add new Task -->
					<NewTask @added="handleAddedTask" />

					<!-- List of tasks -->
					<!-- <Tasks :tasks="tasks" /> -->
					<Tasks :tasks="uncompletedTaks" />

					<!-- show toggle button -->
					<div class="text-center my-3" v-show="showToggleCompletedBtn">
						<button class="btn btn-sm btn-secondary" @click="$event => (showCompletedTasks = !showCompletedTasks)">
							<span v-if="!showCompletedTasks">Show Completed</span>
							<span v-else>Hide Completed</span>
						</button>
					</div>

					<!-- list of completed task -->
					<Tasks :tasks="completedTaks" :show="completedTaskIsVisible && showCompletedTasks" />
				</div>
			</div>
		</div>
	</main>
</template>

<script setup>
import Tasks from "@/components/tasks/Tasks.vue";
import NewTask from "@/components/tasks/NewTask.vue";
import { allTask, createTask } from "../http/task-api";
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
</script>
