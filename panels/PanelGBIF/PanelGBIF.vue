<template>
  <VCard v-if="url">
    <VCardHeader><GBIFLogo class="h-6" /></VCardHeader>
    <VCardContent>
      <a
        class="text-sm"
	target="_blank"
        :href="url"
        v-html="taxon.full_name_tag"
      />
    </VCardContent>
  </VCard>
</template>

<script setup>
import { computed, ref, onMounted } from 'vue'
import axios from 'axios'
import GBIFLogo from './components/gbifLogo.vue'

const props = defineProps({
  taxon: {
    type: Object,
    required: true
  },

  perPage: {
    type: Number,
    default: 60
  }
})

const url = computed(() => {
  return usageKey.value
    ? `https://www.gbif.org/occurrence/search?dataset_key=eae731a7-3e82-4295-b0b3-ec72d75a402d&dataset_key=94adad5f-fb11-426f-93e0-3dd02f3ccd1d&dataset_key=afc62d53-02ae-4bf9-bebc-19255a3417b1&taxon_key=${usageKey.value}&occurrence_status=present`
    : null
})
const usageKey = ref(null)

function loadUsageKey() {
  axios
    .get('https://api.gbif.org/v1/species/match', {
      params: {
        name: props.taxon.expanded_name
      }
    })
    .then(({ data }) => {
      usageKey.value = data?.usageKey
    })
}

onMounted(loadUsageKey)
</script>
