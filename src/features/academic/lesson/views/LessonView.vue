<script setup lang="ts">
import { onMounted } from 'vue'
import { useRouter } from 'vue-router'
import { DataTable } from '@mts241alikhlash/ui'
import {
  Card,
  CardHeader,
  CardTitle,
  CardContent,
} from '@mts241alikhlash/ui/card'
import { createLessonColumns } from '../components/columns'
import { useLessonClassrooms } from '../composables/useLessonClassrooms'
import type { LessonClassItem } from '../types'

const router = useRouter()
const {
  classrooms,
  loading: isLoading,
  fetchClassrooms,
} = useLessonClassrooms()

const tableColumns = createLessonColumns({
  onManageSchedule: (cls: LessonClassItem) => {
    void router.push({
      name: 'classroom-schedule-editor',
      params: { classroomId: cls.id },
    })
  },
})

onMounted(() => {
  void fetchClassrooms()
})
</script>

<template>
  <div class="p-4 md:p-6 lg:p-8">
    <Card
      gap="none"
      class="overflow-hidden rounded-2xl shadow-sm shadow-black/5 ring-1 ring-black/4"
    >
      <CardHeader standard>
        <CardTitle size="lg"> Jadwal Pelajaran </CardTitle>
      </CardHeader>
      <CardContent variant="standard">
        <DataTable
          :columns="tableColumns"
          :data="classrooms"
          :is-loading="isLoading"
          item-label="kelas"
          filter-column="code"
          filter-placeholder="Cari kelas..."
        />
      </CardContent>
    </Card>
  </div>
</template>
