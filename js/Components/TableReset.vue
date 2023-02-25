<template>
  <button
    ref="button"
    type="button"
    dusk="reset-table"
    class="d-flex align-items-center btn btn-light mx-1"
    aria-haspopup="true"
    @click.prevent="onClick"
  >
    <i class="bx bx-x font-size-16 align-middle me-2"></i>Reset</button>
</template>

<script setup>
defineProps({
    onClick: {
        type: Function,
        required: true
    }
});
</script>
