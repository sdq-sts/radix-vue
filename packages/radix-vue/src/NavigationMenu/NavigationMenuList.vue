<script setup lang="ts">
import { inject, onMounted } from 'vue'
import { NAVIGATION_MENU_INJECTION_KEY } from './NavigationMenuRoot.vue'
import {
  Primitive,
  type PrimitiveProps,
  usePrimitiveElement,
} from '@/Primitive'

export interface NavigationMenuListProps extends PrimitiveProps {}
const props = withDefaults(defineProps<NavigationMenuListProps>(), {
  as: 'ul',
})

const context = inject(NAVIGATION_MENU_INJECTION_KEY)
const { primitiveElement, currentElement } = usePrimitiveElement()

onMounted(() => {
  context?.onIndicatorTrackChange(currentElement.value)
})
</script>

<script lang="ts">
export default {
  inheritAttrs: false,
}
</script>

<template>
  <Primitive ref="primitiveElement" style="position: relative">
    <Primitive
      v-bind="$attrs"
      :as-child="props.asChild"
      :as="as"
      :data-orientation="context?.orientation"
    >
      <slot />
    </Primitive>
  </Primitive>
</template>
