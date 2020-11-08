<template>
  <div id="app" class="container">
    <task-component></task-component>
  </div>
</template>

<script>
import TaskComponent from './components/Task/TaskComponent'

export default {
  name: 'App',
  components: {
    TaskComponent
  }
}
</script>

<style>

</style>
