<script setup lang="ts">
import GroupStatsDetail from '@/components/GroupStatsDetail.vue'

definePageMeta({ layout: 'default' })

const { load } = useStatsData()
onMounted(load)
</script>

<template>
  <GroupStatsDetail
    group-key="mobile"
    title="Mobile"
    description="Статистика по направлению Mobile (iOS и Android). 0 баллов не учитываются."
  />
</template>
