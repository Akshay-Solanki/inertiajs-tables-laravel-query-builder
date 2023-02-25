<template>
  <th
    v-show="!cell.hidden"
  >
    <component
      :is="'div'"
      :role="cell.sortable ? 'button' : 'div'"
      class="py-3 px-6 w-full"
      :dusk="cell.sortable ? `sort-${cell.key}` : null"
      @click.prevent="onClick"
    >
      <div class="d-flex flex-row align-items-center justify-content-between">
        <slot name="label"><span class="uppercase">{{ cell.label }}</span></slot>

        <slot name="sort">
          <div v-if="cell.sortable" :sorted="cell.sorted" :class="{'text-gray-400': !cell.sorted, 'text-green-500': cell.sorted }">
            <i
            :class="{
              'mdi mdi-sort' : !cell.sorted,
              'mdi mdi-sort-alphabetical-ascending' : cell.sorted === 'asc',
              'mdi mdi-sort-alphabetical-descending' : cell.sorted === 'desc',
            }"
            >
            </i>
          </div>
          
        </slot>
      </div>
    </component>
  </th>
</template>

<script setup>
const props = defineProps({
    cell: {
        type: Object,
        required: true,
    },
});

function onClick() {
    if (props.cell.sortable) {
        props.cell.onSort(props.cell.key);
    }
}
</script>