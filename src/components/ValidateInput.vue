<template>
  <div class="validate-input-container pd-3">
    <input
      type="text"
      class="form-control"
      :class="{'is-invalid': inputRef.error}"
      aria-describedby="emailHelp"
      :value="inputRef.val"
      v-bind="$attrs"
      @blur="validateEmail"
      @input="updateValue"
    >
    <div class="invalid-feedback" v-if="inputRef.error">
      {{inputRef.message}}
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, PropType, reactive } from 'vue'
import { emitter } from './ValidateForm'
const emailReg = /^[A-Za-z\d]+([-_.][A-Za-z\d]+)*@([A-Za-z\d]+[-.])+[A-Za-z\d]{2,4}$ /
interface RuleProp {
  type: 'required' | 'email',
  message: string
}

export type RulesProp = RuleProp[]
export default defineComponent({
  name: 'ValidateInput',
  inheritAttrs: false, // 不希望组件的根元素继承attribute
  props: {
    rules: {
      type: Array as PropType<RuleProp>
    },
    modelValue: {
      type: String
    }
  },
  setup (props, context) {
    const inputRef = reactive({
      val: props.modelValue || '',
      error: false,
      message: ''
    })
    const updateValue = (e: KeyboardEvent) => {
      const targetValue = (e.target as HTMLInputElement).value
      inputRef.val = targetValue
      context.emit('update:modelValue', targetValue)
    }
    const emailReg = /^[A-Za-z\d]+([-_.][A-Za-z\d]+)*@([A-Za-z\d]+[-.])+[A-Za-z\d]{2,4}$ /
    // 校验的回调函数
    const validateInput = () => {
      if (props.rules) {
        const allPassed = props.rules.every(rule => {
          let passed = true
          inputRef.message = rule.message
          switch (rule.type) {
            case 'required':
              passed = (inputRef.val.trim() !== '')
              break
            case 'email':
              passed = emailReg.test(inputRef.val)
              break
            default:
              break
          }
          return passed
        })
        inputRef.error = !allPassed
        return allPassed
      }
      return true
    }
    onMounted(() => {
      emitter.emit('formItemCreated', validateInput)
    })
    return {
      inputRef,
      validateInput,
      updateValue
    }
  }
})
</script>

<style scoped>

</style>
