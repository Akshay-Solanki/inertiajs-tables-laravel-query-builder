<template>
  <div
    v-for="(searchInput, key) in searchInputs"
    v-show="searchInput.value !== null || isForcedVisible(searchInput.key)"
    :key="key"
    class="px-4 sm:px-0"
  >
    <b-input-group :for="searchInput.key">
      <b-input-group-prepend is-text>
        <i class="mdi mdi-magnify"/>
        <span>{{ searchInput.label }}</span>
      </b-input-group-prepend>
      <b-form-input
      :id="searchInput.key"
      :ref="skipUnwrap.el"
      :key="searchInput.key"
      :name="searchInput.key"
      :value="searchInput.value"
      @input="onChange(searchInput.key, $event)"
      type="text" aria-label="Text input with checkbox">
      </b-form-input>
      <b-input-group-append>
        <b-button size="sm" variant="light" class="border" :dusk="`remove-search-row-${searchInput.key}`"
          @click.prevent="onRemove(searchInput.key)">
          <i class="mdi mdi-close font-size-16"></i>
        </b-button>
      </b-input-group-append>
    </b-input-group>
    
  </div>
</template>

<script setup>
import { computed, ref, watch, nextTick } from "vue";
import find from "lodash-es/find";

const skipUnwrap = { el: ref([]) };
let el = computed(() => skipUnwrap.el.value);

const props = defineProps({
    searchInputs: {
        type: Object,
        required: true,
    },

    forcedVisibleSearchInputs: {
        type: Array,
        required: true,
    },

    onChange: {
        type: Function,
        required: true,
    },

    onRemove: {
        type: Function,
        required: true,
    },
});

function isForcedVisible(key) {
    return props.forcedVisibleSearchInputs.includes(key);
}

watch(props.forcedVisibleSearchInputs, (inputs) => {
    const latestInput = inputs.length > 0 ? inputs[inputs.length -1] : null;

    if(!latestInput) {
        return;
    }

    nextTick().then(() => {
        const inputElement = find(el.value, (el) => {
            return el.__vnode.key ===  latestInput;
        });

        if(inputElement) {
            inputElement.focus();
        }
    });
}, { immediate: true });
</script>

