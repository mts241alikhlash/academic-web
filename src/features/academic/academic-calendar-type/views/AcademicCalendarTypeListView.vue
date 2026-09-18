<script setup lang="ts">
import { ReferenceDataListView } from '@/reference-data'
import { useAcademicCalendarTypeConfig } from '../config'

const config = useAcademicCalendarTypeConfig()
</script>

<template>
  <div class="p-4 md:p-6 lg:p-8">
    <ReferenceDataListView :config="config" />
  </div>
</template>
