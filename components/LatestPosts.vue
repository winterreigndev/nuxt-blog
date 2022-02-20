<template>
  <div id="latestPost">
    <div v-for="latestpost of latestposts" :key="latestpost.title">
      <nuxt-link :to="{ name: 'blog-slug', params: { slug: latestpost.slug } }">
        <b>Latest Article:</b>
        <u>{{ latestpost.title }}</u>
      </nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      latestposts: [],
    };
  },
  async fetch() {
    this.latestposts = await this.$content("blog")
      .limit(1)
      .only(["title", "slug"])
      .sortBy("createdAt", "desc")
      .fetch();
  },
};
</script>
