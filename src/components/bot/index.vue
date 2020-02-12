<template>
  <div>
    <div v-if="isChatOpen" class="chat-window">
      <ChatBot />
    </div>
    <LaunchButton :open="openChat" :close="closeChat" :is-open="isChatOpen" />
  </div>
</template>

<script>
import LaunchButton from './components/LaunchButton'
import ChatBot from './components/chatbot'

export default {
  name: 'Bot',
  components: { LaunchButton, ChatBot },
  props: {
    // serviceName: {
    //   type: String,
    //   required: true,
    // },
    // closeTieSessionOnExit: {
    //   type: String,
    //   required: false,
    // },
  },
  data() {
    return {
      titleImageUrl: 'https://a.slack-edge.com/66f9/img/avatars-teams/ava_0001-34.png',
      isChatOpen: false,
    }
  },
  methods: {
    openChat() {
      this.isChatOpen = true
    },
    closeChat() {
      this.isChatOpen = false
      localStorage.removeItem('message_history')
      //   if (this.closeTieSessionOnExit === 'true') {
      //     this.$teneoApi.closeSession();
      //   }
    },
  },
}
</script>
<style scope>
.chat-window {
  z-index: 999;
  font-family: 'Helvetica Neue', Helvetica, Arial, sans-serif;
  width: 370px;
  height: calc(100% - 120px);
  max-height: 590px;
  position: fixed;
  right: 25px;
  bottom: 100px;
  box-sizing: border-box;
  box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.15);
  background: white;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition: 0.3s ease-in-out;
  border-radius: 10px;
}

@media (max-width: 450px) {
  .chat-window {
    width: 100%;
    height: 100%;
    max-height: 100%;
    right: 0px;
    bottom: 0px;
    border-radius: 0px;
  }
}
</style>
