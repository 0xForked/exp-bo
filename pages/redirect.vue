<template>
  <p>Loading please wait . . . .</p>
</template>

<script>
export default {
  name: 'RedirectPage',
  created() {
    localStorage.setItem('live_chat_access_token', decodeURIComponent(this.getHashParam("access_token")))
    location.reload()
    const win = open(location, '_self')
    win.close();
  },
  methods: {
    getHashParam(key) {
      const params = location.hash.substring(1).split("&");
      const value = params.find(function (item) {
        return item.split("=")[0] === key;
      });
      
      return value ? value.split("=")[1] : "";
    }
  }
}
</script>
