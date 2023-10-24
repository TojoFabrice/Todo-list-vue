<script setup lang="ts">
import { ref } from "vue";
import Header from "@/components/Header.vue";
import TodoForm from "@/components/TodoForm.vue";
import TodoList from "@/components/TodoList.vue";
import { Task } from "@/types";

const tasks = ref<Task[]>([]);

const onTaskCreate = (newName: string) => {
    tasks.value.push({ name: newName, isAccomplished: false });
};

const onTaskDelete = (index: number) => {
    tasks.value.splice(index, 1);
};

const onTaskEdited = ({
    index,
    newName,
}: {
    index: number;
    newName: string;
}) => {
    tasks.value[index].name = newName;
};

const onTaskChecked = (index: number) => {
    tasks.value[index].isAccomplished = !tasks.value[index].isAccomplished;
};
</script>

<template>
    <Header />
    <TodoForm @submit="onTaskCreate" />
    <TodoList
        :tasks="tasks"
        @task-checked="onTaskChecked"
        @delete="onTaskDelete"
        @task-edited="onTaskEdited"
    />
</template>

<style scoped></style>
