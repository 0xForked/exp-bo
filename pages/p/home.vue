<template>
  <div class="container mx-auto">
    <h5 class="text-xl">
      Your are logged in as 
      <span class="text-red-500">{{ user.name }}</span> 
    </h5>
    <h5 class="mt-3 text-xl">With detail :</h5> 
    <div class="bg-gray-100 rounded m-3 p-8">
      <pre class="text-ellipsis overflow-hidden">{{ JSON.stringify(user, null, 2) }}</pre>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  layout (context) {
    return 'BackOffice'
  },
  data () {
    return {
      user: {}
    }
  },
  created() {
    this.user = JSON.parse(localStorage.getItem('user'))
  }
}
</script>
