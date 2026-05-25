---
title: Order Form — PostgresCompare & CezanneHR
description: PostgresCompare subscription order form for CezanneHR.
layout: default
sitemap: false
permalink: /order/cezannehr/
---

<style>
  .of-page { max-width: 900px; margin: 0 auto; padding: 2rem 1.5rem 4rem; font-family: 'Inter', sans-serif; }
  .of-header { text-align: center; margin-bottom: 2.5rem; }
  .of-header h1 { font-size: 1.75rem; font-weight: 700; color: #1a1a2e; margin-bottom: 0.25rem; }
  .of-header p { color: #6b7280; font-size: 0.95rem; }
  .of-card { background: #fff; border: 1px solid #e5e7eb; border-radius: 10px; padding: 1.75rem 2rem; margin-bottom: 1.5rem; box-shadow: 0 1px 3px rgba(0,0,0,0.06); }
  .of-card h2 { font-size: 1rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.05em; color: #374151; margin: 0 0 1.25rem; padding-bottom: 0.75rem; border-bottom: 2px solid #f3f4f6; }
  .of-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
  .of-grid.full { grid-template-columns: 1fr; }
  .of-field { display: flex; flex-direction: column; gap: 0.35rem; }
  .of-field label { font-size: 0.8rem; font-weight: 600; color: #6b7280; text-transform: uppercase; letter-spacing: 0.04em; }
  .of-field input, .of-field textarea, .of-field select {
    padding: 0.55rem 0.75rem; border: 1px solid #d1d5db; border-radius: 6px;
    font-family: inherit; font-size: 0.95rem; color: #111827; background: #f9fafb;
    transition: border-color 0.15s;
  }
  .of-field input:focus, .of-field textarea:focus, .of-field select:focus {
    outline: none; border-color: #6366f1; background: #fff; box-shadow: 0 0 0 3px rgba(99,102,241,0.1);
  }
  .of-field textarea { min-height: 80px; resize: vertical; }
  .of-table { width: 100%; border-collapse: collapse; }
  .of-table th { text-align: left; font-size: 0.8rem; font-weight: 600; color: #6b7280; text-transform: uppercase; letter-spacing: 0.04em; padding: 0.5rem 0.75rem; border-bottom: 2px solid #f3f4f6; }
  .of-table td { padding: 0.6rem 0.75rem; border-bottom: 1px solid #f3f4f6; font-size: 0.95rem; vertical-align: middle; }
  .of-table td input { border: none; border-bottom: 1px solid #d1d5db; border-radius: 0; background: transparent; padding: 0.25rem 0; width: 100%; font-family: inherit; font-size: 0.95rem; }
  .of-table td input:focus { outline: none; border-bottom-color: #6366f1; }
  .of-table .total-row td { font-weight: 700; border-top: 2px solid #e5e7eb; border-bottom: none; padding-top: 0.85rem; }
  .sig-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; margin-top: 0.5rem; }
  .sig-col { display: flex; flex-direction: column; gap: 0.75rem; }
  .sig-col h3 { font-size: 0.85rem; font-weight: 700; color: #374151; text-transform: uppercase; letter-spacing: 0.04em; margin: 0; }
  .sig-line { border-bottom: 1px solid #9ca3af; margin-top: 2.5rem; padding-bottom: 0.25rem; font-size: 0.8rem; color: #9ca3af; }
  .sig-field { display: flex; flex-direction: column; gap: 0.25rem; }
  .sig-field label { font-size: 0.75rem; color: #9ca3af; }
  .sig-field input { border: none; border-bottom: 1px solid #d1d5db; border-radius: 0; background: transparent; font-family: inherit; font-size: 0.95rem; padding: 0.25rem 0; }
  .sig-field input:focus { outline: none; border-bottom-color: #6366f1; }
  .of-print-btn { display: flex; justify-content: center; margin: 2rem 0; }
  .of-print-btn button { padding: 0.75rem 2rem; background: #6366f1; color: #fff; border: none; border-radius: 6px; font-size: 1rem; font-weight: 600; cursor: pointer; font-family: inherit; }
  .of-print-btn button:hover { background: #4f46e5; }
  .tc-divider { border: none; border-top: 3px double #e5e7eb; margin: 3rem 0; }
  .tc-section { }
  .tc-section h1 { font-size: 1.4rem; font-weight: 700; color: #1a1a2e; margin-bottom: 0.25rem; }
  .tc-section > p { color: #6b7280; font-size: 0.9rem; margin-bottom: 2rem; }
  .tc-card { background: #fff; border: 1px solid #e5e7eb; border-radius: 10px; padding: 1.5rem 2rem; margin-bottom: 1.25rem; }
  .tc-card h3 { font-size: 1rem; font-weight: 700; color: #374151; margin: 0 0 1rem; }
  .tc-card ul { padding-left: 1.25rem; margin: 0; }
  .tc-card li { margin-bottom: 0.6rem; font-size: 0.95rem; line-height: 1.6; color: #374151; }
  .tc-card li ul { margin-top: 0.5rem; }
  .amended-badge { display: inline-block; font-size: 0.7rem; font-weight: 700; background: #fef3c7; color: #92400e; border: 1px solid #fcd34d; border-radius: 4px; padding: 0.1rem 0.4rem; margin-left: 0.5rem; vertical-align: middle; text-transform: uppercase; letter-spacing: 0.04em; }
  .new-badge { display: inline-block; font-size: 0.7rem; font-weight: 700; background: #d1fae5; color: #065f46; border: 1px solid #6ee7b7; border-radius: 4px; padding: 0.1rem 0.4rem; margin-left: 0.5rem; vertical-align: middle; text-transform: uppercase; letter-spacing: 0.04em; }
  @media (max-width: 640px) {
    .of-grid { grid-template-columns: 1fr; }
    .sig-grid { grid-template-columns: 1fr; }
  }
  @media print {
    header, footer, .of-print-btn, .amended-badge, .new-badge { display: none !important; }
    .of-page { padding: 0; }
    .of-card, .tc-card { border: 1px solid #ccc; box-shadow: none; break-inside: avoid; }
    .of-field input, .of-field textarea, .of-field select,
    .of-table td input, .sig-field input {
      border-color: #ccc; background: transparent;
    }
    .tc-divider { break-before: page; }
    body { font-size: 11pt; }
  }
</style>

<div class="of-page">

  <div class="of-header">
    <h1>Order Form</h1>
    <p>PostgresCompare — Subscription Agreement</p>
  </div>

  <!-- Supplier Details -->
  <div class="of-card">
    <h2>Supplier</h2>
    <div class="of-grid">
      <div class="of-field">
        <label>Company Name</label>
        <input type="text" value="PostgresCompare" readonly style="color:#6b7280;">
      </div>
      <div class="of-field">
        <label>Contact Email</label>
        <input type="text" value="neil@postgrescompare.com" readonly style="color:#6b7280;">
      </div>
      <div class="of-field">
        <label>Website</label>
        <input type="text" value="www.postgrescompare.com" readonly style="color:#6b7280;">
      </div>
    </div>
  </div>

  <!-- Customer Details -->
  <div class="of-card">
    <h2>Customer</h2>
    <div class="of-grid">
      <div class="of-field">
        <label>Company Name</label>
        <input type="text" value="CezanneHR" id="customer-name">
      </div>
      <div class="of-field">
        <label>Company Registration No.</label>
        <input type="text" id="reg-no" placeholder="e.g. 12345678">
      </div>
      <div class="of-field of-grid full" style="grid-column: 1 / -1;">
        <label>Registered Address</label>
        <textarea id="address" placeholder="Enter full registered address"></textarea>
      </div>
      <div class="of-field">
        <label>Authorised Signatory Name</label>
        <input type="text" id="signatory" placeholder="e.g. John Smith">
      </div>
      <div class="of-field">
        <label>Signatory Title / Role</label>
        <input type="text" id="signatory-title" placeholder="e.g. Chief Technology Officer">
      </div>
      <div class="of-field">
        <label>Email</label>
        <input type="email" id="customer-email" placeholder="e.g. john@cezannehr.com">
      </div>
    </div>
  </div>

  <!-- Order Details -->
  <div class="of-card">
    <h2>Order Details</h2>
    <table class="of-table">
      <thead>
        <tr>
          <th>Description</th>
          <th>Users</th>
          <th>Unit Price (per user / year)</th>
          <th>Total (USD)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>PostgresCompare — Annual Subscription</td>
          <td><input type="number" id="users" placeholder="0" min="1" style="width:70px;" oninput="calcTotal()"></td>
          <td>$149.95</td>
          <td id="total-display">—</td>
        </tr>
        <tr class="total-row">
          <td colspan="3">Annual Subscription Fee (USD)</td>
          <td id="annual-fee-display">—</td>
        </tr>
      </tbody>
    </table>
    <div class="of-grid" style="margin-top: 1.25rem;">
      <div class="of-field">
        <label>Subscription Start Date</label>
        <input type="date" id="start-date">
      </div>
      <div class="of-field">
        <label>Subscription Term</label>
        <input type="text" value="12 months" id="term">
      </div>
      <div class="of-field">
        <label>Renewal</label>
        <input type="text" value="Auto-renews annually unless cancelled" id="renewal">
      </div>
      <div class="of-field">
        <label>Payment Terms</label>
        <input type="text" value="Annual, invoiced in advance" id="payment-terms">
      </div>
    </div>
  </div>

  <!-- Signature -->
  <div class="of-card">
    <h2>Signatures</h2>
    <p style="font-size:0.9rem; color:#6b7280; margin-bottom:1.5rem;">
      By signing below, both parties agree to be bound by this Order Form and the Terms &amp; Conditions set out overleaf.
    </p>
    <div class="sig-grid">
      <div class="sig-col">
        <h3>For and on behalf of the Supplier</h3>
        <div class="sig-line">Signature</div>
        <div class="sig-field">
          <label>Name</label>
          <input type="text" value="Neil Anderson">
        </div>
        <div class="sig-field">
          <label>Title</label>
          <input type="text" value="Director, PostgresCompare">
        </div>
        <div class="sig-field">
          <label>Date</label>
          <input type="date">
        </div>
      </div>
      <div class="sig-col">
        <h3>For and on behalf of the Customer</h3>
        <div class="sig-line">Signature</div>
        <div class="sig-field">
          <label>Name</label>
          <input type="text" placeholder="John (CTO)">
        </div>
        <div class="sig-field">
          <label>Title</label>
          <input type="text" placeholder="Chief Technology Officer">
        </div>
        <div class="sig-field">
          <label>Date</label>
          <input type="date">
        </div>
      </div>
    </div>
  </div>

  <div class="of-print-btn">
    <button onclick="window.print()">Print / Save as PDF</button>
  </div>

  <hr class="tc-divider">

  <!-- Terms & Conditions -->
  <div class="tc-section">
    <h1>Terms &amp; Conditions</h1>
    <p>These terms and conditions govern the subscription to PostgresCompare. The software product is protected by copyright laws and international copyright treaties, as well as other intellectual property laws and treaties. The software product is licensed, not sold.</p>

    <div class="tc-card">
      <h3>1. LICENCES</h3>
      <ul>
        <li><strong>1.1 Installation and Usage:</strong> Licences are per user and valid for use on all supported operating systems. Licence keys may be used on multiple (2) computers and operating systems, provided the licence key holder is the primary user.</li>
        <li><strong>1.2 Backup Copies:</strong> You may make copies of the licence key and or PostgresCompare for backup and archival purposes.</li>
      </ul>
    </div>

    <div class="tc-card">
      <h3>2. DESCRIPTION OF OTHER RIGHTS AND LIMITATIONS</h3>
      <ul>
        <li><strong>2.1 Maintenance of Copyright Notices:</strong> You must not remove or alter any copyright notices on any copy of PostgresCompare.</li>
        <li><strong>2.2 Distribution:</strong> You may not distribute or sell licence keys or PostgresCompare to third parties. Licences will be revoked if distributed or sold to third parties.</li>
        <li><strong>2.3 Rental:</strong> You may not rent, lease, or lend the licence key or PostgresCompare.</li>
      </ul>
    </div>

    <div class="tc-card">
      <h3>3. COPYRIGHT</h3>
      <ul>
        <li><strong>3.1</strong> All title, including but not limited to copyrights, in and to PostgresCompare and any copies thereof are owned by PostgresCompare.</li>
      </ul>
    </div>

    <div class="tc-card">
      <h3>4. WARRANTIES</h3>
      <ul>
        <li><strong>4.1 The Supplier:</strong>
          <ul>
            <li>4.1.1 does not warrant that the Customer's use of the Subscription will be uninterrupted or error-free; or that the Subscription and/or the information obtained by the Customer through the Support Services will meet the Customer's requirements;</li>
            <li>4.1.2 is not responsible for any delays, delivery failures, or any other loss or damage resulting from the transfer of data over communications networks and facilities, including the internet, and the Customer acknowledges that the Subscription may be subject to limitations, delays and other problems inherent in the use of such communications facilities.</li>
          </ul>
        </li>
        <li><strong>4.2</strong> EXCEPT AS EXPRESSLY SET OUT IN THIS AGREEMENT, ALL CONDITIONS, WARRANTIES, TERMS AND UNDERTAKINGS, EXPRESS OR IMPLIED, WHETHER BY STATUTE, COMMON LAW, TRADE PRACTICE, CUSTOM, COURSE OF DEALING OR OTHERWISE (INCLUDING WITHOUT LIMITATION AS TO QUALITY, PERFORMANCE OR FITNESS OR SUITABILITY FOR PURPOSE) IN RESPECT OF THE SUBSCRIPTION ARE HEREBY EXCLUDED TO THE FULLEST EXTENT PERMISSIBLE BY LAW.</li>
      </ul>
    </div>

    <div class="tc-card">
      <h3>5. LIMITATION OF LIABILITY <span class="amended-badge">Amended</span></h3>
      <ul>
        <li><strong>5.1</strong> Nothing in this Agreement shall exclude or restrict the liability of either party to the other for death or personal injury resulting from the negligent act of that party or for liability for any fraudulent misrepresentation by that party to this Agreement.</li>
        <li><strong>5.2</strong> Subject to the provisions of clauses 5.1, 5.3 and 5.4, the liability of each party to the other for direct loss in contract, tort or otherwise arising out of or in connection with this Agreement or the other party's use of the Subscription shall be limited in respect of all claims arising to the Subscription Fee paid under this Agreement in respect of the Subscription provided in the 12 months prior to the event giving rise to the liability.</li>
        <li><strong>5.3</strong> SUBJECT TO THE PROVISIONS OF CLAUSE 5.1, IN NO CIRCUMSTANCES SHALL EITHER PARTY BE LIABLE TO THE OTHER WHETHER IN CONTRACT, TORT, NEGLIGENCE, BREACH OF STATUTORY DUTY OR OTHERWISE IN RESPECT OF:
          <ul>
            <li>5.3.1 LOSS OF PROFITS, ANTICIPATED SAVINGS, REVENUE, GOODWILL OR BUSINESS OPPORTUNITY;</li>
            <li>5.3.2 LOSS OR CORRUPTION OF OR COST OF RESTORATION OF DATA OR FOR USE OF ANY RESULTS OBTAINED BY USE OF THE SOFTWARE; OR</li>
            <li>5.3.3 ANY INDIRECT, CONSEQUENTIAL, FINANCIAL OR ECONOMIC LOSS OR DAMAGE, COSTS OR EXPENSES, WHATEVER OR HOWEVER ARISING OUT OF OR IN CONNECTION WITH THIS AGREEMENT OR THE OTHER PARTY'S USE OF THE SUBSCRIPTION OR DELIVERY OF THE SUBSCRIPTION SERVICES.</li>
          </ul>
        </li>
      </ul>
    </div>

    <div class="tc-card">
      <h3>6. PAYMENT AND PAYMENT TERMS</h3>
      <ul>
        <li><strong>6.1</strong> The Subscription Fee shall be due and payable by the Customer as specified in the Order Form.</li>
        <li><strong>6.2</strong> If the Customer does not pay the Subscription Fee or any other sums payable under this Agreement when due, the Supplier reserves the right to terminate the Subscription and prevent the Customer from accessing and/or using the Software immediately.</li>
      </ul>
    </div>

    <div class="tc-card">
      <h3>7. INTELLECTUAL PROPERTY RIGHTS AND OUTPUTS <span class="new-badge">New</span></h3>
      <ul>
        <li><strong>7.1 Customer IP.</strong> All intellectual property rights in and to:
          <ul>
            <li>(a) the Customer's software platform including all associated tools, methodologies, database structures, schemas, underlying code and documentation ("Customer IP"); and</li>
            <li>(b) any enhancements, modifications, or improvements thereto,</li>
          </ul>
          shall remain vested in Customer (or its licensors) at all times.
        </li>
        <li><strong>7.2 Outputs, Recommendations and Derivative Works.</strong> To the extent the tool is used to generate or support:
          <ul>
            <li>(a) recommendations, proposed changes, or updates to database code or structures;</li>
            <li>(b) scripts, queries, schema modifications, or configuration outputs; or</li>
            <li>(c) any other deliverables derived from or informed by Customer IP,</li>
          </ul>
          all intellectual property rights in such outputs and any resulting implementations, modifications, or updates shall vest in Customer upon creation. No rights are granted to the Supplier in respect of Customer IP, including any underlying logic, structure, or methodologies embedded within outputs.
        </li>
        <li><strong>7.3 Assignment.</strong> To the extent that any intellectual property rights in the outputs, recommendations, or resulting changes do not automatically vest in Customer, the Supplier hereby assigns (and shall procure the assignment of) such rights to Customer by way of present and future assignment.</li>
      </ul>
    </div>

  </div><!-- /.tc-section -->

</div><!-- /.of-page -->

<script>
function calcTotal() {
  var users = parseInt(document.getElementById('users').value, 10);
  var total = document.getElementById('total-display');
  var fee = document.getElementById('annual-fee-display');
  if (users > 0) {
    var amount = (users * 149.95).toFixed(2);
    total.textContent = '$' + amount;
    fee.textContent = '$' + amount;
  } else {
    total.textContent = '—';
    fee.textContent = '—';
  }
}
</script>
