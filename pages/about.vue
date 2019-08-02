<template>
  <div>
    <H1>About</H1>
    <p>
      This site is a tool to help update your multisite's primary domain during
      domain changes (such as transfer to a staging or local environment, or
      when updating the primary domain of the multisite). Ordinarily, such
      changes require either a tedious manual update, or a specialized
      service/plugin. WPMultisiteDomainReplace.com just gives you the requisite
      SQL to run.
    </p>
    <p>
      This current version of the site is built with
      <a href="https://nuxtjs.org/" target="_blank">NuxtJS</a>, and is hosted on
      <a href="https://zeit.co/now" target="_blank">Zeit Now</a>.
    </p>
    <p>
      <nuxt-link to="/">⬅️ Back to home</nuxt-link>
    </p>
  </div>
</template>

<script>
import H1 from '@/components/elements/H1'

export default {
  components: { H1 }
}
</script>

<style scoped></style>
