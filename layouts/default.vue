<template>
  <div>
    <Header />
    <div class="container">
      <main>
        <transition name="fader">
          <nuxt />
        </transition>
      </main>
    </div>
    <Footer />
  </div>
</template>

<script>
import Footer from '../components/Footer'
import Header from '../components/Header'
export default {
  name: 'Default',
  components: { Header, Footer }
}
</script>

<style>
html {
  font-family: 'AmsiProNormal-Regular', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 18px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
  color: #53565a;
}

*:focus {
  outline-color: #ffd100;
}

body {
  min-height: 100vh;
}

h1 {
  font-size: 2.6em;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: 'AmsiProNormal-Ultra';
  font-weight: normal;
  margin: 1em 0;
}

b,
strong {
  font-family: 'AmsiProNormal-Ultra';
  font-weight: normal;
}

p {
  margin: 1em 0;
  line-height: 1.6em;
}

ul {
  line-height: 1.6em;
}

a {
  font-family: 'AmsiProNormal-Ultra';
  font-weight: normal;
  text-decoration-color: #ffd100;
}

p em,
p i {
  font-style: normal;
  font-family: 'Sagona-BookItalic';
}

code {
  background: #e0ebf0;
  border-radius: 4px;
  padding: 0.1em 0.3em;
  font-size: 0.9em;
}

.container {
  width: 100%;
  margin: auto;
  max-width: 34em;
  padding: 0 1em;
}

input {
  border: 2px solid #a7a8aa;
  border-radius: 4px;
  font-family: 'Sagona-BookItalic';
}

main {
  min-height: calc(100vh - (160px + 8rem));
}

pre {
  margin: 2rem 0;
  border-radius: 4px;
  background: #e0ebf0;
  transition: all 0.3s ease;
  padding: 1em;
  opacity: 1;
  transform: translateY(0);
  overflow: auto;
  font-size: 0.9rem;
  line-height: 1.5;
}

.fader-enter-active,
.fader-leave-active {
  transition-delay: 0.3s;
  transition: all 0.5s cubic-bezier(0, 0.32, 0.32, 0.99);
  opacity: 1;
  transform: translateY(0vh);
}

.fader-enter,
.fader-leave-to {
  opacity: 0;
  transform: translateY(3vh);
}
</style>
