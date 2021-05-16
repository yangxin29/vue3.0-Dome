<template>
  <div class="container">
    <GlobalHeader :user="currentUser"/>
    <ValidateForm @formSubmit="onFormSubmit" ref="inputRef">
      <div class="mb-3">
        <label class="form-label">邮箱地址</label>
        <ValidateInput
          placeholder="请输入邮箱地址"
          v-model="emailVal"
          :rules="emailRules"/>
      </div>
      <div class="mb-3">
        <label class="form-label">密码</label>
        <ValidateInput
          type="password"
          placeholder="请输入用户密码"
          v-model="emailVal"
          :rules="emailRules"/>
      </div>
      <template #submit>
        <span class="btn btn-danger">提交</span>
      </template>
    </ValidateForm>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'
import 'bootstrap/dist/css/bootstrap.min.css'
import GlobalHeader, { UserProps } from '@/components/GlobalHeader.vue'
import ValidateInput, { RulesProp } from '@/components/ValidateInput.vue'
import ValidateForm from '@/components/ValidateForm.vue'

const currentUser: UserProps = {
  isLogin: true,
  name: '杨鑫'
}

export default defineComponent({
  name: 'App',
  components: {
    GlobalHeader,
    ValidateInput,
    ValidateForm
  },
  setup () {
    const inputRef = ref<any>()
    const emailVal = ref('')
    const emailRules: RulesProp = [
      { type: 'required', message: '电子邮箱地址不能为空' },
      { type: 'email', message: '请输入正确的电子邮箱格式' }
    ]
    const emailRef = reactive({
      val: '',
      error: false,
      message: ''
    })
    const onFormSubmit = (result: boolean) => {
      console.log('result', result)
    }
    return {
      currentUser,
      emailRef,
      emailRules,
      emailVal,
      onFormSubmit,
      inputRef
    }
  }
})

</script>

<style lang="scss">
</style>
