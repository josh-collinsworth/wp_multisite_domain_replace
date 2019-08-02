<template>
  <div class="options-group">
    <label :for="slug">{{ label }}</label>
    <input
      :id="slug"
      v-model="value"
      :class="!validOldDomain ? 'form-up' : ''"
      placeholder="www.olddomain.net"
      type="text"
      @blur="readyForValidation"
    />{{ valid }}
    <transition name="fade-up">
      <p v-if="!validOldDomain" key="!validOldDomain" class="error">
        Please enter a valid domain <em>without</em> protocol, e.g.,
        <code>olddomain.net</code>
      </p>
      <p v-else key="validOldDomain" class="error">&nbsp;</p>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'OptionsGroup',
  props: {
    slug: {
      type: String,
      default: 'empty'
    },
    isValid: {
      type: RegExp,
      default: /.*/
    },
    label: {
      type: String,
      default: 'Label'
    }
  },
  data() {
    return {
      value: '',
      valid: false
    }
  },
  computed: {},
  methods: {
    readyForValidation() {
      this.valid = this.value.match(this.isValid)
    }
  }
}
</script>

<style scoped>
.fade-up-enter-active,
.fade-up-leave-active {
  transition: all 0.2s ease;
  transform: translateY(0em);
}

.fade-up-enter, .fade-up-leave-to
  /* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateY(1em);
  opacity: 0;
  position: absolute;
}
</style>
