<script setup lang="ts">
import {
    Dialog,
    DialogContent,
    DialogFooter,
    DialogHeader,
    DialogTitle,
    DialogTrigger,
} from "@/components/ui/dialog";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { ref } from "vue";

interface Props {
    taskIndex: number;
    taskName: string;
}
const props = defineProps<Props>();
const emit = defineEmits(["taskEdited"]);

const isOpen = ref(false);
const todoName = ref(props.taskName);

const onSubmit = () => {
    if (todoName.value.length < 1) {
        return;
    }

    emit("taskEdited", {
        index: props.taskIndex,
        newName: todoName.value,
    });
    todoName.value = "";
    isOpen.value = false;
};
</script>

<template>
    <Dialog v-model:open="isOpen">
        <DialogTrigger>
            <slot />
        </DialogTrigger>
        <DialogContent>
            <DialogHeader>
                <DialogTitle>Modifier</DialogTitle>
            </DialogHeader>
            <form @submit.prevent="onSubmit">
                <Input
                    type="text"
                    v-model="todoName"
                    :value="props.taskName"
                    :default-value="props.taskName"
                />
                <DialogFooter class="mt-4">
                    <Button
                        variant="outline"
                        @click="isOpen = false"
                        type="button"
                    >
                        Annuler
                    </Button>
                    <Button type="submit"> Sauvegarder </Button>
                </DialogFooter>
            </form>
        </DialogContent>
    </Dialog>
</template>
