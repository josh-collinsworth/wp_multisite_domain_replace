<template>
  <div>
    <H1 class="faq-question">How do I find the highest blog_id number?</H1>
    <p>
      <strong
        >This is usually the same as the total number of subsites you have in
        your WordPress multisite network.</strong
      >
      So you can do one of two things here. Either:
    </p>
    <ul>
      <li>
        Access your WordPress multisite’s database and find the
        <code>wp_blogs</code> table. (If your database prefix is something other
        than <code>wp_</code>, substitute your database prefix. You can locate
        the table easily once you’re in the database by using your browser’s
        find function for the word “blogs.”) <br />
        <br />Click into that table to view its data, and you should see that
        the first column is <code>blog_id</code>. You just want to find the
        highest number in that column;
      </li>
      <p><strong>OR:</strong></p>
      <li>
        Just put any number that you know is higher than the total number of
        subsites you’ve ever had. That may make the query very long (and it may
        therefore take a while to run), but it should do the trick.
      </li>
    </ul>

    <BackToFAQs />
  </div>
</template>

<script>
import BackToFAQs from '@/components/BackToFAQs'
import H1 from '@/components/elements/H1'

export default {
  components: { H1, BackToFAQs }
}
</script>
