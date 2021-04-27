<template>
  <div id="app">
    <appheader></appheader>
    <main class="main">
      <router-view/>
    </main>
  </div>
</template>
<script>
import Appheader from './components/Appheader.vue'


export default {
  components:{
    Appheader

  }
}
</script>

<style lang="scss">
@import 'scss/main';
</style>
