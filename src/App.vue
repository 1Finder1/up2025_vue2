<script>
import HeroList from "@/components/heroList.vue";
import Input from "@/UIComponents/input.vue";
import Select from '@/UIComponents/select.vue'
import Button from "@/UIComponents/button.vue";
import DetailHeroModal from "@/components/detailHeroModal.vue";
import RegisterModal from "@/components/registerModal.vue";
import LoginModal from "@/components/loginModal.vue";

export default {
  components: {LoginModal, RegisterModal, DetailHeroModal, Input, HeroList, Select, Button},
  data() {
    return {
      heroes: [],
      sortMode: '',
      search: '',
      sortOptions: [
        {id: 'name', label: 'По имени'},
        {id: 'nameDesc', label: 'По имени обратно'},
        {id: 'eyeColour', label: 'По цвету глаз'},
        {id: 'eyeColourDesc', label: 'По цвету глаз обратно'},
        {id: 'house', label: 'По дому'},
        {id: 'houseDesc', label: 'По дому обратно'},
      ],
      actorFilterValue: '',
      hogwartsStaffFilterValue: '',
      hogwartsStaffFilterOption: [
        {id: 'true', label: 'Сотрудник'},
        {id: 'false', label: 'Не сотрудник'},
      ],
      selectedHero: null,
      registerOpen: false,
      loginOpen: false,
      currentUser: null,
    }
  },

  methods: {
    async fetchHeroes() {
      const res = await fetch('https://hp-api.onrender.com/api/characters')
      this.heroes = await res.json()
    },
    resetFilters() {
      this.search = ''
      this.sortMode = ''
      this.actorFilterValue = ''
      this.hogwartsStaffFilterValue = ''
    },
    registerUser(user) {
      const userNew = {...user, favorite: []}
      localStorage.setItem(user.login, JSON.stringify(userNew))
      this.registerOpen = false
    },
    loginUser(user) {
      console.log(user)
      const userData = JSON.parse(localStorage.getItem(user.login))
      console.log(userData)
      if (!userData || userData.password !== user.password) return alert('Логин или пароль введены неверно')
      this.currentUser = userData
      this.loginOpen = false
    },

    toggleFavorite(id) {
      if (!this.currentUser) return alert('Сначала войдите в систему')
      if (this.currentUser.favorite.includes(id)) {
        this.currentUser.favorite = this.currentUser.favorite.filter(item => item !== id)
      } else {
        this.currentUser.favorite.push(id)
      }
      localStorage.setItem(this.currentUser.login, JSON.stringify(this.currentUser))
    }
  },

  computed: {
    actorFilterOption() {
      return this.sortedHeroes.filter(item => item.actor).map(hero => {
        return {id: hero.actor, label: hero.actor}
      })
    },
    //
    filteredByStaff() {
      if (!this.hogwartsStaffFilterValue) return this.heroes
      return this.heroes.filter(item => {
        if (this.hogwartsStaffFilterValue === 'true') return item.hogwartsStaff
        return !item.hogwartsStaff
      })
    },
    filteredByActor() {
      if (!this.actorFilterValue) return this.filteredByStaff
      return this.filteredByStaff.filter(item => item.actor === this.actorFilterValue)
    },
    filteredBySearch() {
      if (!this.search) return this.filteredByActor

      return this.filteredByActor.filter(item =>
          item.name.toLowerCase().includes(this.search.toLowerCase()) ||
          item.house.toLowerCase().includes(this.search.toLowerCase()) ||
          item.alternate_names.find(aname => aname.toLowerCase().includes(this.search.toLowerCase()))
      )
    },
    sortedHeroes() {
      if (!this.sortMode) return this.filteredBySearch

      const isDesc = this.sortMode.includes('Desc')
      const key = this.sortMode.replace('Desc', '')

      return this.filteredBySearch.toSorted((c1, c2) => {
        if (isDesc) return c2[key].localeCompare(c1[key])
        return c1[key].localeCompare(c2[key])
      })
    },
    //
    favoriteHeroes() {
      if (!this.currentUser) return []
      return this.heroes.filter(item => this.currentUser.favorite.includes(item.id))
    },
    favoriteWizardCount() {
      if (!this.currentUser) return 0
      return this.favoriteHeroes.filter(item => item.wizard).length
    },
    favoriteNoWizardCount() {
      if (!this.currentUser) return 0
      return this.favoriteHeroes.filter(item => !item.wizard).length
    }
  },

  mounted() {
    this.fetchHeroes()
  }
}
</script>

<template>
  <div class="control">
    <Input v-model:value="search" label="Поиск по имени, дому и альтернативным именам"/>
    <Select :options="sortOptions" v-model:current-value="sortMode" label="Сортировать по:"></Select>
    <Select :options="hogwartsStaffFilterOption" v-model:current-value="hogwartsStaffFilterValue"
            label="Сотрудник?"></Select>
    <Select :options="actorFilterOption" v-model:current-value="actorFilterValue" label="Актёр"></Select>
    <Button @click="resetFilters">Сбросить</Button>
    <div v-if="!currentUser" class="auth_block">
      <Button @click="registerOpen=true">Регистрация</Button>
      <Button @click="loginOpen=true">Войти</Button>
    </div>

    <div class="favorite_stat__wrapper" v-if="!!currentUser">
      <p>Избранное:</p>
      <div class="favorite_stat">
        <p>Волшебники: {{favoriteWizardCount}}</p>
        <p>Не волшебники: {{favoriteNoWizardCount}}</p>
      </div>
    </div>
  </div>
  <hero-list :heroes="sortedHeroes" @openHero="hero => this.selectedHero=hero"
             :favorite-heroes="this.currentUser?.favorite" @toggleFavorite="toggleFavorite"/>
  <DetailHeroModal v-model:hero="selectedHero"/>
  <RegisterModal v-model:is-open="registerOpen" @register="registerUser"/>
  <LoginModal v-model:is-open="loginOpen" @try_login="loginUser"/>
</template>

<style scoped>
.control {
  display: flex;
  justify-content: center;
  gap: .75rem;

  padding: 1rem;

  background: #2e280d;
  color: #fff;

  margin-bottom: 1rem;
}

.auth_block {
  display: flex;
  gap: .5rem;
  align-self: end;
}

.favorite_stat__wrapper {
  display: flex;
  gap: .5rem;
  align-self: end;

  .favorite_stat {
    display: flex;
    flex-direction: column;
  }
}
</style>
