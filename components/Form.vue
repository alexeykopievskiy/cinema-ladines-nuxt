<template>
  <form @submit.prevent="searchSubmit" class="ladines-header__search">
    <input class="ladines-header__input" v-model="query" type="text" placeholder="Найти" />
  </form>
</template>

<script>
export default {
  data() {
    return {
      query: null
    };
  },
  methods: {
    searchSubmit(e) {
      e.preventDefault();
      this.$router.push("/search?query=" + this.query);
      this.query = null;
    }
  }
};
</script>

<style lang="scss">
.ladines-header__search:before {
  content: url("~../assets/img/search.png");
  width: 16px;
  height: 16px;
  position: absolute;
  left: 21px;
  top: 13px;
  display: block;
  z-index: 1;
}
</style>