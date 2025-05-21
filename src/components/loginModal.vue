<script>
import Modal from "@/components/modal.vue";
import Input from "@/UIComponents/input.vue";
import Button from "@/UIComponents/button.vue";

export default {
  components: {Button, Input, Modal},
  props: {
    isOpen: Boolean,
  },
  data() {
    return {
      user: {
        login: '',
        password: '',
      }
    }
  },
  methods: {
    login() {
      this.$emit('try_login', this.user)
    }
  },
  watch: {
    isOpen(val, _old) {
      if (!val) {
        this.user = {
          login: '',
          password: '',
        }
      }
    }
  }
}
</script>

<template>
  <Modal :is-open="isOpen" @update:isOpen="$emit('update:isOpen', false)">
    <form @submit.prevent class="login-modal">
      <Input
          v-model:value="user.login"
          label="Логин"
          type="text"
          placeholder="Введите логин"/>
      <Input
          v-model:value="user.password"
          label="Пароль"
          type="password"
          placeholder="Введите пароль"/>
      <Button @click="login">Войти</Button>
    </form>

  </Modal>
</template>

<style scoped>
.login-modal {
  display: flex;
  flex-direction: column;
  gap: .75rem;

  padding: 1rem;
}
</style>