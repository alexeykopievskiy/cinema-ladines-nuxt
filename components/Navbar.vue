<template>
  <nav class="v-fall-header__nav">
    <nuxt-link
      prefetch
      v-for="item in menu"
      :key="item.id"
      exact
      class="v-fall-header__link"
      active-class="v-fall-header__link--active"
      :to="item.url"
    >{{item.name}}</nuxt-link>
  </nav>
</template>

<script>
export default {
  props: ['menu'],
};
</script>