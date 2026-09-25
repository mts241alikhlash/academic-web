<script setup lang="ts">
import { onMounted, ref } from 'vue'
import {
  Card,
  CardContent,
  CardHeader,
  CardTitle,
} from '@mts241alikhlash/ui/card'
import { Skeleton } from '@mts241alikhlash/ui/skeleton'
import { Badge } from '@mts241alikhlash/ui/badge'
import { DataTable } from '@mts241alikhlash/ui'
import { School, Users } from 'lucide-vue-next'
import { toast } from 'vue-sonner'
import { classroomApi } from '../api/classroomApi'
import type { MyClassroom } from '../types'
import { baseColumns } from '../components/enrollment-columns'
import { getIndonesianErrorMessage } from '@mts241alikhlash/web-shared/utils/error-handler'
import { formatEntityName } from '@mts241alikhlash/web-shared/utils/utils'

const data = ref<MyClassroom | null>(null)
const loading = ref(true)

onMounted(async () => {
  try {
    const res = await classroomApi.getMyClassroom()
    data.value = res.data?.data ?? null
  } catch (error: unknown) {
    toast.error(getIndonesianErrorMessage(error, 'Gagal memuat data kelas.'))
  } finally {
    loading.value = false
  }
})

function committee(structure: MyClassroom['structure']) {
  if (!structure) return []
  return [
    { role: 'Ketua Kelas', student: structure.president },
    { role: 'Wakil Ketua', student: structure.vicePresident },
    { role: 'Sekretaris', student: structure.secretary },
    { role: 'Bendahara', student: structure.treasurer },
  ].filter((seat) => seat.student)
}

function supervisorName(supervisor: MyClassroom['supervisor']) {
  return supervisor?.employee?.user?.profile?.name ?? null
}
</script>

<template>
  <div class="p-4 md:p-6 lg:p-8">
    <div
      v-if="loading"
      class="space-y-5"
    >
      <Skeleton class="h-[72px] w-full rounded-2xl" />
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-5">
        <div class="rounded-2xl border p-5 space-y-3">
          <Skeleton class="h-4 w-32 rounded" />
          <Skeleton class="h-px w-full" />
          <div class="space-y-2.5">
            <div
              v-for="i in 4"
              :key="i"
              class="flex justify-between"
            >
              <Skeleton class="h-3.5 w-20 rounded" />
              <Skeleton class="h-3.5 w-28 rounded" />
            </div>
          </div>
        </div>
        <div class="rounded-2xl border p-5 space-y-3">
          <Skeleton class="h-4 w-32 rounded" />
          <Skeleton class="h-px w-full" />
          <div class="space-y-2.5">
            <div
              v-for="i in 4"
              :key="i"
              class="flex justify-between"
            >
              <Skeleton class="h-3.5 w-24 rounded" />
              <Skeleton class="h-3.5 w-32 rounded" />
            </div>
          </div>
        </div>
      </div>
      <div class="rounded-2xl border p-5 space-y-3">
        <Skeleton class="h-4 w-28 rounded" />
        <Skeleton class="h-px w-full" />
        <div class="space-y-2">
          <Skeleton
            v-for="i in 6"
            :key="i"
            class="h-9 w-full rounded-lg"
          />
        </div>
      </div>
    </div>

    <Card
      v-else-if="!data"
      class="overflow-hidden rounded-2xl shadow-sm shadow-black/5 ring-1 ring-black/4"
    >
      <CardContent class="py-16 text-center">
        <School class="mx-auto size-10 text-muted-foreground opacity-40" />
        <p class="mt-3 text-sm font-medium">Anda belum terdaftar di kelas</p>
        <p class="mt-1 text-sm text-muted-foreground">
          Ini terjadi bila pendaftaran Anda pada semester berjalan belum
          diproses. Hubungi wali kelas atau tata usaha.
        </p>
      </CardContent>
    </Card>

    <template v-else>
      <Card
        class="overflow-hidden rounded-2xl shadow-sm shadow-black/5 ring-1 ring-black/4"
      >
        <CardHeader
          class="flex flex-row items-center justify-between border-b px-6 py-5"
        >
          <CardTitle class="text-2xl font-bold tracking-tight">
            Kelas {{ data.classroom.code }}
          </CardTitle>
        </CardHeader>

        <div class="p-6 space-y-5">
          <div class="grid grid-cols-1 lg:grid-cols-2 gap-5">
            <Card class="shadow-none">
              <CardHeader class="border-b px-5 py-4">
                <CardTitle class="text-sm font-semibold">
                  Informasi Kelas
                </CardTitle>
              </CardHeader>
              <CardContent class="px-5 py-4 space-y-2.5 text-sm">
                <div class="flex items-center justify-between gap-4">
                  <span class="text-muted-foreground shrink-0">Nama</span>
                  <span class="font-medium text-right">
                    {{
                      data.classroom.displayName
                        ? `${data.classroom.displayName} (${data.classroom.code})`
                        : (data.classroom.code ?? '-')
                    }}
                  </span>
                </div>
                <div class="flex items-center justify-between gap-4">
                  <span class="text-muted-foreground shrink-0">Tingkat</span>
                  <span class="font-medium text-right">
                    {{
                      data.classroom.grade?.name ??
                      data.classroom.classroomLevel?.name ??
                      '-'
                    }}
                  </span>
                </div>
                <div class="flex items-center justify-between gap-4">
                  <span class="text-muted-foreground shrink-0"
                    >Tahun Ajaran</span
                  >
                  <span class="font-medium text-right">
                    {{ data.classroom.academicYear?.name ?? '-' }}
                  </span>
                </div>
                <div class="flex items-center justify-between gap-4">
                  <span class="text-muted-foreground shrink-0">Wali Kelas</span>
                  <span
                    :class="[
                      'text-right',
                      supervisorName(data.supervisor)
                        ? 'font-medium'
                        : 'text-muted-foreground italic',
                    ]"
                  >
                    {{ supervisorName(data.supervisor) ?? 'Belum ditentukan' }}
                  </span>
                </div>
              </CardContent>
            </Card>

            <Card class="shadow-none">
              <CardHeader class="border-b px-5 py-4">
                <CardTitle class="text-sm font-semibold">
                  Struktur Kelas
                </CardTitle>
              </CardHeader>
              <CardContent class="px-5 py-4 text-sm">
                <p
                  v-if="committee(data.structure).length === 0"
                  class="text-muted-foreground"
                >
                  Struktur kelas belum disusun untuk semester ini.
                </p>
                <div
                  v-else
                  class="space-y-2.5"
                >
                  <div
                    v-for="seat in committee(data.structure)"
                    :key="seat.role"
                    class="flex items-center justify-between gap-4"
                  >
                    <span class="text-muted-foreground shrink-0">{{
                      seat.role
                    }}</span>
                    <span class="font-medium text-right">
                      {{
                        seat.student?.user?.profile?.name
                          ? formatEntityName(seat.student.user.profile.name)
                          : '-'
                      }}
                    </span>
                  </div>
                </div>
              </CardContent>
            </Card>
          </div>

          <Card class="shadow-none">
            <CardHeader class="border-b px-5 py-4">
              <div class="flex items-center gap-2">
                <CardTitle class="text-sm font-semibold">
                  Teman Sekelas
                </CardTitle>
                <Badge variant="secondary">
                  {{ data.classmates.length }}
                </Badge>
              </div>
            </CardHeader>
            <CardContent class="p-5 space-y-4">
              <div
                v-if="data.classmates.length === 0"
                class="flex flex-col items-center justify-center py-12 text-center border border-dashed rounded-lg bg-muted/20"
              >
                <Users class="h-12 w-12 text-muted-foreground/30 mb-4" />
                <p class="text-sm font-medium text-muted-foreground">
                  Belum ada siswa terdaftar di kelas ini.
                </p>
              </div>

              <DataTable
                v-else
                :columns="baseColumns"
                :data="data.classmates"
                filter-column="name"
                filter-placeholder="Cari nama siswa..."
                item-label="siswa"
                hide-per-page
                hide-pagination
                :page-size="500"
              />
            </CardContent>
          </Card>
        </div>
      </Card>
    </template>
  </div>
</template>
