<template>
  <div class="w-full h-screen">
    
    <div v-if="showLiveChat && !loading" class="flex h-full">
      <div class="flex-2 h-full overflow-auto px-8">
        <div class="hidden lg:block heading flex-2">
          <h1 class="text-xl text-gray-700 mb-4">Conversation List ({{ totalChat }})</h1>
        </div>
        
        <div v-for="chat in chatList" :key="chat.id" class="entry cursor-pointer transform hover:scale-105 duration-300 transition-transform bg-white mb-4 rounded p-4 flex shadow-md border-l-4" :class="chat === selectedChat ? 'border-red-500' : ''" @click="setSelectedChat(chat)">
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

      <div class="flex-1 flex flex-col">
        <div v-if="showMessageBox" class="flex-3" >
          
          <h2 class="text-xl py-1 mb-8 border-b-2 border-gray-200">
            Chatting with 
            <b>{{ currentUser }}</b>
          </h2>

          <div class="overflow-y-auto h-96">
            <div v-for="msg in messageList" :key="msg.id">
              <div  v-if="msg.type === 'message'" class="flex-1">
                <div v-if="msg.author_id === '0x445.eth@gmail.com'" class="message me mb-4 flex text-right" >
                  <div class="flex-1 px-2">
                      <div class="inline-block bg-blue-600 rounded-full p-2 px-6 text-white">
                          <span>{{ msg.text }}</span>
                      </div>
                      <div class="pr-4"><small class="text-gray-500">{{ myDateFormat(msg.created_at) }}</small></div>
                  </div>
                </div>
                <div v-else class="message mb-4 flex">
                  <div class="flex-2">
                      <div class="w-12 h-12 relative">
                          <img class="w-12 h-12 rounded-full mx-auto" src="https://dummyimage.com/80x80" alt="chat-user" />
                          <span class="absolute w-4 h-4 bg-gray-400 rounded-full right-0 bottom-0 border-2 border-white"></span>
                      </div>
                  </div>
                  <div class="flex-1 px-2">
                      <div class="inline-block bg-gray-300 rounded-full p-2 px-6 text-gray-700">
                          <span>{{ msg.text }}</span>
                      </div>
                      <div class="pl-4"><small class="text-gray-500">{{ myDateFormat(msg.created_at) }}</small></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
         

          <div class="flex-2 pt-4 pb-10">
            <div class="write bg-white shadow flex rounded-lg">
              <div class="flex-3 flex content-center items-center text-center p-4 pr-0">
                <span class="block text-center text-gray-400 hover:text-gray-800">
                    <svg fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" stroke="currentColor" viewBox="0 0 24 24" class="h-6 w-6"><path d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                </span>
              </div>
              <div class="flex-1">
                <textarea v-model="message" name="message" class="w-full block outline-none py-4 px-4 bg-transparent" rows="1"  placeholder="Type a message..." autofocus></textarea>
              </div>
              <div class="flex-2 w-32 p-2 flex content-center items-center">
                <div class="flex-1 text-center">
                  <span class="text-gray-400 hover:text-gray-800">
                    <span class="inline-block align-text-bottom">
                      <svg fill="none" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" stroke="currentColor" viewBox="0 0 24 24" class="w-6 h-6"><path d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13"></path></svg>
                    </span>
                  </span>
                </div>
                <div class="flex-1">
                  <button class="bg-blue-400 hover:bg-blue-500 w-10 h-10 rounded-full inline-block" @click="sendMessage()">
                    <span class="inline-block align-text-bottom">
                      <svg fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" viewBox="0 0 24 24" class="w-4 h-4 text-white"><path d="M5 13l4 4L19 7"></path></svg>
                    </span>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-else>Please select a chat . . .</div>
      </div>
    </div>
    
    <div v-if="loading">
      <h5>
        Loading please wait . . .
      </h5>

      <div v-if="!showLiveChat" class="text-center">
        <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"  @click="showLiveChatPopup = !showLiveChatPopup">
          Login LiveChat
        </button>
      </div>
    </div>
    
    
    <login-live-chat v-model="showLiveChatPopup" />
  </div>
  
</template>


<script>
// import { apiUrl } from '@/services/api'
import LoginLiveChat from '@/components/LoginLiveChat.vue'

export default {
  name: 'LiveChatPage',
  components: {
    LoginLiveChat
  },
  layout: 'BackOffice',
  data() {
    return {
      ws: null,
      chatList: [],
      messageList: [],
      selectedChat: {},
      message: '',
      totalChat: 0,
      currentUser: '',
      showLiveChat: false,
      showLiveChatPopup: false,
      showMessageBox: false,
      loading: true
    }
  },
  created() {
    const accessToken = localStorage.getItem('live_chat_access_token');

    this.ws =  new WebSocket("wss://api.livechatinc.com/v3.4/agent/rtm/ws")

    this.ws.onmessage = (event) => {
      // console.log(event);
      const msg = JSON.parse(event.data)
      console.log(msg)
      if (msg.success === false) { 
        alert(`You got error ${msg.payload.error}`)
        // console.log(msg.payload.error)
      } else if(msg.action === 'login' || msg.action === 'incoming_event') {
        this.loadChatList()
      } else if(msg.action === 'list_chats') {
        this.loading = false
        this.showLiveChat = true
        this.totalChat = msg.payload.found_chats
        this.chatList = msg.payload.chats_summary
        // console.log(msg.payload)        
      } else if(msg.action === 'get_chat') {
        this.messageList = msg.payload.thread.events
        // console.log(msg.payload)        
      } else if(msg.action === 'incoming_sneak_peek' && this.currentUser === '') {
        this.loadChatMessage()
        // console.log(msg.action) 
        // console.log() 
      } else {
        console.log(msg.action)
      }
    }

    this.ws.onopen = (event) => {
      // console.log(event)
      // console.log("Successfully connected to the echo websocket server...")
      this.login(accessToken)
    }
    
  },
  methods: {
    login(token) {
      // login user
      this.ws.send(JSON.stringify({
        action: "login",
        payload: {
          token: `Bearer ${token}`
        }
      }))
    },
    loadChatList() {
      // load chat list
      this.ws.send(JSON.stringify({
        action: "list_chats",
        payload: { }
      }))
    },
    setSelectedChat(chat) {
      this.selectedChat = chat
      this.currentUser = chat.last_event_per_type.filled_form.event.fields[0].answer
      this.showMessageBox = true
      this.loadChatMessage()
    },
    loadChatMessage() {
      this.ws.send(JSON.stringify({
        action: "get_chat",
        payload: { 
          chat_id: this.selectedChat.id,
        }
      }))
    },
    sendMessage() {
      if (this.message === '') {
        alert('plese write a message')
      } else {
        this.ws.send(JSON.stringify({
          action: "send_event",
          payload: {
            chat_id: this.selectedChat.id, 
            event: {
              type: "message",
              text: this.message
            }
          }
        }))
        this.message = ''
      }
    },
    myDateFormat(d) {
      const createdAt = new Date(d);
      return `${createdAt.getDate()} ${createdAt.toLocaleString('default', { month: 'long' })}`;
    },
  }
}
</script>
