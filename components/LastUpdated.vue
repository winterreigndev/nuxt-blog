<template>
  <div id="lastUpdated">
    <div v-for="latestpost of latestposts" :key="latestpost.updatedAt">
      <b>Updated On:</b> <u>{{ formatDate(latestpost.updatedAt) }}</u>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      latestposts: null,
    };
  },
  async fetch() {
    this.latestposts = await this.$content("blog")
      .limit(1)
      .only(["updatedAt"])
      .sortBy("createdAt", "desc")
      .fetch();
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
