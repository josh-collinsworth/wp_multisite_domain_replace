<template>
  <transition name="fade-up" appear>
    <form @submit="submit">
      <H1
        >Update a WordPress Multisite Network's Domain with a Single
        SQL&nbsp;Command
      </H1>

      <p class="note"><nuxt-link to="/about">What is this?</nuxt-link></p>

      <br />

      <OldDomain
        :submitted="submitted"
        name="oldDomain"
        placeholder="www.olddomain.com"
        error-message="Invalid formatting. Be sure the domain's valid; doesn't contain “http://” or a slash; and that the to/from domains don't match."
        emitted="oldDomainIsValid"
        @oldDomainIsValid="oldDomainCheck"
      >
        Change my site's domain FROM this:
      </OldDomain>

      <NewDomain
        :submitted="submitted"
        name="newDomain"
        placeholder="www.newdomain.com"
        error-message="Invalid formatting. Be sure the domain's valid; doesn't contain “http://” or a slash; and that the to/from domains don't match."
        emitted="newDomainIsValid"
        @newDomainIsValid="newDomainCheck"
      >
        TO this:
      </NewDomain>

      <DBPrefix
        :submitted="submitted"
        name="dbPrefix"
        placeholder="wp_"
        error-message="A database prefix should end in an underscore"
        emitted="dbPrefixIsValid"
        :ignore-validation="advanced.allowNoUnderscore"
        @dbPrefixIsValid="dbPrefixCheck"
      >
        Database prefix:
        <nuxt-link class="small-link" to="faq/where-to-find-database-prefix"
          >Where do I find this?</nuxt-link
        >
      </DBPrefix>

      <MaxID
        :submitted="submitted"
        name="maxID"
        placeholder="2"
        error-message="The minimum is 2"
        emitted="maxIDIsValid"
        type="number"
        @maxIDIsValid="maxIDCheck"
      >
        Highest blog_id entry from blogs table:
        <nuxt-link class="small-link" to="/faq/where-to-find-highest-blog-id"
          >Where do I find this?</nuxt-link
        >
      </MaxID>

      <transition name="fade-up">
        <div v-if="showAdvanced" id="advanced">
          <div class="options-wrap">
            <input
              id="ignoreUnderscore"
              v-model="advanced.allowNoUnderscore"
              type="checkbox"
            />
            <label for="ignoreUnderscore"
              >Allow prefix without underscore <i>(rare)</i></label
            >
          </div>
          <div class="options-wrap">
            <input id="blogID1" v-model="advanced.blogID1" type="checkbox" />
            <label for="blogID1"
              >Include SQL for blog ID #1 <i>(rare)</i></label
            >
          </div>
        </div>
      </transition>

      <div id="button-bar">
        <button @click.prevent="showAdvanced = !showAdvanced">
          Show Advanced Options
        </button>

        <input
          id="generate"
          :disabled="!goodToGo"
          type="submit"
          value="Generate SQL"
        />
      </div>

      <transition name="fade-up">
        <div v-if="submitted" key="a">
          <pre><button @click="copySQL">Select All and Copy</button>
          <span id="theCommand">{{ spitOutTheSQL }}</span>
          </pre>
        </div>
        <div v-else key="b">
          <p class="note">
            <i
              >[Your SQL command will appear here once the form is submitted]</i
            >
          </p>
        </div>
      </transition>
    </form>
  </transition>
</template>

<script>
import OldDomain from '@/components/FormFields/OldDomain'
import NewDomain from '@/components/FormFields/NewDomain'
import DBPrefix from '@/components/FormFields/DBPrefix'
import MaxID from '@/components/FormFields/MaxID'
import H1 from '@/components/elements/H1'

export default {
  name: 'WPForm',
  components: { OldDomain, NewDomain, DBPrefix, MaxID, H1 },
  props: {},
  data() {
    return {
      oldDomain: '',
      validOldDomain: false,

      newDomain: '',
      validNewDomain: false,

      dbPrefix: '',
      validDBPrefix: false,

      maxID: '',
      validMaxID: false,

      showAdvanced: false,
      advanced: {
        allowNoUnderscore: false,
        blogID1: false
      },
      submitted: false
    }
  },
  computed: {
    goodToGo() {
      return (
        this.validOldDomain &&
        this.validNewDomain &&
        this.oldDomain !== this.newDomain &&
        this.validDBPrefix &&
        this.validMaxID
      )
    },
    spitOutTheSQL() {
      let compiledSQL = `
UPDATE ${this.dbPrefix}blogs
SET domain = REPLACE(domain, '${this.oldDomain}', '${this.newDomain}')
WHERE domain LIKE '%${this.oldDomain}%';
UPDATE ${this.dbPrefix}options
SET option_value = REPLACE(option_value, '${this.oldDomain}', '${this.newDomain}')
WHERE option_value LIKE '%${this.oldDomain}%' AND option_name = 'siteurl';
UPDATE ${this.dbPrefix}options
SET option_value = REPLACE(option_value, '${this.oldDomain}', '${this.newDomain}')
WHERE option_value LIKE '%${this.oldDomain}%' AND option_name = 'home';
UPDATE ${this.dbPrefix}site
SET domain = REPLACE(domain, '${this.oldDomain}', '${this.newDomain}')
WHERE domain LIKE '%${this.oldDomain}%';
UPDATE ${this.dbPrefix}sitemeta
SET meta_value = REPLACE(meta_value, '${this.oldDomain}', '${this.newDomain}')
WHERE meta_value LIKE '%${this.oldDomain}%';`

      for (let i = this.advanced.blogID1 ? 1 : 2; i <= this.maxID; i++) {
        compiledSQL += `

UPDATE ${this.dbPrefix}${i}_options
SET option_value = REPLACE(option_value, '${this.oldDomain}', '${this.newDomain}')
WHERE option_value LIKE '%${this.oldDomain}%' AND option_name = 'siteurl'; 
UPDATE ${this.dbPrefix}${i}_options
SET option_value = REPLACE(option_value, '${this.oldDomain}', '${this.newDomain}')
WHERE option_value LIKE '%${this.oldDomain}%' AND option_name = 'home';`
      }
      return compiledSQL
    }
  },
  watch: {
    oldDomain: function(newVal, oldVal) {
      this.submitted = newVal !== oldVal ? false : this.submitted
    },
    newDomain: function(newVal, oldVal) {
      this.submitted = newVal !== oldVal ? false : this.submitted
    },
    dbPrefix: function(newVal, oldVal) {
      this.submitted = newVal !== oldVal ? false : this.submitted
    },
    maxID: function(newVal, oldVal) {
      this.submitted = newVal !== oldVal ? false : this.submitted
    }
  },
  methods: {
    oldDomainCheck(emitted) {
      this.validOldDomain = emitted.valid
      this.oldDomain = emitted.data
    },
    newDomainCheck(emitted) {
      this.validNewDomain = emitted.valid
      this.newDomain = emitted.data
    },
    dbPrefixCheck(emitted) {
      this.validDBPrefix = emitted.valid
      this.dbPrefix = emitted.data
    },
    maxIDCheck(emitted) {
      this.validMaxID = this.advanced.blogID1 ? true : emitted.valid
      this.maxID = emitted.data
    },
    submit(e) {
      e.preventDefault()
      if (this.goodToGo) {
        this.submitted = true
      }
    },
    copySQL() {
      const node = document.getElementById('theCommand')

      if (document.body.createTextRange) {
        const range = document.body.createTextRange()
        range.moveToElementText(node)
        range.select()
      } else if (window.getSelection) {
        const selection = window.getSelection()
        const range = document.createRange()
        range.selectNodeContents(node)
        selection.removeAllRanges()
        selection.addRange(range)
      } else {
        alert('Could not select text: Unsupported browser.')
      }
      document.execCommand('copy')
      setTimeout(() => {
        alert(
          `WARNING: THERE IS NO UNDOING AN SQL COMMAND! Make sure you get a backup of your database before running this command.\n\nSQL copied to clipboard!`
        )
      }, 20)
    }
  }
}
</script>

<style>
input[type='submit'],
button {
  font-family: 'AmsiProNormal-Ultra';
  transition: 0.1s ease;
  padding: 1em;
  font-size: 0.8em;
  font-weight: normal;
  background: #ffd100;
  margin: 0;
  color: #53565a;
  cursor: pointer;
  width: auto;
  border: none;
  border-radius: 4px;
}

input[type='submit']:not(:disabled):hover,
button:not(:disabled):hover {
  transform: scale(1.1);
}

input:disabled {
  opacity: 0.3;
  cursor: initial;
}

.expand-enter,
.expand-leave {
  opacity: 0;
  transform: translateY(2rem);
}

.expand-enter-active,
.expand-leave-active {
  opacity: 1;
  transform: translateY(0rem);
}

.fade-up-enter-active,
.fade-up-leave-active {
  transition: all 0.4s cubic-bezier(0, 0.38, 0.32, 1);
  transform: translateY(0em);
}

.fade-up-enter,
.fade-up-leave-to {
  transform: translateY(2em);
  opacity: 0;
  position: absolute;
}

#button-bar,
.options-wrap {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

#button-bar {
  margin-top: 1em;
}

.options-wrap label {
  margin: 0.5em 0;
  margin-left: 0.5em;
  cursor: pointer;
}

#advanced {
  background: #efefef;
  padding: 1rem;
  justify-content: flex-start;
  border-radius: 4px;
  margin: 2em 0;
}

#advanced label {
  font-family: 'AmsiProNormal-Ultra';
  font-weight: normal;
}

#advanced input[type='checkbox'] {
  position: absolute;
  opacity: 0;
  left: -100vw;
}

#advanced input[type='checkbox'] + label:before {
  content: '⬜';
  margin-right: 0.5em;
  transform: scale(1.2);
  display: inline-block;
  vertical-align: middle;
}

#advanced input[type='checkbox']:checked + label:before {
  content: '✅';
}

.options-wrap input[type='checkbox']:focus + label {
  outline: 2px dotted #ff6a13;
  outline-offset: 3px;
}

textarea#secretTextCopySource {
  opacity: 0;
  position: absolute;
  left: -100vh;
  width: 1px;
  height: 1px;
  /* visibility: hidden; */
}

.small-link {
  font-family: 'Sagona-BookItalic', serif;
  float: right;
  font-size: 0.7em;
  font-weight: normal;
}

.note,
.note a {
  margin: 3em 0;
  font-family: 'Sagona-BookItalic', serif !important;
}

.heading-wrap + .note {
  margin: -1em 0 1em;
}

i {
  font-family: 'Sagona-BookItalic';
}

::placeholder {
  color: #aaa;
  font-style: italic;
}
</style>
