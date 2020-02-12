<template>
  <div
    class="launch-button"
    :class="{ opened: isOpen, closed: !isOpen }"
    @click.prevent="isOpen ? close() : open()"
  >
    <BubbleIcon v-if="!isOpen" class="launch-button__open-icon" />
    <XIcon v-if="isOpen" class="launch-button__close-icon" />
  </div>
</template>
<script>
import BubbleIcon from './chatbot/icons/bubble'
import XIcon from './chatbot/icons/x'

export default {
  components: {
    BubbleIcon,
    XIcon
  },
  props: {
    isOpen: {
      type: Boolean,
      required: true
    },
    open: {
      type: Function,
      required: true
    },
    close: {
      type: Function,
      required: true
    }
  }
}
</script>
<style scoped>
.launch-button {
  z-index: 999;
  background-color: #4e8cff;
  width: 60px;
  height: 60px;
  background-position: center;
  background-repeat: no-repeat;
  position: fixed;
  right: 25px;
  bottom: 25px;
  border-radius: 50%;
  box-shadow: none;
  transition: box-shadow 0.2s ease-in-out;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
}

.launch-button__open-icon {
  height: 26px;
  width: 26px;
  color: #ffffff;
}

.launch-button__close-icon {
  stroke-width: 1;
  height: 32px;
  width: 32px;
  color: #ffffff;
}

.launch-button:hover {
  box-shadow: 0 0px 27px 1.5px rgba(0, 0, 0, 0.2);
}

@media (max-width: 450px) {
  .launch-button.opened {
    display: none;
  }
}
</style>
