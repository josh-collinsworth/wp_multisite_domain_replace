<template>
  <header>
    <div class="container">
      <nuxt-link to="/">
        <p>WP Multisite Domain Replace</p>
      </nuxt-link>
      <nav>
        <ul>
          <li>
            <nuxt-link to="/about">What is this?</nuxt-link>
          </li>
          <li>
            <nuxt-link to="/faq">FAQ</nuxt-link>
          </li>
        </ul>
      </nav>
    </div>
  </header>
</template>

<script>
export default {
  name: 'Header',
  components: {},
  props: {}
}
</script>

<style scoped>
header {
  min-height: 80px;
  background: #f8fafc;
  display: flex;
  align-items: center;
  margin-bottom: 4rem;
}

header p {
  font-weight: bold;
  margin: 0;
  font-size: 1.2rem;
}

.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
ul {
  list-style-type: none;
  display: flex;
  padding-left: 0;
}

li {
  margin-left: 1em;
  font-weight: bold;
}
</style>
