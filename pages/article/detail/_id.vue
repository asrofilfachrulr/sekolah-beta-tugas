<template>
  <div class="container">
    <div class="row">
      <div class="col-12 col-md-8 col-lg-6">
        <img class="img-fluid" :src="imgUrl" alt="" />
      </div>
    </div>
    <div class="row">
      <h3 class="col-12">{{ title }}</h3>
    </div>
    <div class="row">
      <div class="col-12">
        <p class="text-muted">{{ time }}</p>
      </div>
      <div class="col-12">
        <p>{{ content }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout(_) {
    return "custom";
  },
  data() {
    return {
      imgUrl: this.$route.query.imgUrl || "empty",
      title: this.$route.query.title || "empty",
      content: this.$route.query.content || "empty",
      time: this.$route.query.time || "empty",
    };
  },
};
</script>

<style>
</style>