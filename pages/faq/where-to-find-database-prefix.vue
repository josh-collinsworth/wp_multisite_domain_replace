<template>
  <div>
    <H1 class="faq-question">Where do I find my database table prefix?</H1>
    <p>
      There are a few of places to find this. The most certain way is to look
      inside your <strong>wp-config.php</strong> file, which you’ll find in the
      root folder of your WordPress install.
    </p>
    <p>
      You should see a line in that file (usually about halfway down) that
      begins with <code>$table_prefix</code>. The text in quotes following that
      is what you’re looking for. In the example below (and generally by
      default), this is <code>wp_</code>.
    </p>
    <pre>$table_prefix = 'wp_';</pre>
    <p>
      You can also view your database directly through phpMyAdmin or your host's
      database manager. Your database prefix will be the text which is shared by
      <em>all</em> of the WordPress tables.
    </p>
    <p>
      Managed hosts may also have a place to view your database prefix from the
      site’s dashboard. (The database prefix of
      <a href="http://getflywheel.com" target="_blank">Flywheel</a> sites, for
      example, can be found in the Advanced tab of the site’s Flywheel
      dashboard.)
    </p>
    <BackToFAQs />
  </div>
</template>

<script>
import BackToFAQs from '@/components/BackToFAQs'
import H1 from '@/components/elements/H1'

export default {
  components: { BackToFAQs, H1 }
}
</script>
