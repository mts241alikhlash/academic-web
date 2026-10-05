<script setup lang="ts">
import type { Grade } from '../types'
import { createGradeColumns } from '../components/columns'
import GradeFormDialog from '../components/GradeFormDialog.vue'
import { useGradeList } from '../composables/useGradeList'
import { DataTable, SearchInput } from '@mts241alikhlash/ui'
import { Button } from '@mts241alikhlash/ui/button'
import { Card, CardHeader, CardTitle } from '@mts241alikhlash/ui/card'
import { Plus } from '@lucide/vue'
import { useRoleGuard } from '@/features/platform/auth'
import { onMounted, ref, watch } from 'vue'
import { watchDebounced } from '@vueuse/core'

const { items, totalItems, loading, currentFilters, fetchGrades, deleteGrade } =
  useGradeList()
const searchKeyword = ref(currentFilters.value.search ?? '')

watchDebounced(
  searchKeyword,
  (val) => {
    void fetchGrades({
      search: val.trim() || '',
      page: 1,
    })
  },
  { debounce: 500 },
)

const isAddModalOpen = ref(false)
const editingItem = ref<Grade | null>(null)
const { can } = useRoleGuard()

const tableColumns = createGradeColumns({
  showActions: can('academic-years.update') || can('academic-years.delete'),
  canUpdate: can('academic-years.update'),
  canDelete: can('academic-years.delete'),
  onEdit: (item: Grade) => {
    editingItem.value = item
    isAddModalOpen.value = true
  },
  onDelete: async (item: Grade, { closeAlert, setLoading }) => {
    setLoading(true)
    const result = await deleteGrade(item.id)
    if (result.success) {
      await fetchGrades()
      closeAlert()
    }
    setLoading(false)
  },
})

watch(isAddModalOpen, (isOpen) => {
  if (!isOpen) {
    editingItem.value = null
  }
})

onMounted(() => {
  void fetchGrades()
})
</script>

<template>
  <div class="p-4 md:p-6 lg:p-8">
    <Card
      class="overflow-hidden rounded-2xl shadow-sm shadow-black/5 ring-1 ring-black/4"
    >
      <CardHeader
        class="flex flex-row items-center justify-between border-b px-6 py-5"
      >
        <CardTitle class="text-2xl font-bold tracking-tight">
          Tingkat Kelas
        </CardTitle>
        <Button
          v-if="can('academic-years.create')"
          @click="isAddModalOpen = true"
        >
          <Plus class="size-4 mr-2" />
          Tambah
        </Button>
      </CardHeader>

      <div class="p-6 space-y-4">
        <DataTable
          :columns="tableColumns"
          :data="items"
          :total-items="totalItems"
          :page="currentFilters.page"
          :is-loading="loading"
          item-label="tingkat kelas"
          @update:page="(page) => fetchGrades({ page })"
          @update:page-size="(limit) => fetchGrades({ limit, page: 1 })"
        >
          <template #header-right>
            <SearchInput
              v-model="searchKeyword"
              label="Cari tingkat kelas"
            />
          </template>
        </DataTable>

        <GradeFormDialog
          v-if="isAddModalOpen"
          v-model:open="isAddModalOpen"
          :edit-data="editingItem"
          @save-success="fetchGrades"
        />
      </div>
    </Card>
  </div>
</template>
