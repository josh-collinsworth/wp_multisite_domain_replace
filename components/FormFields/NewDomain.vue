<script>
import FormField from './FormField'
export default {
  name: 'NewDomain',
  extends: FormField,
  methods: {
    validate(val) {
      return val.match(
        /^(?!https?:\/\/)[a-zA-Z0-9][a-zA-Z0-9\-_.]{1,63}[^\-_]\.[a-zA-Z]{2,}$/i
      )
    }
  }
}
</script>
