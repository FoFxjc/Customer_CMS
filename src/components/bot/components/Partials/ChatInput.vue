<template>
  <div class="bottomchat">
    <div class="input-container">
      <!-- Here are the suggestions -->
      <div class="suggestions"><slot /></div>
      <div class="flexible">
        <!-- Text input -->
        <input
          v-model="query"
          class="input"
          type="text"
          :placeholder="
            (botconfig.i18n[lang()] && botconfig.i18n[lang()].inputTitle) ||
              botconfig.i18n[botconfig.app.fallback_lang].inputTitle
          "
          :aria-label="
            (botconfig.i18n[lang()] && botconfig.i18n[lang()].inputTitle) ||
              botconfig.i18n[botconfig.app.fallback_lang].inputTitle
          "
          @keypress.enter="submit()"
        />

        <!-- Send message button (arrow button) -->
        <button
          v-if="(!micro && query.length > 0) || !recognition"
          class="button"
          :title="
            (botconfig.i18n[lang()] && botconfig.i18n[lang()].sendTitle) ||
              botconfig.i18n[botconfig.app.fallback_lang].sendTitle
          "
          :aria-label="
            (botconfig.i18n[lang()] && botconfig.i18n[lang()].sendTitle) ||
              botconfig.i18n[botconfig.app.fallback_lang].sendTitle
          "
          @click="submit()"
        >
          <i class="material-icons" aria-hidden="true">arrow_upward</i>
        </button>

        <!-- Microphone Button -->
        <button
          v-else
          class="button"
          :aria-label="
            (botconfig.i18n[lang()] && botconfig.i18n[lang()].microphoneTitle) ||
              botconfig.i18n[botconfig.app.fallback_lang].microphoneTitle
          "
          :title="
            (botconfig.i18n[lang()] && botconfig.i18n[lang()].microphoneTitle) ||
              botconfig.i18n[botconfig.app.fallback_lang].microphoneTitle
          "
          :class="{ mic_active: micro }"
          @click="micro = !micro"
        >
          <i class="material-icons" aria-hidden="true">mic</i>
        </button>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '../chatbot/Mixins.sass';

.bottomchat {
  position: absolute;
  bottom: 0;
  width: 100%;
  background-color: white;
}

.input-container {
  max-width: 450px;
  margin-bottom: 2px;
  margin-left: auto;
  margin-right: auto;
}

.flexible {
  display: flex;
}

.suggestions {
  overflow-x: scroll;
  overflow-y: hidden;
  white-space: nowrap;
  -webkit-overflow-scrolling: touch;

  &::-webkit-scrollbar {
    display: none;
  }
}

.input {
  font-size: 16px;
  font-weight: 500;
  width: 100%;
  box-sizing: border-box;
  border: none;
  padding: 10px 12px;
  color: var(--text);
  border-radius: 40px;
  flex: 1 0 0;
  background-color: #f1f3f4;

  &:focus {
    background-color: transparent;
  }
}

.button {
  @include reset;
  padding: 8px;
  margin-left: 6px;
  border-radius: 50%;
  cursor: pointer;
  background-color: #f1f3f4;
  color: #202124;
  font-size: 24px;
  display: flex;

  &.mic_active {
    background-color: #f44336;
    color: white;
  }
}
</style>

<script>
export default {
  name: 'ChatInput',
  data() {
    return {
      query: '',
      micro: false,
      recognition: null,
    }
  },
  watch: {
    /* This function triggers when user clicks on the microphone button */
    micro(activate) {
      if (activate) {
        /* When value is true, reset the language & start voice recognition */
        this.recognition.lang = this.lang()
        this.recognition.start()
        this.recognition.onresult = event => {
          for (let i = event.resultIndex; i < event.results.length; ++i) {
            this.query = event.results[i][0].transcript // <- push results to the Text input
          }
        }

        this.recognition.onend = () => {
          this.recognition.stop()
          this.micro = false
          this.submit(this.query) // <- submit the result
        }
      } else {
        this.recognition.abort() // <- if user stops the recognition, abort it (in V1 this prevented users from starting a new recording)
      }
    },
  },
  created() {
    if (window.webkitSpeechRecognition || window.SpeechRecognition) {
      this.recognition = new window.webkitSpeechRecognition() || new window.SpeechRecognition()
      this.recognition.interimResults = true
    }
  },
  methods: {
    submit() {
      if (this.query.length > 0) {
        this.$emit('submit', this.query)
        this.query = ''
      }
    },
  },
}
</script>
