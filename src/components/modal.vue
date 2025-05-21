<script>
export default {
  props: {
    isOpen: {
      type: Boolean,
      default: false,
    }
  }
}
</script>

<template>
  <div class="modal-wrapper" v-if="isOpen" @click="$emit('update:isOpen', false)">
    <div class="modal" @click.stop>
      <slot></slot>
    </div>
  </div>

</template>

<style scoped>

.modal-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;

  position: fixed;
  inset: 0;

  width: 100%;
  height: 100vh;

  background: rgba(0, 0, 0, 0.3);
  border: none;
}
.modal {
  background: #462E1D;

  border-radius: 1rem;
}

</style>