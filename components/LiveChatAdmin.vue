<template>
  <div class="w-full h-screen">
    <div v-if="loading">
      <h5>
        Loading please wait . . .
      </h5>
    </div>

    <div v-else class="flex h-full overflow-auto px-8">
      <div class="lg:block heading flex-2">
        <h1 class="text-xl text-gray-700 mb-4">Conversation List ({{ totalChat }})</h1>
      </div>

      <div v-for="chat in chatList" :key="chat.id">
        <div v-if="chat.last_event_per_type.filled_form.event.form_type !== 'postchat'" class="h-20 m-3 entry cursor-pointer transform hover:scale-105 duration-300 transition-transform bg-white mb-4 rounded p-4 flex shadow-md border-l-4" :class="chat === selectedChat ? 'border-red-500' : ''" @click="setSelectedChat(chat)">
          <div class="flex-2">
            <div class="w-12 h-12 relative">
                <img class="w-12 h-12 rounded-full mx-auto" src="https://dummyimage.com/80x80" alt="chat-user" />
                <span class="absolute w-4 h-4 rounded-full right-0 bottom-0 border-2 border-white" :class="chat.last_thread_summary.active ? 'bg-green-400' : ''"></span>
            </div>
          </div>
          <div class="flex-1 px-2">
              <div class="truncate w-32">
                <span class="text-gray-800">{{ chat.last_event_per_type.filled_form.event.fields[0].answer }}</span>
              </div>
              <div>
                <small class="text-gray-600">{{ chat.last_event_per_type.message.event.text }}</small>
              </div>
          </div>
          <div class="flex-2 text-right">
            <div>
              <small class="text-gray-500">{{ myDateFormat(chat.last_event_per_type.filled_form.event.created_at) }}</small>
            </div>
          </div>
        </div>
      </div>
    </div>

    <pre> {{ agentList }} </pre>
    <pre> {{ chatList }} </pre>
  </div>
</template>

<script>
export default {
  name: 'LiveChatAdmin',
  components: {},
  data() {
    return {
      ws: null,
      chatList: [],
      agentList: [],
      showChatList: [],
      totalChat: 0,
      loading: true,
      selectedChat: {}
    }
  },
  created() {
    const accessToken = localStorage.getItem('live_chat_access_token');

    this.ws =  new WebSocket("wss://api.livechatinc.com/v3.4/agent/rtm/ws")

    this.ws.onmessage = (event) => {
      const msg = JSON.parse(event.data)
      console.log(msg.payload)
      if (msg.success === false) { 
        alert(`You got error ${msg.payload.error}`)
      } else if(msg.action === 'login' || msg.action === 'incoming_event') {
        this.loadChatList()
      } else if(msg.action === 'list_chats') {
        this.loading = false
        this.totalChat = msg.payload.found_chats
        this.chatList = msg.payload.chats_summary
        // console.log(msg.payload)        
      } else if (msg.action === 'list_agents_for_transfer') { 
        this.agentList = msg.payload
        // console.log(this.agentList)
      }else {
        console.log(msg.action)
        console.log(msg.action.payload)
      }
    }

    this.ws.onopen = (event) => {
      this.login(accessToken)
    }
  },
  methods: { 
    login(token) {
      this.ws.send(JSON.stringify({
        action: "login",
        payload: {
          token: `Bearer ${token}`
        }
      }))
    },

    loadChatList() {
      this.ws.send(JSON.stringify({
        action: "list_chats",
        payload: {}
      }))
    },

    setSelectedChat(chat) {
      // console.log(chat.id)
      this.transferChat(chat.id)
    },

    transferChat(chatId) {
      this.ws.send(JSON.stringify({
        action: "transfer_chat",
        payload: {
          id: chatId,
          target: {
            type: "group",
            ids: [1]
          }
        }
      }))

      // alert('transfer success')
    },

    assignChat() {

    },

    myDateFormat(d) {
      const createdAt = new Date(d);
      return `${createdAt.getDate()} ${createdAt.toLocaleString('default', { month: 'long' })}`;
    },
  }
}
</script>