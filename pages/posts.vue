<template>
  <div>
    <NavBar />
    <b-container class="my-3">

      <b-row>
        <b-col lg="6" class="my-2">
          <b-form-group>
            <b-input-group>
              <b-form-input v-model="searchText" placeholder="Enter search Title" />
              <b-input-group-append>
                <b-button @click="search">search</b-button>
              </b-input-group-append>
            </b-input-group>
          </b-form-group>
        </b-col>
      </b-row>

      <b-table :items="filteredItems" :fields="tableFields" :per-page="perPage" :current-page.sync="currentPage" hoverable
        striped bordered @row-clicked="showUserModal" show-empty></b-table>

      <b-pagination v-model="currentPage" :total-rows="filteredItems.length" :per-page="perPage"
        align="center"></b-pagination>

    </b-container>

    <b-modal v-model="userModalVisible" title="User Information">
      <p>Name: {{ selectedUser?.name }}</p>
    </b-modal>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import { Post, User } from '~/types'
import NavBar from '~/components/NavBar.vue'
import { defineComponent } from '@nuxtjs/composition-api';
import { Context } from '@nuxt/types'

export default defineComponent({
  name: 'TodosPage',

  components: {
    NavBar,
  },

  data() {
    return {
      todos: [] as Post[],
      originalTodos: [] as Post[],
      searchText: '',
      userModalVisible: false,
      selectedUser: null as User | null,
      perPage: 10,
      currentPage: 1
    }
  },

  async asyncData({ $axios }: Context) {
    const { data } = await $axios.get('https://jsonplaceholder.typicode.com/posts')
    return { todos: data, originalTodos: data }
  },

  computed: {
    filteredItems(): Post[] {
      return this.todos
    },

    tableFields(): any {
      return [
        { key: 'title', label: 'Title' },
        { key: 'body', label: 'Body' },
      ]
    },
  },

  methods: {
    async fetchUser(userId: number) {
      const { data } = await axios.get(`https://jsonplaceholder.typicode.com/users/${userId}`)
      this.selectedUser = data
    },

    async showUserModal(row: Post) {
      await this.fetchUser(row.userId)
      this.userModalVisible = true
    },

    search() {
      const filteredTodos = this.originalTodos.filter(todo => todo.title.includes(this.searchText))
      this.todos = filteredTodos
      this.currentPage = 1
    },
  },
})
</script>
