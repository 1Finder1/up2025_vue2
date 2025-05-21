<script>
import HeroCard from "@/components/heroCard.vue";

export default {
  components: {HeroCard},
  props: {
    heroes: Array,
    favoriteHeroes: {
      required: false,
      type: Array,
      default: null
    }
  },
  computed: {
    canFavorite() {
      return this.favoriteHeroes !== null
    }
  }
}
</script>

<template>
  <div class="hero-list">
    <hero-card v-for="hero in heroes" :hero="hero" @click="$emit('openHero', hero)"
               @toggleFavorite="$emit('toggleFavorite', hero.id)" :is-favorite="favoriteHeroes?.includes(hero.id)"
               :favoritable="canFavorite"

    />
  </div>
</template>

<style scoped>
.hero-list {
  display: grid;
  grid-template-columns: repeat(3, 400px);
  gap: 1rem;

  width: fit-content;

  margin: auto;
}

</style>