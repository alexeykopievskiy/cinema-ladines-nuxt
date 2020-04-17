<template>
  <div>
    <footer class="v-fall-footer grid">
      <div class="v-fall__container grid grid-v-center grid-v-between">
        <Logo />
        <Navbar v-bind:menu="bottomMenu" />
        <Form />
      </div>
    </footer>
    <Metrics />
  </div>
</template>

<script>
import Logo from "@/components/Logo";
import Form from "@/components/Form";
import Navbar from "@/components/Navbar";
import Metrics from "@/components/Metrics";

export default {
  components: {
    Logo,
    Navbar,
    Form,
    Metrics
  },
  data() {
    return {
      bottomMenu: null
    };
  },
  async mounted() {
    const { bottomMenu } = await this.$axios.$get(
      "https://api.videout.ru/source"
    );
    this.bottomMenu = bottomMenu;
  }
};
</script>