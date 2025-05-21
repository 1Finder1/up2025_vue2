<script>
import Modal from "@/components/modal.vue";

export default {
  components: {Modal},
  props: {
    hero: {
      type: Object,
      required: false,
    }
  },

  computed: {
    infoFields() {
      return Object.entries(this.hero).filter(([key]) => !['name', 'image', 'id'].includes(key)).map(([key, value]) => {
        let prettyValue = ''
        if (typeof value === 'string' || typeof value === 'number') prettyValue = value
        else if (Array.isArray(value)) prettyValue = value.join(', ')
        else if (typeof value === 'object') {
          if (value?.wood) prettyValue = `${value.wood} - ${value.core} - ${value.length} inches`
        } else if (typeof value === 'boolean')
          prettyValue = value? '+': '-'
        return {key, value: prettyValue}
      });
    }
  }
}
</script>

<template>
  <Modal :is-open="!!hero" @update:isOpen="$emit('update:hero', null)">
    <div class="hero-modal">
      <div>
        <h2>{{ hero.name }}</h2>
        <img :src="hero.image" alt="hero image"/>
      </div>

      <div class="hero-info">
          <p v-for="field in infoFields" :key="field.key">
            <strong>{{ field.key }}:</strong> {{ field.value }}
          </p>
      </div>
    </div>

  </Modal>
</template>

<style scoped>
.hero-modal {
  display: flex;
  justify-content: center;
  gap: .75rem;

  color: #fff;

  border-radius: .5rem;
  padding: 1rem;

  img {
    width: 100%;
    max-width: 300px;
    height: auto;
    border-radius: .5rem;
  }

  .hero-info {
    padding-top: 1.5em;
  }
}
</style>