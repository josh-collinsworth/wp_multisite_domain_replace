<script>
import FormField from './FormField'
export default {
  name: 'DBPrefix',
  extends: FormField,
  methods: {
    validate(val) {
      return val.match(/_$/i) || this.ignoreValidation
    }
  }
}
</script>
