<template>
  <header class="ladines-header grid">
    <div class="ladines__container grid grid-v-center grid-v-between">
      <div class="ladines-header__logo">
        <img class="ladines-header__img" src="img/logo-ladines.png" alt="ladines" />
      </div>
      <nav class="ladines-header__nav">
        <a class="ladines-header__link" href="#">Новинки</a>
        <a class="ladines-header__link" href="#">Фильмы</a>
        <a class="ladines-header__link" href="#">Мультфильмы</a>
        <a class="ladines-header__link" href="#">Новости</a>
        <a class="ladines-header__link" href="#">Кинотеатры</a>
        <a class="ladines-header__link" href="#">Контакты</a>
      </nav>
      <Form />
    </div>
  </header>
</template>

<script>
import Logo from "@/components/Logo";
import Navbar from "@/components/Navbar";
import Form from "@/components/Form";

export default {
  components: {
    Logo,
    Navbar,
    Form
  },
  data() {
    return {
      toggleMenu: null
    };
  },
  async mounted() {
    const { toggleMenu } = await this.$axios.$get(
      "https://api.videout.ru/source"
    );
    this.toggleMenu = toggleMenu;
  }
};
</script>