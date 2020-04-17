<template>
  <main class="ladines-main">
    <div class="ladines__container grid grid-column ladines__container--inner">
      <section class="ladines__container--white ladines-inner__block" v-if="video">
        <div class="ladines-inner__video ut-overroll" style="width: 939px">
          <div>
            <h2 class="ladines-inner__header">{{video.title}}</h2>
            <h4 class="ladines-inner__subheader">{{video.subtitle}}</h4>
          </div>
          <div class="ladines-inner__video-inside" v-html="video.player_code"></div>
        </div>
      </section>
      <section class="ladines-inner__block ladines-inner__block-poster" v-if="video">
        <div class="ladines-inner__content">
          <div class="ladines-inner__content-poster">
            <img class="ladines-inner__content-img" style="width: 235px" :src="video.image" />
          </div>
          <div class="ladines-inner__content-description grid grid-column">
            <div class="ladines-inner__content-list">
              <div class="ladines-inner__content-item">
                <span>Год</span>
                <span>{{video.year}}</span>
              </div>
              <div class="ladines-inner__content-item">
                <span>Страна</span>
                <span>{{video.country}}</span>
              </div>
              <div class="ladines-inner__content-item">
                <span>Жанр</span>
                <span>{{video.category}}</span>
              </div>
            </div>
            <span class="ladines-inner__content-text">{{video.description}}</span>
          </div>
        </div>
      </section>
      <section class="ladines__container--white ladines-main-block ladines-inner__block-container">
        <h2 class="ladines-main-block__header">Новости</h2>
        <div class="ladines-main-block__container" v-if="news">
          <a
            @click.prevent="openNews(item.url)"
            v-for="item in news"
            :key="item.id"
            class="ladines-main-block__item ladines-main-block__item--lg"
            href="#"
          >
            <img class="ladines-main-block__img" :src="item.image" alt />
            <p class="ladines-main-block__text">{{item.title}}</p>
          </a>
        </div>
      </section>
      <section class="ladines__container--white ladines-inner__block-container">
        <div class="ladines-main-block ladines-main-block__carousel">
          <h2 class="ladines-main-block__header">Рекумондуем посмотреть</h2>
          <div
            class="ladines-main-block__container ladines-main-block__container-carousel owl-carousel"
          >
            <a
              @click.prevent="openVideo(item.url)"
              v-for="item in recommendations"
              :key="item.id"
              class="ladines-main-block__item ladines-main-block__item--lg"
              href="#"
            >
              <img class="ladines-main-block__img" :src="item.image" alt />
              <h3 class="ladines-main-block__title">{{item.title}}</h3>
              <p class="ladines-main-block__content">
                <span class="ladines-main-block__elem">{{item.year}}</span>
                <span class="ladines-main-block__elem">{{item.category.join(', ')}}</span>
              </p>
            </a>
          </div>
          <div class="ladines-main-block__carousel-btn">
            <button class="ladines-main-block__carousel-left"></button>
            <button class="ladines-main-block__carousel-right"></button>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<script>
export default {
  validate({ params }) {
    return /^\d+$/.test(params.id);
  },
  data: () => {
    return {
      video: null,
      news: null,
      recommendations: null
    };
  },
  methods: {
    openNews(i) {
      this.$router.push(i);
    }
  },
  head () {
    return {
      title: this.video.title,
      meta: [
        { hid: 'description', name: 'description', content: this.video.description }
      ]
    }
  },
  mounted() {
    $(".ladines-main-block__container-carousel").owlCarousel({
      items: 4,
      loop: true,
      margin: 26,
      autoWidth: true
    });

    $(".ladines-main-block__carousel-right").click(function() {
      $(".ladines-main-block__container-carousel").trigger("next.owl.carousel", [
        300
      ]);
    });
    $(".ladines-main-block__carousel-left").click(function() {
      $(".ladines-main-block__container-carousel").trigger("prev.owl.carousel", [
        300
      ]);
    });
  },
  async asyncData({ $axios, params, route }) {
    const response = await $axios.$get(
      "https://api.videout.ru" + route.fullPath
    );
    const { video, news, recommendations } = await $axios.$get(
      "https://api.videout.ru" + route.fullPath
    );
    return {
      video,
      news,
      recommendations
    };
  }
};
</script>

<style lang="scss">
.ladines-inner__block-container {
  padding-top: 40px;
}
.ladines-inner__video-inside {
  width: 100%;
  max-width: 939px;
  min-height: 530px;

  iframe {
    width: 100%;
    height: 100%;
    min-height: 530px;
  }
}
</style>