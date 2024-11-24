<template>
	<main style="min-height: 50vh; margin-top: 2rem">
		<div class="container">
			<div class="row">
				<div class="col-md-8 offset-md-2">
					<!-- Add new Task -->
					<div class="relative">
						<input type="text" class="form-control form-control-lg padding-right-lg" placeholder="+ Add new task. Press enter to save." />
					</div>
					<!-- List of tasks -->
					<!-- <Tasks :tasks="tasks" /> -->
					<Tasks :tasks="uncompletedTaks" />

					<!-- show toggle button -->

					<!-- list of completed task -->
					<Tasks :tasks="completedTaks" />
				</div>
			</div>
		</div>
	</main>
</template>

<script setup>
import Tasks from "@/components/tasks/Tasks.vue";
import { allTask } from "../http/task-api";
import { onMounted, ref, computed } from "vue";

const tasks = ref([]);

onMounted(async () => {
	const { data } = await allTask();
	tasks.value = data.data;
});

const uncompletedTaks = computed(() => tasks.value.filter(task => !task.is_completed));
const completedTaks = computed(() => tasks.value.filter(task => task.is_completed));
</script>
