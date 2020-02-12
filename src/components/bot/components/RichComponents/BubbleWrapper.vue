<template>
  <div class="bubble-wrapper">
    <slot />
  </div>
</template>

<style lang="scss" scoped>
.bubble-wrapper{
    display: flex;
    justify-content: flex-end;
}
</style>

<script>
export default {
  name: 'BubbleWrapper'
}
</script>
