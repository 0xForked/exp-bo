<template>
  <div v-if="open">
    <slot />
  </div>
</template>

<script>

export default {
  name: 'LoginLiveChat',
  model: {
    prop: 'open',
    event: 'close'
  },
  props: {
    open: {
      type: Boolean,
      default: false,
    }
  },
  data() {
    return {
      windowRef: null,
    }
  },
  watch: {
    open(newOpen) {
      if(newOpen) {
        this.openPortal();
      } else {
        this.closePortal();
      }
    }
  },
  mounted() {
    if(this.open) {
      this.openPortal();
    }
  },
  beforeDestroy() {
    if (this.windowRef) {
      this.closePortal();
    }
  },
  methods: {
    openPortal() {
      this.windowRef = window.open("https://accounts.livechat.com/?response_type=token&client_id=775d8e253b263413d3b2ec6d4ad72208&redirect_uri=http://localhost:3001/redirect", "", "width=600,height=400,left=200,top=200");
      this.windowRef.document.body.appendChild(this.$el);
      this.windowRef.addEventListener('beforeunload', this.closePortal);
    },
    closePortal() {
      if(this.windowRef) {
        this.windowRef.close();
        this.windowRef = null;
        this.$emit('close');
        window.location.reload()
      }
    }
  }
}
</script>