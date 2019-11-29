<template>
  <div>
    <h2 class="faq-question">
      <a href="#" :aria-expanded="expanded" @click.prevent="toggleAccordion">
        {{ question }}
        <span
          role="img"
          :aria-label="expanded ? 'expanded' : 'collapsed'"
          :style="{ transform: expanded ? 'scaleY(-1)' : 'scaleY(1)' }"
          >🔼</span
        >
      </a>
    </h2>
    <div
      ref="drawer"
      class="faq-answer"
      :style="{
        height: expanded ? height : '2px',
        transition: transition,
        opacity: loaded
      }"
    >
      <transition name="accordion" mode="out-in">
        <div v-if="expanded">
          <slot></slot>
          <button class="faq-close" @click.prevent="toggleAccordion">
            Close 🔼
          </button>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Accordion',
  components: {},
  props: {
    question: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      expanded: true,
      height: 'undefined',
      transition: 'none',
      loaded: 0
    }
  },
  mounted() {
    this.height = getComputedStyle(this.$refs.drawer).height
    this.expanded = false
    this.transition = 'height 0.4s ease-in-out'
    setTimeout(() => {
      this.$nextTick(() => {
        this.loaded = 1
      })
    }, 300)
    setTimeout(() => {}, 2000)
  },
  methods: {
    toggleAccordion() {
      this.expanded = !this.expanded
    }
  }
}
</script>

<style scoped>
h2 {
  margin: 1em 0;
}

h2 a {
  font-size: 1.3rem;
  text-decoration: none;
  padding: 0.25em 0 0;
  border-bottom: 2px solid #ffd100;
  display: inline-block;
}

.faq-question a span[role='img'] {
  transition: all 0.4s ease;
  display: inline-block;
}

.faq-answer {
  position: relative;
}

.accordion-enter-active,
.accordion-leave-active {
  transition: all 0.4s ease-out;
}

.accordion-enter,
.accordion-leave-to {
  transform: translateY(-2em);
  opacity: 0;
  position: absolute;
  width: 100%;
}

button {
  cursor: pointer;
  background: #ffd100;
  padding: 0.5em;
  border-radius: 4px;
  border: none;
  font-weight: normal;
  font-family: 'AmsiProNormal-Ultra';
  font-size: 0.8rem;
  margin: 1em 0;
}

.faq-close {
  margin-bottom: 4em;
}
</style>
