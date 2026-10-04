<script setup lang="ts">
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { ReferenceDataListView } from '@/reference-data'
import { useReferenceListConfig } from '../config'
import { REFERENCE_LISTS } from '../lists'

const route = useRoute()
const list = computed(() =>
  REFERENCE_LISTS.find((l) => l.path === route.meta.referenceList)!,
)
const config = computed(() => useReferenceListConfig(list.value))
</script>

<template>
  <div class="p-4 md:p-6 lg:p-8">
    <ReferenceDataListView
      :key="list.path"
      :config="config"
    />
  </div>
</template>
