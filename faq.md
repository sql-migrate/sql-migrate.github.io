---
title: FAQ - PostgresCompare
description: Frequently asked questions about PostgresCompare. Get answers to common questions about installation, licensing, and troubleshooting.
---

<section class="page-hero">
  <h1>Frequently Asked <span>Questions</span></h1>
  <p>Find answers to common questions about PostgresCompare.</p>
</section>

<section style="padding: 3rem 2rem; max-width: 900px; margin: 0 auto;">
  <div class="faq-section" style="padding: 0; border: none; background: none;">
    <div style="display: flex; flex-direction: column; gap: 1.5rem;">

      <div class="faq-item">
        <div class="faq-question">PostgresCompare is reporting that I have reached my maximum installs, what do I do?</div>
        <div class="faq-answer">Drop us an <a href="mailto:neil@postgrescompare.com">email</a>, we will reset your license key so you are good to go again.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question">PostgresCompare is not responding or just showing a spinner, how do I fix it?</div>
        <div class="faq-answer">
          <p>Sometimes (after the app updates for example) the PostgresCompare background process does not restart successfully which can lead to the UI spinning and failing to load projects.</p>
          <p style="margin-top: 1rem;">Fix the issue by finding the process "PostgresCompareWebApi" and killing it, then restart the PostgresCompare app. If the problem persists try restarting your machine.</p>
        </div>
      </div>

      <div class="faq-item">
        <div class="faq-question">PostgresCompare recently updated and now my projects are not showing up, how do I fix it?</div>
        <div class="faq-answer">
          <p>PostgresCompare relies on the operating system to tell it where to store your data. Sometimes this can change leading to a new database file being created.</p>
          <p style="margin-top: 1rem;">To recover your data you should:</p>
          <ol style="margin-top: 0.5rem; padding-left: 1.5rem;">
            <li style="margin-bottom: 0.5rem;">Search your machine for the file "postgrescompare_7.db". There should be 2 instances of a file with that name. Note which file is the older one, it probably also has a larger file size.</li>
            <li style="margin-bottom: 0.5rem;">Copy the old postgrescompare_7.db into the new location, overwriting the db file there.</li>
            <li>Restart PostgresCompare</li>
          </ol>
        </div>
      </div>

      <div class="faq-item">
        <div class="faq-question">Which PostgreSQL versions are supported?</div>
        <div class="faq-answer">PostgresCompare supports PostgreSQL version 9.2 and later, including the latest PostgreSQL 16. We regularly update to support new PostgreSQL releases.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question">Is my data safe? Does PostgresCompare send data to the cloud?</div>
        <div class="faq-answer">All comparisons happen locally on your machine. Your database credentials and data never leave your network. PostgresCompare connects directly to your PostgreSQL databases and processes everything locally.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question">Can I use PostgresCompare for free?</div>
        <div class="faq-answer">Yes! We offer a free 30-day trial with full access to all features. No credit card required. After the trial, you can purchase a monthly or annual subscription to continue using the software. <a href="/downloads">Download the trial here</a>.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question">How do I cancel my subscription?</div>
        <div class="faq-answer">Drop us an <a href="/contact">email</a> and we'll take care of your cancellation right away. We also offer a 30-day money-back guarantee if you're not satisfied.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question">What does "2 locations per user" mean?</div>
        <div class="faq-answer">You can install PostgresCompare on two different machines (e.g., work laptop and home desktop) with a single license. This gives you flexibility to use the tool wherever you need it.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question">Can I use PostgresCompare on macOS, Windows, and Linux?</div>
        <div class="faq-answer">Yes! PostgresCompare is available for all three major platforms. We also provide a command-line interface (CLI) for automation and CI/CD pipeline integration. <a href="/downloads">Download for your platform</a>.</div>
      </div>

      <div class="faq-item">
        <div class="faq-question">Have a question that's not answered here?</div>
        <div class="faq-answer"><a href="/contact">Contact us</a> and we'll be happy to help!</div>
      </div>

    </div>
  </div>
</section>
