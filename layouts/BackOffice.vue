<template>
  <div class="min-h-full">
    <nav class="bg-gray-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div class="flex items-center justify-between h-16">
            <div class="flex items-center">
              <div class="flex-shrink-0">
                <img
                  class="h-8 w-8"
                  src="https://tailwindui.com/img/logos/workflow-mark-indigo-400.svg"
                  alt="Workflow logo"
                >
              </div>
              <div class="hidden md:block">
                <div class="ml-10 flex items-baseline">
                  <a
                    v-for="item in navigation"
                    :key="item.name"
                    :href="item.href"
                    :class="[(item.href === $route.path) ? 'bg-gray-900 text-white' : 'text-gray-300 hover:bg-gray-700 hover:text-white', 'px-3 py-2 rounded-md text-sm font-medium']" 
                    :aria-current="(item.href === $route.path) ? 'page' : undefined"
                  >{{ item.name }}</a>
                </div>
              </div>
            </div>
            <div class="hidden md:block">
              <div class="ml-4 flex items-center md:ml-6">
                <!-- Profile dropdown -->
                <div class="ml-3 relative">
                  <div>
                    <button
                      id="user-menu"
                      class="max-w-xs flex items-center text-sm rounded-full text-white focus:outline-none focus:shadow-solid"
                      aria-label="User menu"
                      aria-haspopup="true"
                      @click="toggle"
                    >
                      <img
                        class="h-8 w-8 rounded-full"
                        src="https://dummyimage.com/80x80"
                        alt
                      />
                      <div class="flex flex-col text-left ml-3 text-xs">
                        <p> {{ user.name }} </p>
                        <p> {{ user.email }} </p>
                      </div>
                    </button>
                  </div>
                  <transition
                    enter-active-class="transition ease-out duration-100"
                    enter-class="transform opacity-0 scale-95"
                    enter-to-class="transform opacity-100 scale-100"
                    leave-active-class="transition ease-in duration-75"
                    leave-class="transform opacity-100 scale-100"
                    leave-to-class="transform opacity-0 scale-95"
                  >
                    <div
                      v-show="isOpen"
                      class="origin-top-right absolute right-0 mt-2 w-48 rounded-md shadow-lg"
                    >
                      <div
                        class="py-1 rounded-md bg-white shadow-xs"
                        role="menu"
                        aria-orientation="vertical"
                        aria-labelledby="user-menu"
                      >
                        <a
                          v-for="item in userNavigation" 
                          :key="item.name"
                          href="#" 
                          class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100"
                          role="menuitem"
                          @click="userNavSelection(item.name)"
                        >{{ item.name }}</a>
                      </div>
                    </div>
                  </transition>
                </div>
              </div>
            </div>
            <div class="-mr-2 flex md:hidden">
              <!-- Mobile menu button -->
              <button
                class="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-white hover:bg-gray-700 focus:outline-none focus:bg-gray-700 focus:text-white"
                @click="toggle"
              >
                <svg
                  :class="[isOpen ? 'hidden' : 'block', 'h-6 w-6']"
                  stroke="currentColor"
                  fill="none"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M4 6h16M4 12h16M4 18h16"
                  ></path>
                </svg>
                <svg
                  :class="[isOpen ? 'block' : 'hidden', 'h-6 w-6']"
                  stroke="currentColor"
                  fill="none"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M6 18L18 6M6 6l12 12"
                  ></path>
                </svg>
              </button>
            </div>
          </div>
        </div>
        <div :class="[isOpen ? '' : 'hidden', 'md:hidden']">
          <div class="px-2 pt-2 pb-3 sm:px-3">
            <a
              v-for="item in navigation" 
              :key="item.name" 
              as="a" 
              :href="item.href" 
              :class="[(item.href === $route.path) ? 'bg-gray-900 text-white' : 'text-gray-300 hover:bg-gray-700 hover:text-white', 'block px-3 py-2 rounded-md text-base font-medium']" 
              :aria-current="(item.href === $route.path) ? 'page' : undefined"
            >
              {{ item.name }}
            </a>
            
          </div>
          <div class="pt-4 pb-3 border-t border-gray-700">
            <div class="flex items-center px-5">
              <div class="flex-shrink-0">
                <img
                  class="h-10 w-10 rounded-full"
                  src="https://dummyimage.com/80x80"
                  alt
                >
              </div>
              <div class="ml-3">
                <div class="text-base font-medium leading-none text-white">{{ user.name }}</div>
                <div class="mt-1 text-sm font-medium leading-none text-gray-400">{{ user.email }}</div>
              </div>
            </div>
            <div class="mt-3 px-2">
              <a
                v-for="item in userNavigation" 
                :key="item.name" 
                as="a" 
                :href="item.href" 
                class="block px-3 py-2 rounded-md text-base font-medium text-gray-400 hover:text-white hover:bg-gray-700 focus:outline-none focus:text-white focus:bg-gray-700"
              >{{ item.name }}</a>
            </div>
          </div>
        </div>
    </nav>

    <header class="bg-white shadow">
      <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
        <h1 class="text-3xl font-bold leading-tight text-gray-900">{{ $route.path }}</h1>
      </div>
    </header>
    
    <main>
      <div class="max-w-7xl mx-auto py-6 sm:px-6 lg:px-8">
        <div class="px-4 py-6 sm:px-0">
          <Nuxt />
        </div>
      </div>
    </main>
  </div>
</template>
// 
<script>
const user = {
  name: 'Tom Cook my',
  email: 'tom@example.com',
  imageUrl:
    'https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80',
}

const navigation = [
  { name: 'Home', href: '/p/home' },
  { name: 'FreshChat', href: '/p/freshchat' },
  { name: 'LiveChat', href: '/p/livechat' },
  { name: 'Twak', href: '/p/twak' },
]

const userNavigation = [
  // { name: 'Your Profile', href: '#' },
  // { name: 'Settings', href: '#' },
  { name: 'Sign out', href: '#' },
]

export default {
  name: 'BackOffice',
  components: { },
  middleware({ route, redirect, from }) {
    if(! localStorage.is_logged_in){
     return redirect("/")
    }
  },
  data() {
    return {
      user,
      navigation,
      userNavigation,
      isOpen: false
    }
  },
  created() {
    this.user = JSON.parse(localStorage.getItem('user'))
  },
  methods: {
    toggle () {
      this.isOpen = !this.isOpen
    },
    userNavSelection(type) {
      if (type === 'Sign out') {
        this.signOut()
      }
    },
    signOut () {
      localStorage.removeItem('is_logged_in')
      localStorage.removeItem('user')
      this.$router.push('/')
    }
  }
}
</script>