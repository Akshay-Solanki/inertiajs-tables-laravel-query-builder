<template>
  <div class="relative search-box w-100 mx-1">
    <!-- block w-full pl-9 text-sm rounded-md shadow-sm focus:ring-indigo-500 focus:border-gray-500 border-gray-300 focus:ring-2 dark:bg-gray-700 dark:text-gray-100 dark:border-gray-500 -->
    <div class="position-relative">
      <input type="text" class="form-control rounded "
      :placeholder="label"
      :value="value"
      @input="onChange($event.target.value)"
      >
      <i class="mdi mdi-magnify search-icon"></i>
    </div>
  </div>
</template>

<script setup>
defineProps({
    label: {
        type: String,
        default: "Search...",
        required: false,
    },

    value: {
        type: String,
        default: "",
        required: false,
    },

    onChange: {
        type: Function,
        required: true,
    },
});
</script>

