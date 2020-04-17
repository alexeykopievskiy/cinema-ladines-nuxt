<template>
  <main class="v-fall-main">
    <section class="v-fall-main-slider owl-carousel" v-if="posters">
      <div
        @click="openVideo(item.url)"
        v-for="item in posters"
        :key="item.id"
        class="v-fall-main-slide"
      >
        <img :src="item.image" alt />
        <div class="v-fall-main-slide__content">
          <p class="v-fall-main-slide__label">
            <span class="v-fall-main-slide__text">{{item.year}}</span>
            <span class="v-fall-main-slide__text">{{item.category.join(', ')}}</span>
          </p>
          <h3 class="v-fall-main-slide__title">{{item.title}}</h3>
        </div>
      </div>
    </section>
    <div class="v-fall-main-slide__controls">
      <button class="v-fall-main-slide__prev"></button>
      <button class="v-fall-main-slide__next"></button>
    </div>
    <div class="v-fall__container v-fall__container--white v-fall__container--main">
      <section class="v-fall-main-block">
        <h2 class="v-fall-main-block__header">Фильмы</h2>
        <div
          class="v-fall-main-block__container v-fall-main-block__container-carousel--default owl-carousel"
          v-if="films"
        >
          <a
            @click.prevent="openVideo(item.url)"
            v-for="item in films"
            :key="item.id"
            class="v-fall-main-block__item"
            href="#"
          >
            <img class="v-fall-main-block__img" :src="item.image" alt />
            <h3 class="v-fall-main-block__title">{{item.title}}</h3>
            <p class="v-fall-main-block__content">
              <span class="v-fall-main-block__elem">{{item.year}}</span>
              <span class="v-fall-main-block__elem">{{item.category.join(', ')}}</span>
            </p>
          </a>
        </div>
      </section>
      <section class="v-fall-main-block v-fall-main-block__carousel">
        <h2 class="v-fall-main-block__header">РЕКОМЕНДУЕМ ПОСМОТРЕТЬ</h2>
        <div
          class="v-fall-main-block__container v-fall-main-block__container-carousel owl-carousel"
        >
          <a
            @click.prevent="openVideo(item.url)"
            v-for="item in recommendations"
            :key="item.id"
            class="v-fall-main-block__item v-fall-main-block__item--lg"
            href="#"
          >
            <img class="v-fall-main-block__img" :src="item.image" alt />
            <h3 class="v-fall-main-block__title">{{item.title}}</h3>
            <p class="v-fall-main-block__content">
              <span class="v-fall-main-block__elem">{{item.year}}</span>
              <span class="v-fall-main-block__elem">{{item.category.join(', ')}}</span>
            </p>
          </a>
        </div>
        <div class="v-fall-main-block__carousel-btn">
          <button class="v-fall-main-block__carousel-left"></button>
          <button class="v-fall-main-block__carousel-right"></button>
        </div>
      </section>
      <section class="v-fall-main-block">
        <h2 class="v-fall-main-block__header">Мультфильмы</h2>
        <p class="v-fall-main-block__subtitle">Коллекция лучших мультфильмов для детей и родителей</p>
        <div
          class="v-fall-main-block__container v-fall-main-block__container-carousel--default owl-carousel"
          v-if="cartoons"
        >
          <a
            @click.prevent="openVideo(item.url)"
            v-for="item in cartoons"
            :key="item.id"
            class="v-fall-main-block__item"
            href="#"
          >
            <img class="v-fall-main-block__img" :src="item.image" alt />
            <h3 class="v-fall-main-block__title">{{item.title}}</h3>
            <p class="v-fall-main-block__content">
              <span class="v-fall-main-block__elem">{{item.year}}</span>
              <span class="v-fall-main-block__elem">{{item.category.join(', ')}}</span>
            </p>
          </a>
        </div>
      </section>
      <section class="v-fall-main-block">
        <h2 class="v-fall-main-block__header">Новости</h2>
        <div
          class="v-fall-main-block__container v-fall-main-block__container-carousel v-fall-main-block__container-carousel--news owl-carousel"
          v-if="news"
        >
          <a
            @click.prevent="openNews(item.url)"
            v-for="item of news"
            :key="item.id"
            class="v-fall-main-block__item v-fall-main-block__item--lg"
            href="#"
          >
            <img class="v-fall-main-block__img" :src="item.image" alt />
            <p class="v-fall-main-block__text">{{item.title}}</p>
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
    $(".v-fall-main-slider").owlCarousel({
      center: true,
      items: 2,
      loop: true,
      margin: 10,
      autoWidth: true
    });
    $(".v-fall-main-slide__next").click(function() {
      $(".v-fall-main-slider").trigger("next.owl.carousel", [300]);
    });
    $(".v-fall-main-slide__prev").click(function() {
      $(".v-fall-main-slider").trigger("prev.owl.carousel", [300]);
    });

    $(".v-fall-main-block__container-carousel").owlCarousel({
      items: 4,
      loop: true,
      margin: 26,
      autoWidth: true
    });

    $(".v-fall-main-block__container-carousel--default").owlCarousel({
      items: 6,
      loop: true,
      margin: 17,
      autoWidth: true,
      autoplay: true,
      autoplayTimeout: 2000,
      autoplayHoverPause: true
    });

    $(".v-fall-main-block__carousel-right").click(function() {
      $(".v-fall-main-block__container-carousel").trigger("next.owl.carousel", [
        300
      ]);
    });
    $(".v-fall-main-block__carousel-left").click(function() {
      $(".v-fall-main-block__container-carousel").trigger("prev.owl.carousel", [
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
.v-fall-main {
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
.v-fall-main-block__container-carousel > span {
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

.v-fall-main-block__img {
  width: 100%;
  height: auto;
}

.v-fall-main-block__container {
  width: 95%;
}
.v-fall-main-slider.owl-carousel .owl-item img {
  height: 100%;
  min-width: 1330px;
}

.v-fall-main-slide {
  height: 479px;
}

.v-fall-main-block__container-carousel {
  .v-fall-main-block__item--lg .v-fall-main-block__img {
    height: 400px;
  }
}

.v-fall-main-block__container-carousel--news {
  .v-fall-main-block__item--lg .v-fall-main-block__img {
    width: auto;
    height: 284px;
  }
}

.v-fall-main-block__item--lg {
  overflow: hidden;

  .v-fall-main-block__img {
    width: auto;
  }
}
</style>
