<template>
  <div class="container">
    <h3 class="mt-3">Latest Headline News</h3>
    <div v-if="isLoading" class="row justify-content-center mt-4">
      <CardLoading v-for="i in 10" :key="i" :article="articles[0]" />
    </div>
    <div v-else class="row justify-content-center mt-4">
      <CardArticle
        v-for="(article, i) in articles"
        :key="i"
        :article="article"
        @news:click="selectNews(i)"
      />
    </div>
  </div>
</template>

<script>
export default {
  layout() {
    return "custom";
  },
  data() {
    return {
      isLoading: true,
      articles: [],
      response: "",
    };
  },
  mounted() {
    this.fetchNews();
  },
  methods: {
    async fetchNews() {
      this.isLoading = true;
      console.log("[INFO] fetching news...");
      let response;
      try {
        response = await this.$axios.get(
          "https://newsdata.io/api/1/news?apiKey=pub_29995ec5aecfa8d784c7058606b593b030ac2&country=id"
        );
      } catch (e) {
        console.log("[ERROR] completed fetching with error: ", e);
        return;
      }
      console.log("[INFO] completed fetching...");

      this.isLoading = false;
      this.response = response; // debugging purposes

      const results = response.data.results;

      results.forEach((result) => {
        const article = {
          imgUrl: result.image_url,
          title: result.title,
          content: result.content,
          description: result.description,
          time: result.pubDate,
        };
        this.articles.push(article);
      });
    },
    selectNews(i) {
      this.$router.push({
        path: `/article/detail/${i}`,
        query: this.articles[i],
      });
    },
  },
};
</script>

<style scoped>
.row {
  gap: 1.25rem;
}

@media (max-width: 768px) {
  .row {
    gap: 0.65rem;
  }
}
</style>