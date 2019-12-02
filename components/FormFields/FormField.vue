<template>
  <div class="form-group">
    <label :for="name"><slot> </slot></label>
    <input
      :id="name"
      v-model="value"
      :class="!valid && modified ? 'form-up' : ''"
      :placeholder="placeholder"
      :type="type"
      :min="type == 'number' ? 2 : null"
      @blur="setModified"
    />
    <p>
      <transition name="fade-up">
        <b v-if="modified && !valid" class="error">
          {{ errorMessage }}
        </b>
      </transition>
    </p>
  </div>
</template>

<script>
export default {
  name: 'FormField',
  props: {
    submitted: {
      type: Boolean,
      default: false
    },
    name: {
      type: String,
      default: 'group'
    },
    type: {
      type: String,
      default: 'text'
    },
    placeholder: {
      type: String,
      default: 'Input value here'
    },
    emitted: {
      type: String,
      default: 'unnamedGroup'
    },
    errorMessage: {
      type: String,
      default: 'The provided value was not valid.'
    },
    ignoreValidation: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      value: '',
      modified: false
    }
  },
  computed: {
    valid() {
      if (this.value) {
        return this.validate(this.value)
      } else {
        return false
      }
    }
  },
  watch: {
    value: function(newValue, oldValue) {
      this.$emit(this.emitted, {
        valid: this.valid,
        data: newValue
      })
    }
  },
  methods: {
    setModified() {
      this.modified = true
    },
    validate(val) {
      return val.match(/.*/)
    }
  }
}
</script>

<style scoped>
label {
  font-size: 1.1em;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: baseline;
  width: 100%;
  margin: 1.5em 0 0.3em;
  font-family: 'AmsiProNormal-Ultra';
}
input[type='text'],
input[type='number'] {
  padding: 1em 0.7em;
  width: 100%;
  font-family: 'Sagona-BookItalic';
  font-size: 0.9em;
  font-style: normal;
}
.form-up {
  border-color: red;
}
p {
  margin: 0.4em 0;
  font-size: 0.9em;
}
.error {
  line-height: 1.2;
  color: red;
  display: inline-block;
}
.form-group {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
</style>
