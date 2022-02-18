<template>
  <div v-if="isLiveChatConnected">
    <live-chat-admin v-if="isAdmin"  />
    <live-chat-agent v-else />
  </div>

  <div v-else>
    <h5>
      Loading please wait . . .
    </h5>

    <div v-if="!isLiveChatConnected" class="text-center">
      <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="showLiveChatPopup = !showLiveChatPopup">
        Login LiveChat
      </button>
    </div>
    <login-live-chat v-model="showLiveChatPopup" />
  </div>
 
</template>


<script>
import LoginLiveChat from '@/components/LoginLiveChat.vue'
import LiveChatAdmin from '@/components/LiveChatAdmin.vue'
import LiveChatAgent from '@/components/LiveChatAgent.vue'

export default {
  name: 'LiveChatPage',
  components: {
    LoginLiveChat,
    LiveChatAdmin,
    LiveChatAgent
  },
  layout: 'BackOffice',
  data() {
    return {
      isAdmin: false,
      isLiveChatConnected: false,
      showLiveChatPopup: false,
    }
  },
  created() {
    this.checkIsAdmin()
    this.checkLiveChatToken()
  },
  methods: {
    checkIsAdmin() {
      const user = localStorage.getItem('user')
      const userObj = JSON.parse(user)

      if (!user) {
        alert('plese login')
        
        localStorage.clear()

        this.$router.push({ path: '/' })

        // force reload if not redirect
        setTimeout(() => {
          window.location.reload()
        }, 3000)
      }

      this.isAdmin = userObj.role === 'admin'
    },
    checkLiveChatToken() {
      const accessToken = localStorage.getItem('live_chat_access_token');

      this.isLiveChatConnected = accessToken;
    }
  }
}
</script>
