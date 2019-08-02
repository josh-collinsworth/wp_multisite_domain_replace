<template>
  <div class="heading-wrap">
    <h1>
      <slot> </slot>
    </h1>
  </div>
</template>

<script>
export default {
  name: 'H1'
}
</script>

<style>
.heading-wrap {
  position: relative;
  width: 100%;
  overflow: hidden;
  margin-bottom: 3em;
}

.heading-wrap h1,
.heading-wrap h2 {
  margin: 0;
  padding-bottom: 0.2em;
}

.heading-wrap h1:after,
.heading-wrap h2:after {
  content: '';
  /* border-bottom: 8px solid orange; */
  height: 8px;
  width: 100%;
  background: #ffd100;
  margin-left: -100%;
  bottom: 0;
  position: absolute;
}
</style>
