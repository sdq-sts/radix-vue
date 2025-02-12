<script setup lang="ts">
import { inject } from 'vue'
import PopoverContentModal from './PopoverContentModal.vue'
import PopoverContentNonModal from './PopoverContentNonModal.vue'
import {
  type PopoverContentImplEmits,
  type PopoverContentImplProps,
} from './PopoverContentImpl.vue'
import { POPOVER_INJECTION_KEY } from './PopoverRoot.vue'
import { useEmitAsProps } from '@/shared'
import { Presence } from '@/Presence'
import { PopperContentPropsDefaultValue } from '@/Popper'

export interface PopoverContentProps extends PopoverContentImplProps {
  /**
   * Used to force mounting when more control is needed. Useful when
   * controlling animation with React animation libraries.
   */
  forceMount?: true
}
export type PopoverContentEmits = PopoverContentImplEmits

const props = withDefaults(defineProps<PopoverContentProps>(), {
  ...PopperContentPropsDefaultValue,
})
const emits = defineEmits<PopoverContentEmits>()

const context = inject(POPOVER_INJECTION_KEY)

const emitsAsProps = useEmitAsProps(emits)
</script>

<template>
  <Presence :present="forceMount || context!.open.value">
    <PopoverContentModal
      v-if="context?.modal.value"
      v-bind="{ ...props, ...emitsAsProps }"
    >
      <slot />
    </PopoverContentModal>
    <PopoverContentNonModal v-else v-bind="{ ...props, ...emitsAsProps }">
      <slot />
    </PopoverContentNonModal>
  </Presence>
</template>
