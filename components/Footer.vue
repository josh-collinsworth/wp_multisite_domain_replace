<template>
  <footer>
    <div class="container">
      Built with
      <a href="https://nuxtjs.org/" target="_blank">NuxtJS</a>
      by
      <a href="https://joshcollinsworth.com/" target="_blank"
        >Josh Collinsworth</a
      >
    </div>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
  components: {},
  props: {}
}
</script>

<style scoped>
footer {
  min-height: 80px;
  background: #f8fafc;
  display: flex;
  align-items: center;
  margin-top: 4rem;
}
</style>
