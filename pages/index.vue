<template>
  <div class="container" style="height: 500px">
    <h1>Selamat Datang di Halaman Tugas Sekolah Beta</h1>
    <div
      class="row h-100 flex-column justify-content-center align-items-center"
      style="gap: 2rem"
    >
      <h3 style="text-align: center; width: 100%">Topik Tugas</h3>
      <div class="d-flex justify-content-center w-100" style="gap: 2rem">
        <nuxt-link
          :class="'btn btn-success'"
          :style="'width: fit-content; height: fit-content'"
          to="/task"
          >Tugas 03 - Task (Form & Event Handler)</nuxt-link
        >
        <nuxt-link
          :class="'btn btn-success'"
          :style="'width: fit-content; height: fit-content'"
          to="/article"
          >Tugas 04 - Artikel (Routing)</nuxt-link
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout(_) {
    return "custom";
  },
};
</script>

<style>
</style>