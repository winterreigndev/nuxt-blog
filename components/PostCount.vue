<template>
  <div>{{ total_articles }} Total Articles</div>
</template>

<script>
export default {
  name: "Articles",
  data() {
    return {
      total_articles: require
        .context("../content/blog", false, /\.(md)$/)
        .keys().length,
    };
  },
};
</script>
