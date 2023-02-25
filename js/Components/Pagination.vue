<template>
  <nav
    v-if="hasPagination"
    class="px-4 py-3 d-flex align-items-center justify-content-between "
  >
    <p v-if="!hasData || pagination.total < 1">
      {{ translations.no_results_found }}
    </p>

    <!-- simple and mobile -->
    <div
      v-if="hasData"
      class="flex-1 d-flex justify-content-between"
      :class="{'d-sm-none': hasLinks}"
    >
      <component
        :is="previousPageUrl ? 'a' : 'div'"
        
        :class="{
          'cursor-not-allowed text-gray-400': !previousPageUrl,
          'text-gray-700 hover:text-gray-500': previousPageUrl
        }"
        :href="previousPageUrl"
        :dusk="previousPageUrl ? 'pagination-simple-previous' : null"
        class="relative d-inline-flex align-items-center px-4 py-2 "
        @click.prevent="onClick(previousPageUrl)"
      >
        <i class="mdi mdi-arrow-left"></i>
        <span class="d-none d-sm-inline ml-2">{{ translations.previous }}</span>
      </component>
      <PerPageSelector
        dusk="per-page-mobile"
        :value="perPage"
        :options="perPageOptions"
        :on-change="onPerPageChange"
      />
      <component
        :is="nextPageUrl ? 'a' : 'div'"
        :class="{
          'cursor-not-allowed text-gray-400': !nextPageUrl,
          'text-gray-700 hover:text-gray-500': nextPageUrl
        }"
        :href="nextPageUrl"
        :dusk="nextPageUrl ? 'pagination-simple-next' : null"
        class="ml-3 relative d-inline-flex align-items-center px-4 py-2"
        @click.prevent="onClick(nextPageUrl)"
      >
        <span class="d-none d-sm-inline mr-2">{{ translations.next }}</span>
        <i class="mdi mdi-arrow-right"></i>
      </component>
    </div>

    <!-- full pagination -->
    <div
      v-if="hasData && hasLinks"
      class="d-none sm:flex-1 d-sm-flex align-items-sm-center justify-content-sm-between"
    >
      <div class="d-flex flex-row ml-3 align-items-center flex-grow-1">
        <PerPageSelector
          dusk="per-page-full"
          :value="perPage"
          :options="perPageOptions"
          :on-change="onPerPageChange"
        />

        <p class="d-none d-lg-block text-sm flex-grow-1 m-0 mx-2">
          <span class="font-medium">{{ pagination.from }}</span>
          {{ translations.to }}
          <span class="font-medium">{{ pagination.to }}</span>
          {{ translations.of }}
          <span class="font-medium">{{ pagination.total }}</span>
          {{ translations.results }}
        </p>
      </div>
      <div>
        <nav
          class="relative z-0 d-inline-flex "
          aria-label="Pagination"
        >
          <component
            :is="previousPageUrl ? 'a' : 'div'"
            :class="{
              'cursor-not-allowed text-gray-400 dark:bg-gray-700 dark:text-gray-500': !previousPageUrl,
              'text-gray-500 hover:bg-gray-50 dark:bg-gray-700 dark:text-gray-400 dark:hover:bg-gray-600': previousPageUrl
            }"
            :href="previousPageUrl"
            :dusk="previousPageUrl ? 'pagination-previous' : null"
            class="relative d-inline-flex align-items-center px-2 py-2 rounded-l-md border border-gray-300 bg-white text-sm font-medium dark:border-gray-500"
            @click.prevent="onClick(previousPageUrl)"
          >
            <span class="sr-only">{{ translations.previous }}</span>
            <i class="mdi mdi-chevron-left"></i>
          </component>

          <div
            v-for="(link, key) in pagination.links"
            :key="key"
          >
            <slot name="link">
              <component
                :is="link.url ? 'a' : 'div'"
                v-if="
                  !isNaN(link.label) || link.label === '...'
                "
                :href="link.url"
                :dusk="link.url ? `pagination-${link.label}` : null"
                class="relative d-inline-flex items-center px-4 py-2 border border-gray-300 bg-white text-sm font-medium text-gray-700 dark:bg-gray-700 dark:text-gray-100 dark:border-gray-500 dark:hover:bg-gray-500"
                :class="{
                  'cursor-not-allowed': !link.url,
                  'hover:bg-gray-50': link.url,
                  'bg-gray-100 dark:bg-blue-500 dark:hover:bg-blue-400': link.active,
                }"
                @click.prevent="onClick(link.url)"
              >
                {{ link.label }}
              </component>
            </slot>
          </div>

          <component
            :is="nextPageUrl ? 'a' : 'div'"
            :class="{
              'cursor-not-allowed text-gray-400 dark:bg-gray-700 dark:text-gray-500': !nextPageUrl,
              'text-gray-500 hover:bg-gray-50 dark:bg-gray-700 dark:text-gray-400 dark:hover:bg-gray-600': nextPageUrl
            }"
            :href="nextPageUrl"
            :dusk="nextPageUrl ? 'pagination-next' : null"
            class="relative inline-flex items-center px-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm font-medium dark:border-gray-500"
            @click.prevent="onClick(nextPageUrl)"
          >
            <span class="sr-only">{{ translations.next }}</span>
            <i class="mdi mdi-chevron-right"></i>
          </component>
        </nav>
      </div>
    </div>
  </nav>
</template>

<script setup>
import PerPageSelector from "./PerPageSelector.vue";
import { computed } from "vue";
import { getTranslations } from "./translations.js";

const translations = getTranslations();

const props = defineProps({
    onClick: {
        type: Function,
        required: false,
    },
    perPageOptions: {
        type: Array,
        default() {
            return () => [15, 30, 50, 100];
        },
        required: false
    },
    onPerPageChange: {
        type: Function,
        default() {
            return () => {};
        },
        required: false,
    },
    hasData: {
        type: Boolean,
        required: true,
    },
    meta: {
        type: Object,
        required: false,
    }
});

const hasLinks = computed(() => {
    if(!("links" in pagination.value)) {
        return false;
    }

    return pagination.value.links.length > 0;
});

const hasPagination = computed(() => {
    return Object.keys(pagination.value).length > 0;
});

const pagination = computed(() => {
    return props.meta;
});

const previousPageUrl = computed(() => {
    if ("prev_page_url" in pagination.value) {
        return pagination.value.prev_page_url;
    }

    return null;
});

const nextPageUrl = computed(() => {
    if ("next_page_url" in pagination.value) {
        return pagination.value.next_page_url;
    }

    return null;
});

const perPage = computed(() => {
    return parseInt(pagination.value.per_page);
});
</script>
