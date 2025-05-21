<script>
import Button from "@/UIComponents/button.vue";

export default {
  components: {Button},
  props: {
    hero: Object,
    isFavorite: {
      type: Boolean,
      default: false
    },
    favoritable: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    image() {
      if (!this.hero.image) return 'https://ik.imagekit.io/hpapi/harry.jpg'
      return this.hero.image
    },
    species() {
      return this.hero.species[0].toUpperCase() + this.hero.species.slice(1)
    },
    wizard() {
      return this.hero.wizard ? 'Волшебник' : 'Нормис'
    },
    years() {
      if (!this.hero.dateOfBirth) return '???'
      const formattedDate = this.hero.dateOfBirth.split('-').toReversed().join('-')
      const birthday = +new Date(formattedDate)
      const now = +new Date()

      return Math.floor((now - birthday) / 1000 / 60 / 60 / 24 / 365)
    }
  }
}

</script>

<template>
  <div class="hero-card"
       :style="{'background': `linear-gradient(to top, rgba(0,0,0,.3), rgba(0,0,0,.3)), url(${image})`, 'background-position': 'center top', 'background-size': 'cover'}">
    <div class="">
      <h2>{{ hero.name }}, {{ wizard }}</h2>
      <p>{{ species }} {{ this.hero.gender }}</p>
      <p>{{ this.hero.dateOfBirth ?? '???' }}, {{ years }} лет</p>
      <p><strong>Дом: </strong>{{ this.hero.house }}</p>
      <p><strong>Предки: </strong>{{ this.hero.ancestry }}</p>
    </div>
    <Button v-if="favoritable" class="favorite_button" @click.stop="$emit('toggleFavorite', this.hero.id)">
      {{ this.isFavorite ? 'Удалить из избранного' : 'Добавить в избранное' }}
    </Button>
  </div>
</template>

<style scoped>
.hero-card {
  position: relative;
  display: flex;
  flex-direction: column-reverse;

  width: 400px;
  height: 400px;

  background-position: center center;
  background-size: contain;

  color: white;

  border-radius: .5rem;
  padding: .5rem;

  cursor: pointer;

  transition: .3s;
}

.hero-card:hover {
  transform: scale(1.02);
}

.favorite_button {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
}
</style>