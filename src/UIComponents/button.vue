<script>
export default {
  props: {
    'class': {
      type: String,
      default: ''
    }
  }
}
</script>

<template>
<button :class="this.class">
  <slot></slot>
</button>
</template>

<style scoped>
button {
  padding: .5rem .75rem;
  border-radius: .5rem;
  outline: none;

  border: none;
  background: #eee;
}
</style>