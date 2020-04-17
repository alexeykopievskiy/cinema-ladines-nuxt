<template>
  <header class="ladines-header grid">
    <div class="ladines__container grid grid-v-center grid-v-between">
      <Logo />
      <Navbar v-bind:menu="toggleMenu" />
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