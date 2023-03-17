<template>
  <div>
    <NavBar />
    <b-container class="d-flex justify-content-center pt-5">
      <h1>Welcome to my website!</h1>
    </b-container>
  </div>
</template>
  
<script>
import NavBar from '~/components/NavBar.vue'

export default {
  name: 'IndexPage',
  components: {
    NavBar,
  },
}
</script>