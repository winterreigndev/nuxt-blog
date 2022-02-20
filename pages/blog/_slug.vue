<template>
  <div>
    <img
      :src="require(`~/assets/resources/${article.img}`)"
      :alt="article.alt"
      class="post-img"
    />
    <div v-if="article.photoCredit" class="img-credit">
      <a :href="article.photoCreditLink">© {{ article.photoCredit }}</a>
    </div>
    <div class="container">
      <div class="grid-post">
        <article>
          <div class="articles">
            <div class="post-info">
              Published: {{ formatDate(article.createdAt) }} &bull; By:
              {{ article.author }}
            </div>
            <h1>{{ article.title }}</h1>
            <nuxt-content :document="article" />
            <div class="tags">{{ article.tags }}</div>
          </div>
          <prev-next :prev="prev" :next="next" />
        </article>

        <aside>
          <SideBar />
        </aside>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content("blog", params.slug).fetch();
    const [prev, next] = await $content("blog").surround(params.slug).fetch();

    return {
      article,
      prev,
      next,
    };
  },
  methods: {
    formatDate(date) {
      const options = {
        year: "numeric",
        month: "long",
        day: "numeric",
      };
      return new Date(date).toLocaleDateString("en", options);
    },
  },
};
</script>
