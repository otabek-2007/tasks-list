<template>
  <div id="app">
    <task-list></task-list>
  </div>
</template>

<script>
import TaskList from '@/components/TaskView.vue'
const components = {
  TaskList
}
export default {
  name: 'App',
  components
}
</script>

<style lang="scss" scoped>
  #app{
    
  }
</style>
