<template>
  <main class="ladines-main">
    <div class="ladines__container grid grid-column ladines__container--contacts">
      <section class="ladines__container--white ladines-contacts__block">
        <h2 class="ladines-contacts__header">{{name}}</h2>
        <div class="ladines-contacts__content" v-html="content"></div>
      </section>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    const { name, content } = await $axios.$get(
      "https://api.videout.ru/pages/" + params.name
    );
    return { name, content };
  },
  head() {
    return {
      title: this.name,
      meta: [
        {
          hid: "description",
          name: "description",
          content: "ladines - лучшие фильмы в формате HD"
        }
      ]
    };
  }
};
</script>