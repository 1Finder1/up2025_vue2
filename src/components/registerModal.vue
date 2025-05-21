<script>
import Modal from "@/components/modal.vue";
import Input from "@/UIComponents/input.vue";
import Select from "@/UIComponents/select.vue";
import Button from "@/UIComponents/button.vue";

const firstNameRegexp = /^[А-Я][а-я]*$/
const LFNameRegexp = /^[А-Я][а-я]+$/
const phoneRegexp = /^8\(\d{3}\)\d{3}-\d{2}-\d{2}$/

export default {
  components: {Button, Select, Input, Modal},
  props: {
    isOpen: Boolean,
  },
  data() {
    return {
      user: {
        login: "",
        password: "",
        firstName: "",
        lastName: "",
        fatherName: "",
        phoneNumber: '',
        sex: '',
        age: '',
        loveFaculty: '',
      },
      facultyOptions: ['Гриффиндор', 'Когтевран', 'Пуффендуй', 'Слизерин'].map(item => ({id: item, label: item})),
      sexOptions: [{id: 'man', label: 'Мужской'}, {id: 'woman', label: 'Женский'}],
    };
  },
  methods: {
    register() {
      if (!firstNameRegexp.test(this.user.firstName))
        return alert('Имя должно содержать минимум 1 символ и начинаться с заглавной буквы')
      if (!LFNameRegexp.test(this.user.lastName))
        return alert('Фамилия должна содержать минимум 2 символа и начинаться с заглавной буквы')
      if (!LFNameRegexp.test(this.user.fatherName))
        return alert('Отчество должно содержать минимум 2 символа и начинаться с заглавной буквы')
      if (!phoneRegexp.test(this.user.phoneNumber))
        return alert('Телефон должен быть в формате 8(XXX)XXX-XX-XX')
      if (!this.user.sex) return alert('Необходимо выбрать пол')
      if (!this.user.loveFaculty) return alert('Необходимо выбрать любимый факультет')
      if (this.user.age < 10) return alert('Возраст должен быть не менее 10 лет')

      this.$emit('register', this.user)
      this.user = {
        login: "",
        password: "",
        firstName: "",
        lastName: "",
        fatherName: "",
        phoneNumber: '',
        sex: '',
        age: '',
        loveFaculty: '',
      }
    }
  }
}
</script>

<template>
  <Modal :is-open="isOpen" @update:isOpen="$emit('update:isOpen', false)">
    <form @submit.prevent class="register-modal">
      <Input type="text" label="Логин" v-model:value="user.login"/>
      <Input type="password" label="Пароль" v-model:value="user.password"/>
      <Input type="text" label="Имя" v-model:value="user.firstName"/>
      <Input type="text" label="Фамилия" v-model:value="user.lastName"/>
      <Input type="text" label="Отчество" v-model:value="user.fatherName"/>
      <Input type="tel" label="Номер телефона" v-model:value="user.phoneNumber"/>
      <Input type="number" label="Возраст" v-model:value="user.age"/>
      <Select :options="sexOptions" v-model:current-value="user.sex" label="Пол"/>
      <Select :options="facultyOptions" v-model:current-value="user.loveFaculty" label="Любимый факультет"/>
      <Button @click="register">Зарегистрироваться</Button>
    </form>
  </Modal>
</template>

<style>

.register-modal {
  display: grid;
  grid-template-columns: repeat(2, 200px);
  gap: .75rem 1rem;

  padding: 1rem;
}

</style>