<template>
  <div class="max-h-32 overflow-y-auto text-xs min-w-80">
    <ul>
      <li
        v-for="item in items"
        class="py-2 last:border-0 truncate border-b"
        title="label"
      >
        <a :href="`https://www.gbif.org/occurrence/search?occurrenceId=${getOccurrenceId(item.label)}`" target="_blank">{{ getOccurrenceId(item.label) }}</a>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { COLLECTION_OBJECT, FIELD_OCCURRENCE } from '@/constants/objectTypes.js'

const CLICKEABLE_TYPES = [COLLECTION_OBJECT, FIELD_OCCURRENCE]

defineProps({
  items: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['selected'])

function getOccurrenceId(label, pattern) {
  if (!label) return ''
  const ids = label.split(';')
  const match = label.match(/occurrenceID:\s*([^;,]+)/)
  const occurrenceIds = ids.find(id => id.match(/^\s*occurrenceID/))?.trim() || ''

  return match ? match[1].trim() : '' // occurrenceIds.split(',')[0]
}
</script>
