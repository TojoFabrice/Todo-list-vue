<script setup lang="ts">
import { cn } from "@/lib/utils";

const props = defineProps<{ class?: string }>();
</script>

<template>
    <td
        :class="
            cn(
                'px-4 py-2.5 align-middle [&:has([role=checkbox])]:pr-0',
                props.class
            )
        "
    >
        <slot />
    </td>
</template>
