<template>
  <div class="shape">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: ["widget"],
};
</script>

<style lang="scss" scoped>
.shape {
  position: relative;
}
</style>