<template>
  <div class="app-head">
    <img
      v-if="app.avatarUri"
      class="app-icon"
      :alt="app.displayName"
      :src="'https://storage.googleapis.com/cloudprod-apiai/' + app.avatarUri"
    />
    <img v-else class="app-icon" src="@/assets/img/bot-logo.png" :alt="app.displayName" />
    <div class="app-info">
      <div class="app-name">Food Bot</div>
      <!-- <div class="app-poweredby">
        {{ app.displayName }} Built with
        <a target="_blank" aria-hidden="true">Food Bot</a>
      </div> -->
    </div>
    <slot />
  </div>
</template>

<style lang="scss" scoped>
.app-head {
  z-index: 999;
  padding: 16px;
  width: 100%;
  text-align: left;
  display: flex;
  background-color: #4e8cff;

  .app-icon {
    width: 35px;
    height: 35px;
    object-fit: cover;
    margin-top: -5px;
    margin-bottom: -5px;
    padding: 8px 8px 8px 8px;
    background-color: rgba(204, 204, 204, 0.25);
    border-radius: 50%;
  }

  .app-info {
    padding: 5px 0px;
    margin-left: 10px;
    height: 100%;

    .app-name {
      font-size: 25px;
      font-weight: 400;
      color: #202124;
    }

    .app-poweredby {
      color: #5f6368;
      font-size: 14px;

      a[href] {
        color: #202124;
        text-decoration: none;
      }
    }
  }
}

.audio-toggle {
  display: flex;
  position: absolute;
  top: 0;
  right: 45px;
  margin: 15px 0px;
  z-index: 999;
  padding: 8px 8px 8px 8px;
  background-color: rgba(204, 204, 204, 0.25);
  border-radius: 50%;
  cursor: pointer;
  color: inherit;
  transition: padding 0.25s ease;
  @media screen and (min-width: 720px) {
    right: 0;
  }
  i {
    padding: 1px;
  }
}

.close-toggle {
  display: flex;
  position: absolute;
  top: 0;
  right: 5px;
  margin: 15px 0;
  z-index: 999;
  padding: 8px 8px 8px 8px;
  background-color: rgba(204, 204, 204, 0.25);
  border-radius: 50%;
  border-radius: 20px 20px;
  cursor: pointer;
  color: white;
  transition: padding 0.25s ease;

  @media screen and (min-width: 720px) {
    display: none;
  }
  i {
    padding: 1px;
  }
}
</style>

<script>
export default {
  name: 'TopHead',
  props: {
    app: {
      type: Object,
      default: null,
    },
  },
}
</script>
