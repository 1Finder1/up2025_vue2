<script>
export default {
  props: {
    currentValue: String,
    options: {
      type: Array,
      default: []
    },
    label: {
      type: String,
      default: ""
    }
  }
}
</script>

<template>
  <div class="form-group">
    <label v-if="label">{{ label }}</label>
    <select :value="currentValue" @change="$emit('update:currentValue', $event.target.value)">
      <option value="">Ничего не выбрано</option>
      <option :value="option.id" v-for="option in options">{{ option.label }}</option>
    </select>
  </div>

</template>

<style scoped>
select {
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