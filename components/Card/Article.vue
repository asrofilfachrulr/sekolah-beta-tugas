<template>
  <div
    class="col-6 col-md-3 border border-1 rounded border-dark-subtle p-0 m-0 overflow-hidden"
    style="height: 400px; max-width: 200px; cursor: pointer"
    @click="$emit('news:click')"
  >
    <div class="card-header overflow-hidden h-25 p-0 m-0">
      <img
        class="img-fluid mx-auto"
        style="object-fit: cover"
        :src="article?.imgUrl ? article?.imgUrl : 'https://placehold.co/100'"
        alt="news image"
      />
    </div>
    <div class="card-body p-2" style="height: 65%; min-height: 65%">
      <h5 class="" style="font-size: 1rem; font-weight: 500">
        {{ article?.title }}
      </h5>
      <p class="" style="font-size: 0.75rem">
        {{ article?.description }}
      </p>
    </div>
    <div
      class="card-footer border-top border-dark-subtle p-2"
      style="height: 10%; min-height: 10%"
    >
      <p class="text-muted" style="font-size: 0.75rem">
        {{ article?.time }}
      </p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["article", "isLoading"],
};
</script>

<style>
</style>