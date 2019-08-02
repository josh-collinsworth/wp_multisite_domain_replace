<template>
  <div>
    <H1>FAQ</H1>
    <Accordion question="What is this site for? What does it do?">
      <p>
        Good question! It’s rare that you would need to update your WordPress
        multisite’s domain (it usually happens when moving between the live site
        and a local or staging environment), but it’s very troublesome to do,
        and it involves manually updating many different entries in the
        multisite’s database. That can be tricky even for advanced users, and
        downright scary for somebody who isn’t well-acquainted with WordPress
        multisite databases.
      </p>
      <p>
        This site makes things easier by generating a single SQL query that you
        can run in the database to automatically make all the updates for you,
        so that you shouldn’t have to edit database field values manually at
        all.
      </p>
    </Accordion>

    <Accordion
      question="What specific parts of my multisite database are being updated when I run this command?"
    >
      <p>
        Only your main domain (what you enter in the “replace this domain”
        field) in these specific tables and columns:
      </p>
      <p>
        <code>wp_options</code>: the rows named “siteurl” and “home”<br />
        <code>wp_site</code><br />
        <code>wp_sitemeta</code>: the row named “siteurl”<br />
        <code>wp_blogs</code>: all entries in the “domains” column that have the
        domain<br />
        <code>wp_#_options</code>: the “siteurl” and “home” rows. (“#” equals
        each of your subsite’s ID numbers.)
      </p>
    </Accordion>

    <Accordion
      id="faq-table-prefix"
      question="Where do I find my database table prefix?"
    >
      <p>
        There are a few of places to find this. The most certain way is to look
        inside your <strong>wp-config.php</strong> file, which you’ll find in
        the root folder of your WordPress install.
      </p>
      <p>
        You should see a line in that file (usually about halfway down) that
        begins with <code>$table_prefix</code>. The text in quotes following
        that is what you’re looking for. In the example below (and generally by
        default), this is <code>wp_</code>.
      </p>
      <pre>$table_prefix = 'wp_';</pre>
      <p>
        You can also view your database directly through phpMyAdmin or your
        host's database manager. Your database prefix will be the text which is
        shared by <em>all</em> of the WordPress tables.
      </p>
      <p>
        Managed hosts may also have a place to view your database prefix from
        the site’s dashboard. (The database prefix of
        <a href="http://getflywheel.com" target="_blank">Flywheel</a> sites, for
        example, can be found in the Advanced tab of the site’s Flywheel
        dashboard.)
      </p>
    </Accordion>

    <Accordion
      id="faq-blog_id"
      question="How do I find the highest blog_id number?"
    >
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
          <code>wp_blogs</code> table. (If your database prefix is something
          other than <code>wp_</code>, substitute your database prefix. You can
          locate the table easily once you’re in the database by using your
          browser’s find function for the word “blogs”.)
          <br />
          <br />Click into that table to view its data, and you should see that
          the first column is <code>blog_id</code>. You just want to find the
          highest number in that column;
        </li>
        <p><strong>OR:</strong></p>
        <li>
          Just put any number that you know is higher than the total number of
          subsites you’ve ever had. That may make the query very long (and it
          may therefore take a while to run), but it should do the trick.
        </li>
      </ul>
    </Accordion>

    <Accordion
      question="Will this tool work on both subsite and subdomain multisite setups?"
    >
      <p>
        <strong>Yes! </strong>Both subdomain (<code>subsite.mysite.com</code>)
        and subdirectory (<code>mysite.com/subsite</code>) domain configurations
        can be effectively updated with this tool.
      </p>
    </Accordion>

    <Accordion question="How do I know if I need to use the advanced options?">
      <p>
        If you're not sure, odds are you don't. It's extremely uncommon for a
        database prefix to <em>not</em> end in an underscore, and multisites
        don't use the blog ID number “1” by default.
      </p>
    </Accordion>

    <Accordion
      question="Ok, I've copied the SQL command. Now where do I run it?"
    >
      <p>
        You’ll need to access your site’s database manager. This might be
        phpMyAdmin, or your host may have a different database manager.
      </p>
      <p>
        Regardless, once you access the database, you should see a tab that says
        “SQL,” or possibly “Query.”
      </p>
      <p>
        If you click that, you should see a large text field to paste your
        command into.
      </p>
      <p>
        Every host is different, so while this general set of instructions
        should cover most cases, you may need to reach out to your host to find
        out how to run SQL commands and whether that’s possible.
      </p>
      <p>
        If you’re on <a href="https://getflywheel.com">Flywheel</a>, you’ll find
        your database in the Advanced tab of the site’s Flywheel dashboard.
        Click the “Manage” button (it will appear when you hover over the
        database card) then click the SQL tab in the next screen to find your
        SQL command input field.
      </p>
    </Accordion>

    <Accordion
      question="Why don't I want to include the “http://” portion of my domain?"
    >
      <p>
        With most search-and-replace commands, you’d probably want to include
        the protocol; that helps insure the command doesn’t replace things it
        shouldn’t. But he SQL command you’ll get from this tool is specifically
        written to
        <em>only </em>target the specific places where your multisite’s domain
        is found in the database, so there’s no worry of the command replacing
        things it shouldn’t.
      </p>
      <p>
        Some of those places do include the “http://” portion of the domain, but
        some do not. So in order to make the search as comprehensive and
        targeted as possible, it’s important to leave “http://” off of the
        domain so that the command will hit every instance of the domain that it
        needs to.
      </p>
    </Accordion>

    <Accordion
      question="I ran my SQL command and it didn't update everything on my site. Why?"
    >
      <p>
        See the
        <b
          >“What specific parts of my multisite database are being updated when
          I run this command?”</b
        >
        section above and check those locations to make sure the change was
        made.
      </p>
      <p>
        If not, your search term may have been inaccurate, or wasn’t targeted
        enough to get your main domain.
      </p>
      <p>
        But if so, the most likely explanation is that there are references to
        your old domain hard-coded into the site, or in places in the database
        that aren’t targeted.
      </p>
      <p>
        For example, if you (or your site’s developer) added an image link
        somewhere in WordPress or in a template file where the site’s old domain
        was explicitly typed out, that code may not be replaceable via standard
        search-and-replace methods, and may instead need to be manually updated.
      </p>
      <p>
        It could also mean that a plugin or theme has added extra database
        tables which aren’t included in the standard WordPress install, and
        which are therefore not included in this site’s search.
      </p>
      <p>
        It’s also worth double-checking that the proper database prefix was
        entered, that the blog_id number entered was high enough, and that the
        correct domain was entered for the search, with no “http://” or “/”
        before or after it.
      </p>
    </Accordion>

    <Accordion
      question="Can I use this tool to search/replace more than just my site's domain?"
    >
      <p>
        <strong>No. </strong>This tool is specifically designed for updating
        your WordPress multisite network’s domain only. That’s all it’s designed
        for.
      </p>
      <p>
        For general search-and-replace purposes, there are other plugins and
        pre-built SQL commands readily available.
      </p>
    </Accordion>

    <Accordion question="Will this tool work on domain-mapped multisites?">
      <p>
        <strong>No. </strong>Or rather: it will work to the extent possible, but
        it can’t have any impact on the mapped domains. Those will need to be
        updated manually, since the SQL command can’t possibly update them on
        its own.
      </p>
    </Accordion>

    <Accordion question="Can I use this tool on single WordPress sites?">
      <p>
        <b
          >Potentially, yes, but it’s not recommended and may not work as
          intended.</b
        >
      </p>
      <p>
        If used on a single site, only one part of the SQL command will succeed:
        the part that searches and replaces the
        <code>option_value </code>column of the <code>wp_options</code> table.
      </p>
      <p>
        If all of the links in your site are relative, this should be enough to
        effectively change the domain of a single WordPress site. However, it
        may have unintended side effects as well (it did in my testing), so I’d
        strongly recommend seeking out other options intended for standard,
        single WordPress sites. This tool just isn’t built for it.
      </p>
    </Accordion>

    <Accordion
      question="I'm getting SQL errors when I run the code. Why is that happening?"
    >
      <p>
        As long as most of the individual commands are running successfully,
        it’s ok that some of them fail. This will likely happen if there are
        skipped <code>blog_id</code> numbers in the <code>wp_blogs</code> table.
      </p>
      <p>
        For example, if the <code>blog_id</code> numbers of your subsites are
        <code>1</code>, <code>2</code> and <code>4</code>, the query that
        searches and replaces the subsite with the <code>blog_id</code> of
        <code>3</code> will fail, since that subsite doesn’t exist. That’s ok,
        though, and it shouldn’t stop the rest of the command from running
        successfully.
      </p>
      <p>
        If <em>none</em> of the command is working, double-check your database
        table prefix and the domain you’re searching, and confirm that you
        haven’t already updated the domain.
      </p>
    </Accordion>

    <Accordion
      question="My database prefix doesn't end with an underscore. What should I do?"
    >
      <p>
        It’s extremely rare that a WordPress site would have a database prefix
        which did <em>not </em>end in an underscore, but on the off-chance this
        happens to you, there’s an advanced option to allow it.
      </p>
    </Accordion>

    <Accordion
      question="Is there a risk to running the SQL command I get from this site?"
    >
      <p>
        <strong
          >There’s <em>always </em>risk in running SQL commands, so please do so
          with extreme caution.</strong
        >
        It is possible that running a bad SQL command could break your site
        entirely and/or make it inaccessible, although this tool is designed to
        absolutely minimize the chances of that happening and help you to run
        exactly the right command.
      </p>
      <p>
        Even so, you should always back up your database before running any SQL
        commands in case something goes wrong.
      </p>
      <p>
        WordPress multisite databases are very complex, and it’s impossible to
        predict all the variances that may occur either within their
        configuration or that may have been made by plugins and themes. So
        again, always have a database backup ready to go just in case.
      </p>
    </Accordion>

    <Accordion question="Can I go back from this to reverse the command?">
      <p>
        <strong>Yes.*</strong> If you just need to change your multisite’s
        domain temporarily and then change it back again later (such as to stage
        a site before going live), just use this tool again when you’re ready to
        switch back, and reverse the two domain fields.
      </p>
      <p>
        <strong>*That said:</strong> be careful. There isn’t any real “undo” in
        SQL. If the “new domain” field already exists in the database, you may
        get some weird results and catch extra things when going back.
      </p></Accordion
    >

    <Accordion question="What are the terms of use of this tool?">
      <p>
        You may use this site for anything you like, but it’s only designed to
        search and replace the domain of standard subdomain or subsite WordPress
        multisite networks.
      </p>
      <p>
        Whatever you do, however, by using this site you accept the
        responsibility for the risk involved. As explained above, while I’ve
        gone to great lengths to make this tool as easy to use and as foolproof
        as possible based on standard multisite network installations, WordPress
        multisite network databases are very complex, and there’s no way to know
        or predict how your multisite database is configured.
      </p>
      <p>
        Always get a database backup before running any SQL commands and you
        should be good. But regardless, you use this tool at your own risk.
      </p>
    </Accordion>
  </div>
</template>

<script>
import Accordion from '@/components/Accordion'
import H1 from '@/components/elements/H1'

export default {
  name: 'FAQ',
  components: { H1, Accordion }
}
</script>

<style scoped></style>
