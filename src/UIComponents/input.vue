<script>
export default {
  props: {
    value: String,
    type: {
      type: String,
      default:'text'
    },
    label: {
      type: String,
      required: false
    }
  }
}
</script>

<template>
  <div class="form-group">
    <label v-if="!!label">{{label}}</label>
    <input :type="type" :value="value" @input="$emit('update:value', $event.target.value)">
  </div>
</template>

<style scoped>
input {
  padding: .5rem .75rem;
  border-radius: .5rem;
  outline: none;
  border: 1px solid black;
  background: #fff;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: .25rem;

  color: #fff;
}
</style>