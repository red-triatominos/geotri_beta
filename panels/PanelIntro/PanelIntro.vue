<template>
  <VCard v-if="intro">
    <VCardContent class="panel-content-list">
      <div
        class="pt-1 text-sm"
        v-html="intro.text"
      ></div>
    </VCardContent>
  </VCard>
</template>

<script setup>
import { computed, ref, onBeforeMount, onBeforeUnmount } from 'vue'
import { useOtuPageRequest } from '@/modules/otus/helpers/useOtuPageRequest'
import TaxonWorks from '@/modules/otus/services/TaxonWorks'

const props = defineProps({
  otuId: {
    type: Number,
    required: true
  }
})

const contents = ref([])
const controller = new AbortController()

const intro = computed(() =>
  contents.value.find((c) => c.name === "Intro (Spanish)")
)

onBeforeMount(() => {
  useOtuPageRequest('panel:content', () =>
    TaxonWorks.getOtuContent(props.otuId, {
      params: {
        extend: ['depiction']
      },
      signal: controller.signal
    })
  )
    .then(({ data }) => {
      contents.value = data
    })
    .catch((e) => {})
})

onBeforeUnmount(() => {
  controller.abort()
})
</script>

<style>
.panel-content-list {
  ul {
    margin: 1rem 0;
    list-style: disc;
    margin-left: 1rem;
  }

  ol {
    list-style-type: decimal;
    margin-left: 1rem;
  }
}
</style>