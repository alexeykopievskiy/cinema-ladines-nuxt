<template>
  <main class="v-fall-main v-fall-main--page-news">
    <div class="v-fall__container v-fall__container--white v-fall__container--main">
      <section class="v-fall-main-block grid grid-column">
        <h2 class="v-fall-main-block__header">Новости</h2>
        <div class="v-fall-main-block__container grid grid-wrap">
          <a
            href="#"
            @click.prevent="openNews(item.url)"
            v-for="item of news"
            :key="item.id"
            class="v-fall-main-block__item v-fall-main-block__item--lg"
          >
            <img class="v-fall-main-block__img" :src="item.image" alt />
            <p class="v-fall-main-block__text">{{item.title}}</p>
          </a>
        </div>
        <button
          v-if="loadBtn"
          class="v-fall-main-block__btn btn btn-disabled"
          @click="loadMore()"
          type="button"
        >Загрузить еще</button>
      </section>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      news: null,
      loadBtn: true
    };
  },
  async asyncData({ $axios, params }) {
    const { data, page, page_count } = await $axios.$get(
      "https://api.videout.ru/news"
    );
    if (page_count == 1) this.loadBtn = false;
    return {
      news: data,
      page,
      page_count
    };
  },
  head() {
    return {
      title: 'Новости',
      meta: [
        {
          hid: "description",
          name: "description",
          content: "V-fall - лучшие фильмы в формате HD"
        }
      ]
    };
  },
  methods: {
    openNews(i) {
      this.$router.push(i);
    },
    async loadMore() {
      if (this.page != this.page_count) {
        const { data, page } = await this.$axios.$get(
          "https://api.videout.ru" +
            this.$route.fullPath +
            "?limit=12&page=" +
            ++this.page
        );

        if (this.page_count == page) this.loadBtn = false;
        this.news = this.news.concat(data);

        return {
          page
        };
      }
    }
  }
};
</script>

<style lang="scss">
.v-fall-main--page-news {
  .v-fall-main-block__item--lg .v-fall-main-block__img {
    width: 100%;
    height: auto;
  }
}
</style>