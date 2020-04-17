<template>
  <nav class="ladines-header__nav">
    <nuxt-link
      prefetch
      v-for="item in menu"
      :key="item.id"
      exact
      class="ladines-header__link"
      active-class="ladines-header__link--active"
      :to="item.url"
    >{{item.name}}</nuxt-link>
  </nav>
</template>

<script>
export default {
  props: ['menu'],
};
</script>