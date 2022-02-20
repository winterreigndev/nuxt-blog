<template>
  <div>
    <div class="sort-count">
      <div>
        <span class="showing-articles"
          >Articles:
          <span class="no-focus">
            <input
              v-model="searchQuery"
              style="background: transparent"
              tabindex="-1"
              readonly="readonly"
              placeholder="All"
            />
          </span>
        </span>
      </div>
      <div>
        <span class="sort-articles">
          Search Articles For:
          <input
            v-model="searchQuery"
            type="search"
            autocomplete="off"
            placeholder="Type Here"
          />
        </span>
      </div>
      <div class="post-count">
        <PostCount />
      </div>
    </div>

    <div class="container">
      <div class="wrap" v-show="searchQuery">
        <div v-if="results.length" class="grid-two">
          <div v-for="result of results" :key="result.slug" class="width-100">
            <div class="articles grid-article">
            <div>
            <nuxt-link
              :to="{ name: 'blog-slug', params: { slug: result.slug } }"
            >
              <img
                :src="require(`~/assets/resources/${result.img}`)"
                alt="result.title"
              />
            </nuxt-link>
            </div>
            <div>
              <div class="date">{{ formatDate(result.createdAt) }}</div>
              <nuxt-link
              class="post-title" 
              :to="{ name: 'blog-slug', params: { slug: result.slug } }"
            >
              {{ result.title }}
            </nuxt-link>
            <article>
              {{ result.description | excerpt }} <em>...</em>
            </article>
            <div class="tags">{{ result.tags }}</div>
            </div>
            </div>
          </div>
        </div>
        <div class="articles" v-else>
          <h2 class="post-title">Search Results:</h2>
          <p>We're sorry, there are no articles that include the search query.</p>
          <p><strong>Search Suggestions:</strong>
          <ul class="suggestions-list"><li>Check for any spelling errors</li>
          <li>Try to use keywords rather than full sentences</li></ul></p>
          </div>
      </div>
      <div v-if="!searchQuery" class="grid-two">
        <div v-for="article of articles" :key="article.title" class="width-100">
          <div class="articles grid-article">
            <div>
            <nuxt-link
              :to="{ name: 'blog-slug', params: { slug: article.slug } }"
            >
              <img
                :src="require(`~/assets/resources/${article.img}`)"
                alt="article.title"
              />
            </nuxt-link>
            </div>
          <div>
            <div class="date">{{ formatDate(article.createdAt) }}</div>
            <nuxt-link
              class="post-title" 
              :to="{ name: 'blog-slug', params: { slug: article.slug } }"
            >
              {{ article.title }}
            </nuxt-link>
            <article>
              {{ article.description | excerpt }} <em>...</em>
            </article>
            <div class="tags">{{ article.tags }}</div>
          </div>
          </div>
        </div>
      </div>
    <div v-if="!searchQuery" class="pagination-wrap">
    <section id="prev-next">
      <nuxt-link :to="prevLink">Prev page</nuxt-link>
      <nuxt-link v-if="nextPage" :to="`/page/${pageNo + 1}`"
        >Next page</nuxt-link
      >
    </section>
    </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: "",
      results: [],
    };
  },
  watch: {
    async searchQuery(searchQuery) {
      if (!searchQuery) {
        this.results = [];
        return;
      }
      this.results = await this.$content("blog")
      .search(searchQuery)
      .sortBy("createdAt", "desc")
      .fetch();
    },
  },
  async asyncData({ $content, params,error }) {
    const pageNo = parseInt(params.number);
    const blogPosts = await $content("blog", params.slug)
      .only(["title", "description", "img", "slug", "tags", "createdAt"])
      .sortBy("createdAt", "desc")
      .limit(9)
      .skip(8 * (pageNo - 1))
      .fetch();

    if (!blogPosts.length) {
      return error({ statusCode: 404, message: "No articles found!" });
    }

    const nextPage = blogPosts.length === 9;
    const articles = nextPage ? blogPosts.slice(0, -1) : blogPosts;

    return {
      nextPage,
      articles,
      blogPosts,
      pageNo,
    };
  },
  filters: {
    excerpt: function (value) {
      return value.slice(0, 70);
    },
  },
  methods: {
    formatDate(date) {
      const options = { year: "numeric", month: "long", day: "numeric" };
      return new Date(date).toLocaleDateString("en", options);
    },
  },
  computed: {
    prevLink() {
      return this.pageNo === 2 ? "/" : `/page/${this.pageNo - 1}`;
    },
  },
};
</script>
