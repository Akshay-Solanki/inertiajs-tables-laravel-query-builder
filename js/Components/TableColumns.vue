<template>
  <ButtonWithDropdown
    placement="bottom-end"
    dusk="columns-dropdown"
    :active="hasHiddenColumns"
  >
    <template #button>
      <i class="mdi mdi-eye font-size-16" />
    </template>

    <div
      role="menu"
      aria-orientation="horizontal"
      aria-labelledby="toggle-columns-menu"
      class="min-w-max"
    >
      <div class="">
          <li
            v-for="(column, key) in props.columns"
            v-show="column.can_be_hidden"
            :key="key"
            class="d-flex align-items-center justify-content-between"
          >
          <!-- py-2 flex items-center justify-between -->
            <span
              class="text-sm text-gray-900 dark:text-gray-100 text-nowrap px-3"
            >
              {{ column.label }}
            </span>
            <b-form-checkbox switch size="lg"
            :checked="!column.hidden"
            :unchecked-value="false"
            @click.prevent="onChange(column.key, column.hidden)"
            ></b-form-checkbox>
            <!-- <button
              type="button"
              class="ml-4 relative inline-flex flex-shrink-0 h-6 w-11 border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none focus:ring-2 focus:ring-light-blue-500"
              :class="{
                'bg-green-500': !column.hidden,
                'bg-gray-200': column.hidden,
              }"
              :aria-pressed="!column.hidden"
              :aria-labelledby="`toggle-column-${column.key}`"
              :aria-describedby="`toggle-column-${column.key}`"
              :dusk="`toggle-column-${column.key}`"
              @click.prevent="onChange(column.key, column.hidden)"
            >
              <span class="sr-only">Column status</span>
              <span
                aria-hidden="true"
                :class="{
                  'translate-x-5': !column.hidden,
                  'translate-x-0': column.hidden,
                }"
                class="inline-block h-5 w-5 rounded-full bg-white shadow transform ring-0 transition ease-in-out duration-200"
              />
            </button> -->
          </li>

      </div>
    </div>
  </ButtonWithDropdown>
</template>

<script setup>
import ButtonWithDropdown from "./ButtonWithDropdown.vue";

const props = defineProps({
    columns: {
        type: Object,
        required: true,
    },

    hasHiddenColumns: {
        type: Boolean,
        required: true,
    },

    onChange: {
        type: Function,
        required: true,
    },
});
</script>
