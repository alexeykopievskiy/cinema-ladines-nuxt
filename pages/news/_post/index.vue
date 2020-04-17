<template>
  <main class="ladines-main ladines-main--page-news">
    <div class="ladines__container ladines__container--white ladines__container--main">
      <section class="ladines-main-block grid grid-column">
        <h2 class="ladines-main-block__header">Новости</h2>

        <div class="ladines-main-block__container grid grid-wrap">
          <a
            href="#"
            @click.prevent="openNews(item.url)"
            v-for="item of news"
            :key="item.id"
            class="ladines-main-block__item ladines-main-block__item--lg"
          >
            <img class="ladines-main-block__img" :src="item.image" alt />
            <p class="ladines-main-block__text">{{item.title}}</p>
          </a>
        </div>
        <button
          v-if="loadBtn"
          @click="loadMore()"
          class="ladines-main-block__btn btn btn-disabled"
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
      "https://api.videout.ru/news/" + params.post
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
          content: "ladines - лучшие фильмы в формате HD"
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