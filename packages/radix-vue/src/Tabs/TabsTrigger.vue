<script setup lang="ts">
import { computed, inject } from 'vue'
import { useArrowNavigation } from '../shared'
import { TABS_INJECTION_KEY } from './TabsRoot.vue'
import {
  Primitive,
  type PrimitiveProps,
  usePrimitiveElement,
} from '@/Primitive'

export interface TabsTriggerProps extends PrimitiveProps {
  value: string
  disabled?: boolean
}

const props = withDefaults(defineProps<TabsTriggerProps>(), {
  disabled: false,
  as: 'button',
})
const injectedValue = inject(TABS_INJECTION_KEY)
const { primitiveElement, currentElement } = usePrimitiveElement()

function changeTab(value: string) {
  injectedValue?.changeModelValue(value)
}

function handleKeydown(e: KeyboardEvent) {
  const newSelectedElement = useArrowNavigation(
    e,
    currentElement.value!,
    injectedValue?.parentElement.value!,
    {
      arrowKeyOptions: injectedValue?.orientation,
      loop: injectedValue?.loop.value,
      focus: true,
    },
  )

  if (!newSelectedElement)
    return

  injectedValue!.currentFocusedElement!.value = newSelectedElement

  if (injectedValue?.activationMode === 'automatic')
    changeTab(newSelectedElement?.getAttribute('data-radix-vue-tab-value')!)
}

const getTabIndex = computed(() => {
  if (!injectedValue?.currentFocusedElement?.value) {
    return injectedValue?.modelValue?.value === props.value ? '0' : '-1'
  }
  else {
    return injectedValue?.currentFocusedElement?.value === currentElement.value
      ? '0'
      : '-1'
  }
})
</script>

<template>
  <Primitive
    ref="primitiveElement"
    :type="as === 'button' ? 'button' : undefined"
    :as="as"
    :as-child="props.asChild"
    role="tab"
    :aria-selected="
      injectedValue?.modelValue?.value === props.value ? 'true' : 'false'
    "
    :data-state="
      injectedValue?.modelValue?.value === props.value ? 'active' : 'inactive'
    "
    :disabled="props.disabled"
    :data-disabled="props.disabled ? '' : undefined"
    :tabindex="getTabIndex"
    :data-orientation="injectedValue?.orientation"
    data-radix-vue-collection-item
    :data-radix-vue-tab-value="props.value"
    @click="changeTab(props.value!)"
    @keydown.up.down.left.right.home.end="handleKeydown"
  >
    <slot />
  </Primitive>
</template>
