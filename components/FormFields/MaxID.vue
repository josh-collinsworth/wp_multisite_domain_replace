<script>
import FormField from './FormField'
export default {
  name: 'MaxID',
  extends: FormField,
  methods: {
    validate(val) {
      return val && val > 1
    }
  }
}
</script>
