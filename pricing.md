---
title: Pricing
heading: Purchase your PostgresCompare licence
description: After checkout is complete you'll receive your licence key via email.
pricing_table:
  - name: Monthly user
    color: "#8e8e8e"
    features:
      - text: $10 per month
        highlight: true
      - text: Access to the latest version of PostgresCompare
        highlight: false
      - text: 2 active installs per user
        highlight: false
      - text: Support via email
        highlight: false
      - text: 30 day money back guarantee
        highlight: false
    call_to_action:
      link: http://mysite.com?plan=basic
      text: Get started
    plan: plan_F7LHPebph3tpfU
  - name: Annual user
    color: "#4a4a4a"
    features:
      - text: $100 per year
        highlight: true
      - text: Access to the latest version of PostgresCompare
        highlight: false
      - text: 2 active installs per user
        highlight: false
      - text: Support via email
        highlight: false
      - text: 30 day money back guarantee
        highlight: false
    call_to_action:
      link: http://mysite.com?plan=pro
      text: Get started
    plan: plan_F8N7x2Jh0miMkY
<!-- faq:
  - question: What types of payment do you accept?
    answer: Credit cards including MasterCard, Visa or American Express.
  - question: Can I change my plan at a later time?
    answer: Yes, you can upgrade and downgrade your plan at anytime.
  - question: Do you offer pricing for nonprofit organizations?
    answer: Yes, send us a message and we'll set you up on our nonprofit pricing.
  - question: Questions?
    answer: Contact us for any further questions at <a href="#">john@business.com</a>. -->
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
				<li class="pricing-cta"><div class="button"><a id="{{plan.plan}}" style="background: {{ plan.color }}" href="#">{{ plan.call_to_action.text }} &rarr;</a></div></li>
			{% endif %}
		</ul>
	{% endfor %}
</div>


<script>
  var stripe = Stripe('pk_live_WBgGhfOBShwMGAXipP1KHl7u');

  var monthlyPlanButton = document.getElementById('plan_F7LHPebph3tpfU');
  monthlyPlanButton.addEventListener('click', function () {
    // When the customer clicks on the button, redirect
    // them to Checkout.
    stripe.redirectToCheckout({
      items: [{plan: 'plan_F7LHPebph3tpfU', quantity: 1}],
      successUrl: 'https://www.postgrescompare.com/success',
      cancelUrl: 'https://www.postgrescompare.com/',
    });
    return false;
  });

  var annualPlanButton = document.getElementById('plan_F8N7x2Jh0miMkY');
  annualPlanButton.addEventListener('click', function () {
    // When the customer clicks on the button, redirect
    // them to Checkout.
    stripe.redirectToCheckout({
      items: [{plan: 'plan_F8N7x2Jh0miMkY', quantity: 1}],
      successUrl: 'https://www.postgrescompare.com/success',
      cancelUrl: 'https://www.postgrescompare.com/',
    });
    return false;
  });
</script>

{% if page.faq %}
	<h2>Pricing FAQ</h2>
	<dl class="faq">
		{% for item in page.faq %}
			<div>
				<dt>{{ item.question }}</dt>
				<dd>{{ item.answer }}</dd>
			</div>
		{% endfor %}
	</dl>
{% endif %}
