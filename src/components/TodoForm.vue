<script setup lang="ts">
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { ref } from "vue";

const emit = defineEmits(["submit"]);
const todoName = ref("");

const onSubmit = () => {
    if (todoName.value.length < 1) {
        return;
    }

    emit("submit", todoName.value);
    todoName.value = "";
};
</script>

<template>
    <div class="mt-10 mx-5 max-w-xl sm:mx-auto">
        <form @submit.prevent="onSubmit">
            <label>
                Nom du tache
                <Input
                    class="mt-1"
                    type="text"
                    placeholder="Ex: Faire les courses"
                    v-model="todoName"
                />
            </label>

            <Button type="submit" class="w-full mt-3">
                <font-awesome-icon icon="fa-solid fa-plus" class="me-4" />
                Ajouter
            </Button>
        </form>
    </div>
</template>
