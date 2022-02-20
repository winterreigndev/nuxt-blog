<template>
  <div>
    <Header />
    <div class="banner">
      <div class="banner-text">
        <div>
          <LatestPosts />
        </div>
        <div>
          <TodaysDate />
        </div>
        <div>
          <LastUpdated />
        </div>
      </div>
    </div>
    <nuxt />
    <div class="clear"></div>
    <footer>
      Made with ❤️ by
      <a href="https://winterreign.dev" alt="WinterReign.dev">WinterReign</a>
    </footer>
  </div>
</template>

<script>
export default {
  head: {
    title: "My First Nuxt Blog",
    meta: [
      {
        charset: "utf-8",
      },
      {
        name: "viewport",
        content: "width=device-width, initial-scale=1",
      },
      {
        hid: "a simple nuxt blog",
        name: "a simple nuxt blog",
        content: "My First Nuxt Blog",
      },
    ],
  },
};
</script>

<style scoped></style>
