<script setup lang="ts">
import type { HTMLAttributes } from "vue"
import { computed, onMounted, ref } from "vue"
import { cn } from "@/lib/utils"
import { useChart } from "."

const props = withDefaults(defineProps<{
  hideIcon?: boolean
  nameKey?: string
  verticalAlign?: "bottom" | "top"
  // payload?: any[]
  class?: HTMLAttributes["class"]
}>(), {
  verticalAlign: "bottom",
})

const { id, config } = useChart()

const payload = computed(() => Object.entries(config.value).map(([key, value]) => {
  return {
    key: props.nameKey || key,
    itemConfig: config.value[key],
  }
}))

const containerSelector = ref("")
onMounted(() => {
  containerSelector.value = `[data-chart="chart-${id}"]>[data-vis-xy-container]`
})
</script>

<template>
  <div
    v-if="containerSelector"
    :class="cn(
      'flex flex-wrap items-center justify-center gap-x-6 gap-y-2 text-sm sm:gap-4 sm:text-xs',
      verticalAlign === 'top' ? 'pb-3' : 'pt-3',
      props.class,
    )"
  >
    <div
      v-for="{ key, itemConfig } in payload"
      :key="key"
      :class="cn(
        '[&>svg]:text-muted-foreground flex items-center gap-2 [&>svg]:h-4 [&>svg]:w-4 sm:gap-1.5 sm:[&>svg]:h-3 sm:[&>svg]:w-3',
      )"
    >
      <component :is="itemConfig.icon" v-if="itemConfig?.icon" />
      <div
        v-else
        class="h-3 w-3 shrink-0 rounded-[2px] sm:h-2 sm:w-2"
        :style="{
          backgroundColor: itemConfig.color,
        }"
      />

      <span class="font-medium sm:font-normal">{{ itemConfig?.label }}</span>
    </div>
  </div>
</template>
