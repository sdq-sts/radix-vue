<script setup lang="ts">
import { Primitive, type PrimitiveProps } from '@/Primitive'

export interface MenuSeparatorProps extends PrimitiveProps {}

const props = defineProps<MenuSeparatorProps>()
</script>

<template>
  <Primitive v-bind="props" role="separator" aria-orientation="horizontal">
    <slot />
  </Primitive>
</template>
