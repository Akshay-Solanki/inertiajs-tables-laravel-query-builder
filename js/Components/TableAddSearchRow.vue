<template>
  <ButtonWithDropdown
    ref="dropdown"
    dusk="add-search-row-dropdown"
    :disabled="!hasSearchInputsWithoutValue"
    class="w-auto"
  >
    <template #button>
      <i class="mdi mdi-magnify font-size-16" />
    </template>

    <div
      role="menu"
      aria-orientation="horizontal"
      aria-labelledby="add-search-input-menu"
      class="min-w-max"
    >
    <!-- text-left w-full px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 dark:text-gray-100 dark:hover:bg-gray-600 dark:hover:text-gray-100 rounded-md -->
      <b-dropdown-item
        v-for="(searchInput, key) in searchInputs"
        :key="key"
        :dusk="`add-search-row-${searchInput.key}`"
        
        role="menuitem"
        @click.prevent="enableSearch(searchInput.key)"
      >
        {{ searchInput.label }}
      </b-dropdown-item>
    </div>
  </ButtonWithDropdown>
</template>

<script setup>
import ButtonWithDropdown from "./ButtonWithDropdown.vue";
import { ref } from "vue";

const props = defineProps({
    searchInputs: {
        type: Object,
        required: true,
    },

    hasSearchInputsWithoutValue: {
        type: Boolean,
        required: true,
    },

    onAdd: {
        type: Function,
        required: true,
    },
});

const dropdown = ref(null);

function enableSearch(key) {
    props.onAdd(key);
    dropdown.value.hide();
}
</script>
