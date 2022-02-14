<template>
  <div class="text-center my-16">
    <h5 class="text-5xl font-extrabold bg-clip-text text-transparent bg-gradient-to-r from-pink-500 to-blue-500">Welcome Tester</h5>
    <p class="text-lg mt-3">Please Select User to Continue</p>
    <div class="flex flex-col m-8 justify-center items-center">
      <div v-for="user in users" :key="user.id" class="p-2 lg:w-1/3 md:w-1/2 w-full">
        <a href="#" @click="loggedIn(user)">
          <div class="h-full flex items-center border-gray-200 border p-4 rounded-lg hover:bg-gray-200 hover:text-gray-600">
            <img alt="team" class="w-16 h-16 bg-gray-100 object-cover object-center flex-shrink-0 rounded-full mr-4" src="https://dummyimage.com/80x80">
            <div class="flex-grow text-left">
              <h2 class="text-gray-900 title-font font-medium">
                {{ user.name }}
              </h2>
              <p class="text-gray-500">
                {{ user.email }}
              </p>
            </div>
          </div>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import { apiUrl } from '@/services/api'

export default {
  name: 'IndexPage',
  data() {
    return {
     users: []
    }
  },
  async fetch() {
    this.users = await this.$axios.$get(`${apiUrl()}/users`)
  },
  beforeCreate () {
    if (localStorage.is_logged_in) {
      this.$router.push('/p/home') 
    }
  },
  methods: {
    loggedIn(user) {
      localStorage.setItem('is_logged_in', true)
      localStorage.setItem('user', JSON.stringify(user))
      this.$router.push('/p/home')
    },
    updateFCMToken() {
      // TODO: 
    },
  }
}
</script>
