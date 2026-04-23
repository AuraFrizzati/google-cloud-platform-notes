# BigQuery ML Pricing Calculator

This calculator estimates the cost of **BigQuery ML ARIMA_PLUS / ARIMA_PLUS_XREG** time-series model training queries for the **London (europe-west2)** region.

**Unit price:** $390.625 per TiB &nbsp;·&nbsp; Region: europe-west2

> **How it works:** When `AUTO_ARIMA` is enabled, BigQuery ML trains multiple candidate models in parallel. The bytes billed by your input `SELECT` statement are multiplied by the number of candidate models, which depends on `AUTO_ARIMA_MAX_ORDER` and the type of forecasting.

---

<div id="bqml-calculator">

<style>
#bqml-calculator {
  font-family: inherit;
  max-width: 640px;
}
#bqml-calculator .calc-card {
  border: 1px solid var(--md-default-fg-color--lightest, #e0e0e0);
  border-radius: 8px;
  padding: 1.5rem 1.75rem;
  background: var(--md-default-bg-color, #fff);
  box-shadow: 0 2px 6px rgba(0,0,0,0.07);
}
#bqml-calculator label {
  display: block;
  font-weight: 600;
  margin-top: 1.1rem;
  margin-bottom: 0.3rem;
  font-size: 0.92rem;
}
#bqml-calculator .input-row {
  display: flex;
  gap: 0.5rem;
  align-items: center;
}
#bqml-calculator input[type="number"],
#bqml-calculator select {
  border: 1px solid var(--md-default-fg-color--light, #aaa);
  border-radius: 4px;
  padding: 0.45rem 0.6rem;
  font-size: 0.95rem;
  background: var(--md-default-bg-color, #fff);
  color: var(--md-default-fg-color, #333);
  outline: none;
  transition: border-color 0.2s;
}
#bqml-calculator input[type="number"]:focus,
#bqml-calculator select:focus {
  border-color: #e65100;
}
#bqml-calculator input[type="number"] {
  flex: 1;
  min-width: 0;
}
#bqml-calculator select {
  min-width: 90px;
}
#bqml-calculator .hint {
  font-size: 0.8rem;
  color: var(--md-default-fg-color--light, #777);
  margin-top: 0.2rem;
}
#bqml-calculator #d-row {
  margin-top: 0.9rem;
  padding: 0.75rem 1rem;
  background: var(--md-code-bg-color, #f5f5f5);
  border-radius: 6px;
  border-left: 3px solid #e65100;
}
#bqml-calculator #d-row label {
  margin-top: 0;
}
#bqml-calculator button {
  margin-top: 1.4rem;
  background: #e65100;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 0.6rem 1.4rem;
  font-size: 0.97rem;
  font-weight: 700;
  cursor: pointer;
  letter-spacing: 0.02em;
  transition: background 0.2s;
}
#bqml-calculator button:hover {
  background: #bf360c;
}
#bqml-calculator #calc-result {
  display: none;
  margin-top: 1.4rem;
  padding: 1rem 1.25rem;
  border-radius: 6px;
  background: var(--md-code-bg-color, #f5f5f5);
  border-left: 4px solid #e65100;
}
#bqml-calculator #calc-result h4 {
  margin: 0 0 0.75rem 0;
  font-size: 1rem;
  color: #e65100;
}
#bqml-calculator .result-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
  padding: 0.22rem 0;
  border-bottom: 1px solid var(--md-default-fg-color--lightest, #eee);
}
#bqml-calculator .result-row:last-child {
  border-bottom: none;
}
#bqml-calculator .result-label {
  color: var(--md-default-fg-color--light, #555);
}
#bqml-calculator .result-value {
  font-weight: 600;
}
#bqml-calculator .result-total {
  margin-top: 0.75rem;
  font-size: 1.15rem;
  font-weight: 700;
  color: #e65100;
  text-align: right;
}
#bqml-calculator #calc-error {
  display: none;
  margin-top: 1rem;
  color: #c62828;
  font-size: 0.9rem;
}
</style>

<div class="calc-card">

  <label for="bytes-input">Bytes billed by the input SELECT statement</label>
  <div class="input-row">
    <input type="number" id="bytes-input" min="0" placeholder="e.g. 500" step="any" />
    <select id="unit-select">
      <option value="1">Bytes</option>
      <option value="1024">KiB (KB)</option>
      <option value="1048576" selected>MiB (MB)</option>
      <option value="1073741824">GiB (GB)</option>
      <option value="1099511627776">TiB (TB)</option>
    </select>
  </div>
  <p class="hint">Units follow binary prefixes (1 KiB = 1,024 B, 1 MiB = 1,024 KiB, …) consistent with BigQuery billing.</p>

  <label for="model-type">Forecasting model type</label>
  <select id="model-type">
    <option value="single">Single time series forecasting</option>
    <option value="multiple">Multiple time series forecasting (TIME_SERIES_ID_COL)</option>
  </select>

  <div id="d-row">
    <label for="d-select">Does non-seasonal <em>d</em> equal 1?</label>
    <select id="d-select">
      <option value="yes" selected>Yes (d = 1)</option>
      <option value="no">No (d ≠ 1)</option>
    </select>
    <p class="hint">Only relevant for single time series forecasting. For multiple time series this is ignored.</p>
  </div>

  <label for="arima-order">AUTO_ARIMA_MAX_ORDER</label>
  <select id="arima-order">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
    <option value="4">4</option>
    <option value="5" selected>5</option>
  </select>
  <p class="hint">Controls the maximum order of the ARIMA model and the number of candidate models evaluated.</p>

  <button onclick="calculateCost()">Calculate cost</button>

  <p id="calc-error">⚠ Please enter a valid positive number for bytes billed.</p>

  <div id="calc-result">
    <h4>Estimated Training Cost</h4>
    <div class="result-row">
      <span class="result-label">Bytes billed (input)</span>
      <span class="result-value" id="r-bytes-input"></span>
    </div>
    <div class="result-row">
      <span class="result-label">Equivalent in TiB</span>
      <span class="result-value" id="r-tib-input"></span>
    </div>
    <div class="result-row">
      <span class="result-label">Candidate models (multiplier)</span>
      <span class="result-value" id="r-candidates"></span>
    </div>
    <div class="result-row">
      <span class="result-label">Effective TiB billed</span>
      <span class="result-value" id="r-tib-effective"></span>
    </div>
    <div class="result-total" id="r-cost"></div>
  </div>

</div>

</div>

<script>
(function () {
  // Candidate model counts from GCP docs
  // Index 0 = AUTO_ARIMA_MAX_ORDER 1, index 4 = order 5
  // Multiple TS and single TS with d=1 both use the same multipliers
  var candidatesMultipleOrD1  = [6, 12, 20, 30, 42];  // multiple TS, or single TS with d = 1
  var candidatesSingleDNot1   = [3, 6, 10, 15, 21];   // single TS with d != 1

  var PRICE_PER_TIB = 390.625;  // USD, europe-west2
  var TIB_IN_BYTES  = 1099511627776;  // 2^40

  function formatBytes(bytes) {
    if (bytes < 1024) return bytes.toFixed(2) + ' B';
    if (bytes < 1048576) return (bytes / 1024).toFixed(2) + ' KiB';
    if (bytes < 1073741824) return (bytes / 1048576).toFixed(2) + ' MiB';
    if (bytes < 1099511627776) return (bytes / 1073741824).toFixed(4) + ' GiB';
    return (bytes / 1099511627776).toFixed(6) + ' TiB';
  }

  window.calculateCost = function () {
    var errorEl  = document.getElementById('calc-error');
    var resultEl = document.getElementById('calc-result');

    var rawValue  = parseFloat(document.getElementById('bytes-input').value);
    var unitMult  = parseFloat(document.getElementById('unit-select').value);
    var modelType = document.getElementById('model-type').value;
    var dChoice   = document.getElementById('d-select').value;
    var orderIdx  = parseInt(document.getElementById('arima-order').value, 10) - 1;

    errorEl.style.display  = 'none';
    resultEl.style.display = 'none';

    if (isNaN(rawValue) || rawValue <= 0) {
      errorEl.style.display = 'block';
      return;
    }

    var totalBytes = rawValue * unitMult;

    var candidates;
    if (modelType === 'multiple' || dChoice === 'yes') {
      candidates = candidatesMultipleOrD1[orderIdx];
    } else {
      candidates = candidatesSingleDNot1[orderIdx];
    }

    var effectiveBytes = totalBytes * candidates;
    var effectiveTiB   = effectiveBytes / TIB_IN_BYTES;
    var cost           = effectiveTiB * PRICE_PER_TIB;

    document.getElementById('r-bytes-input').textContent  = formatBytes(totalBytes);
    document.getElementById('r-tib-input').textContent    = (totalBytes / TIB_IN_BYTES).toExponential(4) + ' TiB';
    document.getElementById('r-candidates').textContent   = candidates + ' models';
    document.getElementById('r-tib-effective').textContent = effectiveTiB.toExponential(4) + ' TiB';

    var decimals;
    if (cost < 0.01)      { decimals = 8; }
    else if (cost < 1)    { decimals = 6; }
    else                  { decimals = 4; }
    document.getElementById('r-cost').textContent = 'Estimated cost: $' + cost.toFixed(decimals);

    resultEl.style.display = 'block';
  };

  // Show/hide the d-row depending on model type selection
  function setupModelTypeToggle() {
    var modelTypeEl = document.getElementById('model-type');
    var dRow        = document.getElementById('d-row');
    if (modelTypeEl && dRow) {
      modelTypeEl.addEventListener('change', function () {
        dRow.style.display = (this.value === 'single') ? '' : 'none';
      });
    }
  }
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', setupModelTypeToggle);
  } else {
    setupModelTypeToggle();
  }
})();
</script>

---

## Pricing reference

| Parameter | Value |
|-----------|-------|
| Region | europe-west2 (London) |
| Unit price | $390.625 / TiB |
| Billing unit | Tebibyte (TiB = 2⁴⁰ bytes) |

### Candidate models by AUTO_ARIMA_MAX_ORDER

| AUTO_ARIMA_MAX_ORDER | Single TS — d = 1 | Single TS — d ≠ 1 | Multiple TS |
|:---:|:---:|:---:|:---:|
| 1 | 6 | 3 | 6 |
| 2 | 12 | 6 | 12 |
| 3 | 20 | 10 | 20 |
| 4 | 30 | 15 | 30 |
| 5 | 42 | 21 | 42 |

Source: [BigQuery ML pricing — Google Cloud](https://cloud.google.com/bigquery/pricing?hl=en#bigquery-ml-pricing)

> **Note:** This applies only to **model training**. For evaluation, inspection, and prediction queries, only the selected best model is used and regular query pricing applies.
