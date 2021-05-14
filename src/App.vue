<template>
  <div id="nav">
    <span style="color: #ea1414" @click="add">{{ count }}</span>
    {{doubel}}
    <ul>
      <li v-for="number in numbers" :key="number">
        {{ number }}
      </li>
    </ul>
    <h1 v-if="loading">Loading!...</h1>
    <img v-if="loaded" :src="result.message" alt="">
    <div @click="unpteGteetin">
      hah
      {{greeting}}
      <h1>坐标</h1>
      <div>X:{{x}}</div>
      <div>Y:{{y}}</div>
    </div>
  </div>
  <router-view/>
  <div class="container">
    <div></div>
  </div>
</template>

<script lang="ts">
import { ref, computed, reactive, toRefs, watch, onMounted, onUnmounted } from 'vue'
import useMousePosition from './hooks/useMousePosition'
import useURLLoader from '@/hooks/useURLLoader'

interface DataProps {
  count: number;
  doubel: number;
  add: () => void;
  numbers: number[];
  person: {name ?: string}
}

interface DogResult {
  message: string;
  status: string
}

export default {
  name: 'app',
  setup () {
    // const count = ref(0)
    // const doubel = computed(() => {
    //   return count.value * 2
    // })
    // const add = () => {
    //   count.value++
    // }
    // return {
    //   count,
    //   add,
    //   doubel
    // }
    const data:DataProps = reactive({
      count: 0,
      add: () => {
        data.count++
      },
      doubel: computed(() => {
        return data.count * 2
      }),
      numbers: [1, 2, 3],
      person: { name: 'yangxin' }
    })

    const greeting = ref('')
    const unpdateGteetin = () => {
      greeting.value += 'hello'
    }
    // 监听器
    watch([greeting, () => data.count], (newValue, oldValue) => {
      console.log(newValue, 'newVal')
      console.log(oldValue, 'oldVal')
      document.title = 'update' + greeting.value + data.count
    })
    const { x, y } = useMousePosition()
    const { result, loading, loaded } = useURLLoader<DogResult>('https://dog.ceo/api/breeds/image/random')
    watch(result, () => {
      if (result.value) {
        console.log('value', result.value.message)
      }
    })
    return {
      ...toRefs(data),
      greeting,
      unpdateGteetin,
      x,
      y,
      result,
      loading,
      loaded
    }
  }
}
</script>

<style lang="scss">
.container{
  width: 300px;
  height: 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
  border: 1px solid red;
  div{
    width: 100px;
    height: 20px;
    background: red;
  }
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
