<template>
  <b-navbar toggleable="lg" type="dark" variant="dark">
    <b-navbar-brand href="/">DataCose</b-navbar-brand>
    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item :active="activeNavItem == '/posts'" :to="{ name: 'posts' }">Posts</b-nav-item>
        <b-nav-item :active="activeNavItem == '/todos'" :to="{ name: 'todos' }">Todos</b-nav-item>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script lang="ts">
import { useRoute, defineComponent } from '@nuxtjs/composition-api';

export default defineComponent({
  name: 'NavBar',

  computed: {
    activeNavItem() {
      const route = useRoute()
      const routes = ['/todos', '/posts']
      const matchingRoute = routes.find(r => r === route.value.path)
      return matchingRoute ? matchingRoute : null
    }
  }
})
</script>