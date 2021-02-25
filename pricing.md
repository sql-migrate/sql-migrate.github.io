---
title: Pricing
heading: Purchase your PostgresCompare subscription
description: After checkout is complete you'll receive your license key via email.
pricing_table:
  - name: Monthly subscription
    color: "#8e8e8e"
    features:
      - text: $14.95 per user per month
        highlight: true
      - text: Always free upgrades
        highlight: false
      - text: Install in 2 locations per user
        highlight: false
      - text: Support via email
        highlight: false
      - text: 30 day money back guarantee
        highlight: false
    call_to_action:
      text: PURCHASE MONTHLY
      link: "#"
    plan: price_1IOXkqKhvEuHFknZlCwbX1eV
  - name: Annual subscription
    color: "#4282ef"
    features:
      - text: $149.95 per user per year
        highlight: true
      - text: Always free upgrades
        highlight: false
      - text: Install in 2 locations per user
        highlight: false
      - text: Support via email
        highlight: false
      - text: 30 day money back guarantee
        highlight: false
    call_to_action:
      link: "#"
      text: PURCHASE ANNUAL
    plan: price_1IOXlOKhvEuHFknZBrJxEeZP
  - name: Need something else?
    color: "#8e8e8e"
    features:
      - text: ""
        highlight: true
      - text: Require a quote?
        highlight: false
      - text: Or an invoice?
        highlight: false
      - text: Multiple licenses?
        highlight: false
      - text: ""
        highlight: false
      - text: ""
        highlight: false
    call_to_action:
      link: mailto:neil@postgrescompare.com
      text: GET IN TOUCH
faq:
  - question: How do I cancel my subscription?
    answer: Drop us an <a href="/contact">email</a> and we'll take care of your cancellation right away.
  - question: Questions?
    answer: For any further questions <a href="/contact">contact us</a>.
---

<script src="https://js.stripe.com/v3"></script>

<div class="plans">
	{% for plan in page.pricing_table %}
		<ul class="plan">
			<li style="background: {{ plan.color }}">
				<h3>{{ plan.name }}</h3>
			</li>
			{% for feature in plan.features %}
				<li {% if feature.highlight %} class="highlighted"{% endif %}>{{ feature.text }}</li>
			{% endfor %}

    		{% if plan.call_to_action %}
    			<li class="pricing-cta"><div class="button"><a id="{{plan.plan}}" style="background: {{ plan.color }}" href="{{plan.call_to_action.link}}">{{ plan.call_to_action.text }}</a></div></li>
    		{% endif %}
    	</ul>
    {% endfor %}

</div>

<h2>Pricing FAQ</h2>
<dl class="faq">
		<div>
			<dt>Who is processing my payment?</dt>
			<dd>We use <a href="https://www.stripe.com">Stripe</a> to manage your subscription and payments.</dd>
		</div>

    	<div>
    		<dt>How do I cancel my subscription?</dt>
    		<dd>Drop us an <a href="/contact">email</a> and we'll take care of your cancellation right away.</dd>
    	</div>

</dl>

<script>
  var stripe = Stripe('pk_live_WBgGhfOBShwMGAXipP1KHl7u');

  var monthlyPlanButton = document.getElementById('plan_H5jWkJgAr05v7C');
  monthlyPlanButton.addEventListener('click', function () {
    // When the customer clicks on the button, redirect
    // them to Checkout.
    stripe.redirectToCheckout({
      items: [{plan: 'plan_H5jWkJgAr05v7C', quantity: 1}],
      successUrl: 'https://www.postgrescompare.com/success',
      cancelUrl: 'https://www.postgrescompare.com/',
    });
    return false;
  });

  var annualPlanButton = document.getElementById('plan_H5jV4xSixPYKJw');
  annualPlanButton.addEventListener('click', function () {
    // When the customer clicks on the button, redirect
    // them to Checkout.
    stripe.redirectToCheckout({
      items: [{plan: 'plan_H5jV4xSixPYKJw', quantity: 1}],
      successUrl: 'https://www.postgrescompare.com/success',
      cancelUrl: 'https://www.postgrescompare.com/',
    });
    return false;
  });
</script>
