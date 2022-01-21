<template>
  <div class="app">
    <Navbar/>
<!--    <h1>This is testing the layout</h1>-->
    <Nuxt keep-alive />
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
export default {
  components: {Navbar}
}
</script>
