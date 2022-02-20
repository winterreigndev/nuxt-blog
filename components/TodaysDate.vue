<template>
  <div class="todaysdate">
    <div class="today">TODAY</div>
    <p v-text="str" :key="str" />
    <span>
      {{
        formatDate(
          $moment(new Date(), "YYYYMMDD").format("YYYY-MM-DDTHH:mm:ss")
        )
      }}
    </span>
  </div>
</template>

<script>
export default {
  props: ["str"],
  asyncData(ctx) {
    return {
      str: ctx.app.$moment(new Date(), "YYYYMMDD").format("MMMM"),
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
