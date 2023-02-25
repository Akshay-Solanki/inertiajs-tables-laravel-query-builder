<template>
  <ButtonWithDropdown
    placement="bottom-end"
    dusk="filters-dropdown"
    :active="hasEnabledFilters"
  >
    <template #button>
      <div class="py-1">
        <i class="bx bx-filter-alt "></i>
      </div>
      
    </template>

    <div
      role="menu"
      aria-orientation="horizontal"
      aria-labelledby="filter-menu"
      class="min-w-max"
    >
      <div
        v-for="(filter, key) in filters"
        :key="key"
        class="rounded-5"
      >
      <!-- text-xs uppercase tracking-wide bg-gray-100 p-3 dark:text-gray-100 dark:bg-gray-700 dark:rounded-md -->
        <span class="font-weight-semibold p-2">
          {{ filter.label }}
        </span>
        <div class="p-2">
          <select
            v-if="filter.type === 'select'"
            :name="filter.key"
            :value="filter.value"
            class="form-control"
            @change="onFilterChange(filter.key, $event.target.value)"
          >
            <option
              v-for="(option, optionKey) in filter.options"
              :key="optionKey"
              :value="optionKey"
            >
              {{ option }}
            </option>
          </select>
        </div>
      </div>
    </div>
  </ButtonWithDropdown>
</template>

<script setup>
import ButtonWithDropdown from "./ButtonWithDropdown.vue";

defineProps({
    hasEnabledFilters: {
        type: Boolean,
        required: true,
    },

    filters: {
        type: Object,
        required: true,
    },

    onFilterChange: {
        type: Function,
        required: true,
    },
});
</script>

