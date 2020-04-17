<template>
  <main class="ladines-article ladines-article">
    <div class="ladines__container ladines__container--white ladines__container--article">
      <section class="ladines-article-block grid grid-column">
        <h2 class="ladines-article-block__header" v-if="article">{{article.title}}</h2>
        <div class="ladines-article-block__poster">
          <img
            class="ladines-article-block__img"
            style="width: 100%"
            v-if="article"
            :src="article.poster"
            alt
          />
        </div>
        <div class="ladines-article-block__content grid" v-if="article">
          <div class="ladines-article-block__content--left">
            <h3
              class="ladines-article-block__subheader"
            >{{article.excerpt}}</h3>
            <div v-html="article.content"></div>
          </div>
          <div class="ladines-article-block__content--right">
            <div class="ladines-article-block__promo" v-if="ad" v-html="ad"></div>
            <div class="ladines-article-block__related" v-if="related">
              <p class="ladines-article-block__related-header">Смотрите также</p>
              <a
                @click.prevent="openNews(related.url)"
                class="ladines-main-block__item ladines-main-block__item--lg"
                href="#"
              >
                <img class="ladines-main-block__img" :src="related.image" alt />
                <p class="ladines-main-block__text">{{related.title}}</p>
              </a>
            </div>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      article: null,
      related: null,
      ad: null
    };
  },
  methods: {
    openNews(i) {
      this.$router.push(i);
    }
  },
  head() {
    return {
      title: this.article.title,
      meta: [
        {
          hid: "description",
          name: "description",
          content: this.article.excerpt
        }
      ]
    };
  },
  async asyncData({ $axios, params }) {
    const { article, related, ad } = await $axios.$get(
      "https://api.videout.ru/news/" + params.post + "/" + params.id
    );
    return {
      article,
      related,
      ad
    };
  }
};
</script>