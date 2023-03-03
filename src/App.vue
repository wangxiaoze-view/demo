<template>
  <div class="container">
  <dom-player/>
  </div>
</template>

<script>
import domPlayer from './components/domPlayer'
export default {
  name: 'App',
  components: {
    domPlayer
  }
}
</script>

<style>
.container {
  padding: 30px;
}

</style>
