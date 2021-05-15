<template>
  <div class="dropdown" ref="dropdownRef">
    <a href="#" class="btn btn-outline-light my-2 dropdown-toggle" @click.prevent="toggleOpen">{{title}}</a>
    <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1" style="display: block" v-if="isOpen">
      <slot></slot>
    </ul>
  </div>
</template>

<script lang="ts">

import { defineComponent, ref, watch } from 'vue'
import useClickOutside from '@/hooks/useClickOutside'
export default defineComponent({
  name: 'Dropdown',
  props: {
    title: {
      type: String,
      required: true
    }
  },
  setup () {
    const isOpen = ref(false)
    const dropdownRef = ref<null | HTMLElement>(null)
    const toggleOpen = () => {
      isOpen.value = !isOpen.value
    }
    // 使用自定义hook
    const isClickOutside = useClickOutside(dropdownRef)
    watch(isClickOutside, () => {
      if (isOpen.value && isClickOutside.value) {
        isOpen.value = false
      }
    })
    return {
      isOpen,
      toggleOpen,
      dropdownRef,
      isClickOutside
    }
  }
})

</script>

<style lang="scss" scoped>

</style>
