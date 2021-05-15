<template>
  <div class="container">
    <GlobalHeader :user="currentUser"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import 'bootstrap/dist/css/bootstrap.min.css'
import GlobalHeader, { UserProps } from '@/components/GlobalHeader.vue'

const currentUser: UserProps = {
  isLogin: true,
  name: '杨鑫'
}

export default defineComponent({
  name: 'App',
  components: {
    GlobalHeader
  },
  setup () {
    return {
      currentUser
    }
  }
})

</script>

<style lang="scss">
</style>
