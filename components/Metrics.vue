<template>
  <div>
    <div v-if="yandex_id">
      <div v-html="yandex"></div>
    </div>
    <div v-if="google_id">
      <div v-html="google"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      yandex_id: null,
      google_id: null,
      yandex:
        "<scr" +
        'ipt>(function(m,e,t,r,i,k,a){m[i]=m[i]||function(){(m[i].a=m[i].a||[]).push(arguments)};m[i].l=1*new Date();k=e.createElement(t),a=e.getElementsByTagName(t)[0],k.async=1,k.src=r,a.parentNode.insertBefore(k,a)})(window, document, "script", "https://mc.yandex.ru/metrika/tag.js", "ym");ym(' +
        this.yandex_id +
        ', "init", {clickmap:true,trackLinks:true,accurateTrackBounce:true});</scr' +
        'ipt><noscript><div><img src="https://mc.yandex.ru/watch/' +
        this.yandex_id +
        '" style="position:absolute; left:-9999px;" alt=""></div></noscript>',
      google:
        "<scr" +
        'ipt async="" src="https://www.googletagmanager.com/gtag/js?id=' +
        this.google_id +
        '"></scr' +
        "ipt>" +
        '<script>window.dataLayer = window.dataLayer || [];function gtag(){dataLayer.push(arguments);}gtag("js", new Date());gtag("config", ' +
        this.google_id +
        ");" +
        "</scr" +
        "ipt>"
    };
  },
  async mounted() {
    const response = await this.$axios.$get("https://api.videout.ru/source");

    if (response.counter != null) {
      const { yandex_metrica, google_analytics } = response.counter;

      this.yandex_id = yandex_metrica,
      this.google_id = google_analytics
    }
  }
};
</script>