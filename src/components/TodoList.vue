<script setup lang="ts">
import {
    Table,
    TableBody,
    TableCaption,
    TableCell,
    TableHead,
    TableHeader,
    TableRow,
} from "@/components/ui/table";
import { Button } from "@/components/ui/button";
import TodoEdit from "@/components/TodoEdit.vue";
import { Task } from "@/types";
import { computed } from "vue";

interface Props {
    tasks: Task[];
}
const { tasks } = defineProps<Props>();
const emit = defineEmits(["delete", "task-edited", "task-checked"]);
const incompleteTasksCount = computed(() =>
    tasks.reduce((acc, curr) => (!curr.isAccomplished ? acc + 1 : acc), 0)
);

const onTaskEdited = ({
    index,
    newName,
}: {
    index: number;
    newName: string;
}) => {
    emit("task-edited", { index, newName });
};
</script>

<template>
    <div class="mx-5 mt-10 max-w-4xl lg:mx-auto">
        <h4 class="my-4 text-lg font-semibold">({{ tasks.length }}) Taches</h4>
        <Table class="border rounded-md">
            <TableCaption>
                ({{ incompleteTasksCount }}) taches incomplètes</TableCaption
            >
            <TableHeader>
                <TableRow>
                    <TableHead> </TableHead>
                    <TableHead>Nom du tache</TableHead>
                    <TableHead class="text-right"> </TableHead>
                </TableRow>
            </TableHeader>

            <TableBody>
                <TableRow v-if="tasks.length === 0">
                    <TableCell></TableCell>
                    <TableCell class="text-center py-5">Pas de tache.</TableCell>
                    <TableCell></TableCell>
                </TableRow>
                <TableRow v-for="(task, i) in tasks" :key="i">
                    <TableCell>
                        <input
                            class="cursor-pointer"
                            type="checkbox"
                            :checked="task.isAccomplished"
                            @change="emit('task-checked', i)"
                        />
                    </TableCell>
                    <TableCell class="w-full">
                        <span
                            class="line-through text-gray-500"
                            v-if="task.isAccomplished"
                            >{{ task.name }}</span
                        >
                        <span v-else>{{ task.name }}</span>
                    </TableCell>
                    <TableCell class="px-0 pr-2">
                        <Button variant="outline" v-show="!task.isAccomplished">
                            <TodoEdit
                                :task-index="i"
                                :task-name="task.name"
                                @task-edited="onTaskEdited"
                            >
                                <span class="text-lg">
                                    <font-awesome-icon
                                        icon="fa-solid fa-pen-to-square"
                                    />
                                </span>
                            </TodoEdit>
                        </Button>
                    </TableCell>
                    <TableCell class="ps-0">
                        <Button variant="outline" @click="emit('delete', i)">
                            <span class="text-lg">
                                <font-awesome-icon icon="fa-solid fa-trash" />
                            </span>
                        </Button>
                    </TableCell>
                </TableRow>
            </TableBody>
        </Table>
    </div>
</template>
