<template>
  <div>
    <Home />
  </div>
</template>

<script>
import Home from './components/Home.vue'

export default {
  name: 'app',
  components: {
    Home
  }
}
</script>

<style lang="sass">
*
  margin: 0
  padding: 0
  box-sizing: border-box
body
  font-family: sans-serif
.container
  width: 60%
  margin: 30px auto
h1
  border-bottom: 1px solid #bbb
  margin-top: 30px
ul
  list-style: none

@media only screen and (max-width: 500px)
  .container
    width: 98%
  h1
    text-align: center
</style>
