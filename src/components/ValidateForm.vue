<template>
  <form class="validate-form-container">
    <slot name="default"></slot>
    <div class="submit-area" @click.prevent="submitForm">
      <slot name="submit">
        <button type="submit" class="btn btn-primary">提交</button>
      </slot>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUnmounted } from 'vue'
import mitt from 'mitt'

type ValidateFunc = () => boolean

// 新建监听器
export const emitter = mitt()

export default defineComponent({
  name: 'ValidateForm',
  emits: ['form-submit'],
  setup (props, context) {
    let funcArr: ValidateFunc[] = [] // 存放的回调
    const submitForm = () => {
      const result = funcArr.map(func => func()).every(result => result)
      context.emit('form-submit', result)
    }
    const callback = (func: ValidateFunc) => {
      funcArr.push(func)
    }
    emitter.on('formItemCreated', callback)
    // 取消订阅
    onUnmounted(() => {
      emitter.off('formItemCreated', callback)
      // 指控数组
      funcArr = []
    })
    return {
      submitForm
    }
  }
})
</script>

<style scoped>

</style>
