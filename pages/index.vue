<template>
  <main class="ladines-main">
    <section class="ladines-main-slider owl-carousel" v-if="posters">
      <div
        @click="openVideo(item.url)"
        v-for="item in posters"
        :key="item.id"
        class="ladines-main-slide"
      >
        <img :src="item.image" alt />
        <div class="ladines-main-slide__content">
          <p class="ladines-main-slide__label">
            <span class="ladines-main-slide__text">{{item.year}}</span>
            <span class="ladines-main-slide__text">{{item.category.join(', ')}}</span>
          </p>
          <h3 class="ladines-main-slide__title">{{item.title}}</h3>
        </div>
      </div>
    </section>
    <div class="ladines-main-slide__controls">
      <button class="ladines-main-slide__prev"></button>
      <button class="ladines-main-slide__next"></button>
    </div>
    <div class="ladines__container ladines__container--white ladines__container--main">
      <section class="ladines-main-block">
        <h2 class="ladines-main-block__header">Фильмы</h2>
        <div
          class="ladines-main-block__container ladines-main-block__container-carousel--default owl-carousel"
          v-if="films"
        >
          <a
            @click.prevent="openVideo(item.url)"
            v-for="item in films"
            :key="item.id"
            class="ladines-main-block__item"
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
      </section>
      <section class="ladines-main-block ladines-main-block__carousel">
        <h2 class="ladines-main-block__header">РЕКОМЕНДУЕМ ПОСМОТРЕТЬ</h2>
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
      </section>
      <section class="ladines-main-block">
        <h2 class="ladines-main-block__header">Мультфильмы</h2>
        <p class="ladines-main-block__subtitle">Коллекция лучших мультфильмов для детей и родителей</p>
        <div
          class="ladines-main-block__container ladines-main-block__container-carousel--default owl-carousel"
          v-if="cartoons"
        >
          <a
            @click.prevent="openVideo(item.url)"
            v-for="item in cartoons"
            :key="item.id"
            class="ladines-main-block__item"
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
      </section>
      <section class="ladines-main-block">
        <h2 class="ladines-main-block__header">Новости</h2>
        <div
          class="ladines-main-block__container ladines-main-block__container-carousel ladines-main-block__container-carousel--news owl-carousel"
          v-if="news"
        >
          <a
            @click.prevent="openNews(item.url)"
            v-for="item of news"
            :key="item.id"
            class="ladines-main-block__item ladines-main-block__item--lg"
            href="#"
          >
            <img class="ladines-main-block__img" :src="item.image" alt />
            <p class="ladines-main-block__text">{{item.title}}</p>
          </a>
        </div>
      </section>
    </div>
  </main>
</template>

<script>
let requestUrl = "https://api.videout.ru";

export default {
  data: () => {
    return {
      source: null,
      films: null,
      cartoons: null,
      news: null,
      posters: null
    };
  },
  mounted() {
    $(".ladines-main-slider").owlCarousel({
      center: true,
      items: 2,
      loop: true,
      margin: 10,
      autoWidth: true
    });
    $(".ladines-main-slide__next").click(function() {
      $(".ladines-main-slider").trigger("next.owl.carousel", [300]);
    });
    $(".ladines-main-slide__prev").click(function() {
      $(".ladines-main-slider").trigger("prev.owl.carousel", [300]);
    });

    $(".ladines-main-block__container-carousel").owlCarousel({
      items: 4,
      loop: true,
      margin: 26,
      autoWidth: true
    });

    $(".ladines-main-block__container-carousel--default").owlCarousel({
      items: 6,
      loop: true,
      margin: 17,
      autoWidth: true,
      autoplay: true,
      autoplayTimeout: 2000,
      autoplayHoverPause: true
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
  methods: {
    openVideo(i) {
      this.$router.push(i);
    },
    openNews(i) {
      this.$router.push(i);
    }
  },
  async asyncData({ $axios, params }) {
    let response = await $axios.get(requestUrl);
    const { recommendations, movies, cartoons, news, posters } = response.data;
    return {
      recommendations,
      films: movies,
      cartoons,
      news,
      posters
    };
  }
};
</script>

<style lang="scss">
.ladines-main {
  &-slide {
    &__prev {
      margin-left: 80px;
      background: transparent url("../assets/img/arrow.png") no-repeat;
      width: 27px;
      height: 42px;
      border: none;
      outline: none;
      cursor: pointer;

      &:hover {
        background: transparent url("../assets/img/arrow-hover.png") no-repeat;
        transform: rotate(-180deg);
      }
    }
    &__next {
      margin-right: 80px;
      margin-left: 80px;
      background: transparent url("../assets/img/arrow.png") no-repeat;
      width: 27px;
      height: 42px;
      border: none;
      outline: none;
      transform: rotate(-180deg);
      cursor: pointer;

      &:hover {
        background: transparent url("../assets/img/arrow-hover.png") no-repeat;
        transform: rotate(0deg);
      }
    }
  }
  &-block {
    &__carousel {
      &-left {
        background: transparent url("../assets/img/arrow-hover.png") no-repeat;
        width: 27px;
        height: 42px;
        margin-left: 24px;
        transform: rotate(-180deg);
        border: none;
        outline: none;
        cursor: pointer;
        position: relative;
        z-index: 100;
      }
      &-right {
        background: transparent url("../assets/img/arrow-hover.png") no-repeat;
        width: 27px;
        height: 42px;
        margin-right: 24px;
        border: none;
        outline: none;
        cursor: pointer;
        position: relative;
        z-index: 100;
      }
    }
  }
}
.ladines-main-block__container-carousel > span {
  position: absolute;
  top: 50%;
  left: 0;
}

.owl-carousel.owl-theme + span {
  left: initial;
  right: 0;
}

.owl-carousel.owl-loaded {
  display: flex !important;
}

.ladines-main-block__img {
  width: 100%;
  height: auto;
}

.ladines-main-block__container {
  width: 95%;
}
.ladines-main-slider.owl-carousel .owl-item img {
  height: 100%;
  min-width: 1330px;
}

.ladines-main-slide {
  height: 479px;
}

.ladines-main-block__container-carousel {
  .ladines-main-block__item--lg .ladines-main-block__img {
    height: 400px;
  }
}

.ladines-main-block__container-carousel--news {
  .ladines-main-block__item--lg .ladines-main-block__img {
    width: auto;
    height: 284px;
  }
}

.ladines-main-block__item--lg {
  overflow: hidden;

  .ladines-main-block__img {
    width: auto;
  }
}
</style>
