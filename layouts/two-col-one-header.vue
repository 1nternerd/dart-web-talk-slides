<script setup lang="ts">
const props = defineProps({
    class: {
        type: String,
    },
    layoutClass: {
        type: String,
    },
})
</script>

<template>
    <div class="slidev-layout w-full h-full" :class="props.layoutClass">
        <div class="w-full">
            <slot name="header" />
        </div>
        <div class="h-full w-full grid grid-cols-2">
            <div class="col-left" :class="props.class">
                <slot />
            </div>
            <div class="col-right" :class="props.class">
                <slot name="right" />
            </div>
        </div>
    </div>
</template>