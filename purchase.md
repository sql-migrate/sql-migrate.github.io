---
title: "PostgresCompare pricing — free forever, Pro at $29.95/month"
description: "Comparing, scripting and deploying are free forever. History, pipelines and the pgc CLI are what you pay for. Monthly or annual, cancel anytime."
---

<script src="https://js.stripe.com/v3"></script>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "PostgresCompare",
  "description": "PostgreSQL database schema comparison tool. Compare schemas, generate deployment SQL, and keep databases in sync.",
  "image": "https://www.postgrescompare.com/images/postgrescomparescreenshot.png",
  "brand": {
    "@type": "Brand",
    "name": "PostgresCompare"
  },
  "offers": [
    {
      "@type": "Offer",
      "name": "Free tier",
      "price": "0",
      "priceCurrency": "USD",
      "availability": "https://schema.org/InStock",
      "url": "https://www.postgrescompare.com/downloads"
    },
    {
      "@type": "Offer",
      "name": "Pro — monthly subscription",
      "price": "29.95",
      "priceCurrency": "USD",
      "priceValidUntil": "2026-12-31",
      "availability": "https://schema.org/InStock",
      "url": "https://www.postgrescompare.com/purchase"
    },
    {
      "@type": "Offer",
      "name": "Pro — annual subscription",
      "price": "299.95",
      "priceCurrency": "USD",
      "priceValidUntil": "2026-12-31",
      "availability": "https://schema.org/InStock",
      "url": "https://www.postgrescompare.com/purchase"
    }
  ]
}
</script>

<section class="page-hero">
  <p class="hunk">@@ <b>pricing</b> @@</p>
  <h1>Free to compare. <span>Paid to automate.</span></h1>
  <p>Comparing, scripting and deploying are free forever. History, pipelines and the CLI are what you pay for.</p>
</section>

<section class="dw" style="padding-bottom: 3rem;">

  <div class="prices">
    <div class="price">
      <h3>Free</h3>
      <div class="amt">$0</div>
      <p class="note">Where your 14-day trial lands</p>
      <ul>
        <li>Unlimited comparisons, all 40+ object types</li>
        <li>Live databases, pg_dump files, migration folders</li>
        <li>Dependency-ordered deployment scripts</li>
        <li>Deploy, with destructive-change safeguards</li>
        <li>Data comparison, snapshots and exports</li>
        <li class="na">Holds one comparison at a time</li>
        <li class="na">No pipelines, no CLI</li>
      </ul>
      <a href="/downloads" class="d-btn d-btn-quiet">Download free</a>
    </div>

    <div class="price feat">
      <h3>Pro</h3>
      <div class="amt">$29.95 <small>/ user / month</small></div>
      <p class="note">Or $299.95 a year &mdash; saves $59.45</p>
      <ul>
        <li>Everything in Free</li>
        <li>Full comparison history and drift timelines</li>
        <li>Pipelines</li>
        <li>The pgc CLI, for CI and automation</li>
        <li>Parallel connections, include/exclude rules</li>
        <li>Pre- and post-deploy scripts</li>
        <li>Install on 2 machines per user</li>
        <li>Free upgrades while your subscription is active</li>
      </ul>
      <div class="buy-pair">
        <button type="button" id="buy-annual" class="d-btn d-btn-primary">Subscribe yearly &mdash; $299.95</button>
        <button type="button" id="buy-monthly" class="d-btn d-btn-quiet">Subscribe monthly &mdash; $29.95</button>
      </div>
    </div>

    <div class="price">
      <h3>Team</h3>
      <div class="amt">Volume <small>pricing</small></div>
      <p class="note">For teams and consultancies</p>
      <ul>
        <li>Everything in Pro</li>
        <li>Volume discounts</li>
        <li>Client-transferable licenses</li>
        <li>Invoice billing</li>
        <li>Priority support</li>
      </ul>
      <a href="/contact" class="d-btn d-btn-quiet">Talk to us</a>
    </div>
  </div>

  <p class="trust buy-trust">
    <span>cancel anytime</span><span>30-day money-back guarantee</span><span>payments handled by stripe</span>
  </p>

</section>

<section class="dw" style="padding-bottom: 4rem;">
  <p class="hunk">@@ <b>pricing questions</b> @@</p>
  <h2 class="d-h2">Before you buy.</h2>

  <div class="d-grid">
    <div class="cell">
      <h3>What do I get without paying?</h3>
      <p>Unlimited comparisons across all 40+ object types, the diff and DDL viewers, dependency-ordered deployment scripts, deploying those scripts, and data comparison with snapshots and exports. The free tier is a working tool, not a viewer.</p>
    </div>
    <div class="cell">
      <h3>What is the one-comparison limit?</h3>
      <p>The free tier holds one comparison at a time per project. Running a new one asks you to delete the existing one first &mdash; nothing is replaced silently. Comparisons you created during your trial are kept and don't count against the limit.</p>
    </div>
    <div class="cell">
      <h3>What happens when my trial ends?</h3>
      <p>Nothing stops working. After 14 days PostgresCompare becomes the free tier rather than expiring, and everything you made during the trial stays open and readable.</p>
    </div>
    <div class="cell">
      <h3>What does "2 machines per user" mean?</h3>
      <p>One subscription installs on two machines &mdash; a work laptop and a home desktop, say. If you reach the install limit, <a href="/contact">email us</a> and we'll reset your license key.</p>
    </div>
    <div class="cell">
      <h3>Can I transfer a license to a client?</h3>
      <p>On Team plans, yes. Consultancies buy in volume and hand a license to the client at the end of an engagement. <a href="/contact">Get in touch</a> and we'll set it up.</p>
    </div>
    <div class="cell">
      <h3>How do I cancel?</h3>
      <p>Drop us an <a href="/contact">email</a> and we'll take care of it right away. There's a 30-day money-back guarantee if it isn't for you, and the app falls back to the free tier rather than going dark.</p>
    </div>
  </div>
</section>

<script>
  var stripe = Stripe('pk_live_WBgGhfOBShwMGAXipP1KHl7u');

  function pgcCheckout(priceId) {
    stripe.redirectToCheckout({
      items: [{plan: priceId, quantity: 1}],
      successUrl: 'https://www.postgrescompare.com/success',
      cancelUrl: 'https://www.postgrescompare.com/purchase',
      billingAddressCollection: 'required'
    });
  }

  var monthlyButton = document.getElementById('buy-monthly');
  if (monthlyButton) {
    monthlyButton.addEventListener('click', function () {
      pgcCheckout('price_1U2gmnKhvEuHFknZ7qYklVhI');
      return false;
    });
  }

  var annualButton = document.getElementById('buy-annual');
  if (annualButton) {
    annualButton.addEventListener('click', function () {
      pgcCheckout('price_1U2gmSKhvEuHFknZrkJItaph');
      return false;
    });
  }
</script>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {"@type": "ListItem", "position": 1, "name": "Home", "item": "https://www.postgrescompare.com"},
    {"@type": "ListItem", "position": 2, "name": "Pricing", "item": "https://www.postgrescompare.com/purchase"}
  ]
}
</script>
