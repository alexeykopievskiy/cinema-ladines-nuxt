<template>
  <main class="v-fall-main">
    <div class="v-fall__container grid grid-column v-fall__container--contacts">
      <section class="v-fall__container--white v-fall-contacts__block">
        <h2 class="v-fall-contacts__header">{{name}}</h2>
        <div class="v-fall-contacts__content" v-html="content"></div>
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
          content: "V-fall - лучшие фильмы в формате HD"
        }
      ]
    };
  }
};
</script>