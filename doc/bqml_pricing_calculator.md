# BigQuery ML Pricing Calculator

This calculator estimates the cost of **BigQuery ML ARIMA_PLUS / ARIMA_PLUS_XREG** time-series model training queries.

> **How it works:** When `AUTO_ARIMA` is enabled, BigQuery ML trains multiple candidate models in parallel. The bytes billed by your input `SELECT` statement are multiplied by the number of candidate models, which depends on `AUTO_ARIMA_MAX_ORDER` and the type of forecasting. The ARIMA_PLUS pricing tier costs **50× the standard BQ ML rate**.

---

<div id="bqml-calculator">

<style>
#bqml-calculator {
  font-family: inherit;
  max-width: 660px;
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
#bqml-calculator .section-divider {
  border: none;
  border-top: 1px solid var(--md-default-fg-color--lightest, #e0e0e0);
  margin: 1.4rem 0 0.2rem;
}
#bqml-calculator .section-heading {
  font-size: 0.78rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--md-default-fg-color--light, #888);
  margin: 0.9rem 0 0;
}
#bqml-calculator .unit-label {
  font-size: 0.88rem;
  color: var(--md-default-fg-color--light, #666);
  white-space: nowrap;
}
#bqml-calculator .derived-price {
  margin-top: 0.5rem;
  padding: 0.55rem 0.9rem;
  background: var(--md-code-bg-color, #f5f5f5);
  border-radius: 5px;
  border-left: 3px solid #e65100;
  font-size: 0.88rem;
  color: var(--md-default-fg-color, #333);
}
#bqml-calculator .override-check-row {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-top: 0.9rem;
}
#bqml-calculator .override-check-row input[type="checkbox"] {
  width: 1rem;
  height: 1rem;
  cursor: pointer;
  flex-shrink: 0;
}
#bqml-calculator .override-check-row label {
  margin: 0;
  font-weight: 500;
  font-size: 0.88rem;
  cursor: pointer;
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
#bqml-calculator .calc-steps {
  margin-top: 0.9rem;
  padding: 0.7rem 0.9rem;
  background: rgba(0,0,0,0.03);
  border-radius: 4px;
  font-size: 0.77rem;
  color: var(--md-default-fg-color--light, #555);
  line-height: 1.7;
}
#bqml-calculator .calc-steps strong {
  color: var(--md-default-fg-color, #333);
  display: block;
  margin-bottom: 0.3rem;
}
#bqml-calculator #calc-error {
  display: none;
  margin-top: 1rem;
  color: #c62828;
  font-size: 0.9rem;
}
</style>

<div class="calc-card">

  <p class="section-heading">Input data</p>

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

  <div class="override-check-row">
    <input type="checkbox" id="override-check" />
    <label for="override-check">Override candidate models multiplier manually</label>
  </div>
  <div id="override-input-row" style="display:none; margin-top:0.5rem;">
    <input type="number" id="override-value" min="1" step="1" placeholder="e.g. 29" style="max-width:120px; flex:none;" />
  </div>
  <p class="hint" style="margin-top:0.35rem;">Enable to set a custom multiplier (e.g. if BigQuery pruned the model space during the job).</p>

  <hr class="section-divider" />
  <p class="section-heading">Pricing</p>

  <label for="std-price">Standard BQ unit price per TiB (USD)</label>
  <div class="input-row">
    <input type="number" id="std-price" min="0" step="any" value="7.8125" style="max-width:150px; flex:none;" />
    <span class="unit-label">USD / TiB</span>
  </div>
  <p class="hint">Default is $7.8125 / TiB for europe-west2 (standard on-demand BQ query price). Change for other regions or if the price has been updated. The ARIMA_PLUS tier factor (×50) is applied to the processed <em>volume</em>, not the unit price.</p>

  <hr class="section-divider" />
  <p class="section-heading">Currency conversion <span style="font-weight:400; text-transform:none; font-size:1.1em;">(optional)</span></p>

  <label for="conv-rate">Conversion rate</label>
  <div class="input-row">
    <input type="number" id="conv-rate" min="0" step="any" placeholder="e.g. 0.79 for USD → GBP" />
  </div>
  <p class="hint">Multiplies the USD total by this rate to convert to another currency. Leave empty or 0 to skip.</p>

  <hr class="section-divider" />
  <p class="section-heading">Discount <span style="font-weight:400; text-transform:none; font-size:1.1em;">(optional)</span></p>

  <label for="discount-pct">Percentage discount</label>
  <div class="input-row">
    <input type="number" id="discount-pct" min="0" max="100" step="any" placeholder="e.g. 10" style="max-width:150px; flex:none;" />
    <span class="unit-label">%</span>
  </div>
  <p class="hint">Subtracts this percentage from the total cost. Leave empty or 0 to skip.</p>

  <button onclick="calculateCost()">Calculate cost</button>

  <p id="calc-error">⚠ Please enter a valid positive number for bytes billed.</p>

  <div id="calc-result">
    <h4>Estimated Training Cost</h4>
    <div class="result-row">
      <span class="result-label">Bytes billed (input)</span>
      <span class="result-value" id="r-bytes-input"></span>
    </div>
    <div class="result-row">
      <span class="result-label">Raw scan (TiB)</span>
      <span class="result-value" id="r-tib-input"></span>
    </div>
    <div class="result-row">
      <span class="result-label">ML tier factor (ARIMA_PLUS)</span>
      <span class="result-value">× 50</span>
    </div>
    <div class="result-row">
      <span class="result-label">Candidate models (multiplier)</span>
      <span class="result-value" id="r-candidates"></span>
    </div>
    <div class="result-row">
      <span class="result-label">Total Processed (TiB)</span>
      <span class="result-value" id="r-total-processed"></span>
    </div>
    <div class="result-row">
      <span class="result-label">Standard BQ unit price</span>
      <span class="result-value" id="r-std-price"></span>
    </div>
    <div class="result-row">
      <span class="result-label">Cost (USD)</span>
      <span class="result-value" id="r-cost-usd"></span>
    </div>
    <div id="r-conv-row" class="result-row" style="display:none;">
      <span class="result-label" id="r-conv-label">Cost (converted)</span>
      <span class="result-value" id="r-cost-conv"></span>
    </div>
    <div id="r-discount-row" class="result-row" style="display:none;">
      <span class="result-label" id="r-discount-label">Discount</span>
      <span class="result-value" id="r-discount-val"></span>
    </div>
    <div class="result-total" id="r-final-cost"></div>
    <div class="calc-steps" id="r-steps"></div>
  </div>

</div>

</div>

<script>
(function () {
  // Candidate model counts from GCP docs
  // Index 0 = AUTO_ARIMA_MAX_ORDER 1, index 4 = order 5
  var candidatesMultipleOrD1 = [6, 12, 20, 30, 42];  // multiple TS, or single TS with d = 1
  var candidatesSingleDNot1  = [3, 6,  10, 15, 21];  // single TS with d ≠ 1

  var ML_TIER_FACTOR = 50;
  var TIB_IN_BYTES   = 1099511627776;  // 2^40

  function formatBytes(bytes) {
    if (bytes < 1024)         return bytes.toFixed(2) + ' B';
    if (bytes < 1048576)      return (bytes / 1024).toFixed(2) + ' KiB';
    if (bytes < 1073741824)   return (bytes / 1048576).toFixed(2) + ' MiB';
    if (bytes < TIB_IN_BYTES) return (bytes / 1073741824).toFixed(4) + ' GiB';
    return (bytes / TIB_IN_BYTES).toFixed(6) + ' TiB';
  }

  function fmtMoney(val) {
    var d;
    if      (val < 0.0001) d = 10;
    else if (val < 0.01)   d = 8;
    else if (val < 1)      d = 6;
    else                   d = 4;
    return val.toFixed(d);
  }

  window.calculateCost = function () {
    var errorEl  = document.getElementById('calc-error');
    var resultEl = document.getElementById('calc-result');
    errorEl.style.display  = 'none';
    resultEl.style.display = 'none';

    var rawValue  = parseFloat(document.getElementById('bytes-input').value);
    var unitMult  = parseFloat(document.getElementById('unit-select').value);
    var modelType = document.getElementById('model-type').value;
    var dChoice   = document.getElementById('d-select').value;
    var orderIdx  = parseInt(document.getElementById('arima-order').value, 10) - 1;

    if (isNaN(rawValue) || rawValue <= 0) {
      errorEl.style.display = 'block';
      return;
    }

    var totalBytes = rawValue * unitMult;
    var inputTiB   = totalBytes / TIB_IN_BYTES;

    // ── Candidate models ──────────────────────────────────────────────────────
    var computedCandidates = (modelType === 'multiple' || dChoice === 'yes')
      ? candidatesMultipleOrD1[orderIdx]
      : candidatesSingleDNot1[orderIdx];

    var overrideCheck   = document.getElementById('override-check').checked;
    var overrideRaw     = parseInt(document.getElementById('override-value').value, 10);
    var overrideApplied = overrideCheck && !isNaN(overrideRaw) && overrideRaw > 0;
    var finalCandidates = overrideApplied ? overrideRaw : computedCandidates;

    // ── Pricing ───────────────────────────────────────────────────────────────
    var stdRaw            = parseFloat(document.getElementById('std-price').value);
    var stdPrice          = (isNaN(stdRaw) || stdRaw <= 0) ? 7.8125 : stdRaw;
    var totalProcessedTiB = inputTiB * ML_TIER_FACTOR * finalCandidates;
    var costUSD           = totalProcessedTiB * stdPrice;

    // ── Currency conversion ───────────────────────────────────────────────────
    var convRaw  = document.getElementById('conv-rate').value.trim();
    var convRate = parseFloat(convRaw);
    var hasConv  = convRaw !== '' && !isNaN(convRate) && convRate > 0;
    var costConv = hasConv ? costUSD * convRate : null;

    // ── Discount ──────────────────────────────────────────────────────────────
    var discRaw          = document.getElementById('discount-pct').value.trim();
    var discPct          = parseFloat(discRaw);
    var hasDisc          = discRaw !== '' && !isNaN(discPct) && discPct > 0;
    var baseBeforeDisc   = hasConv ? costConv : costUSD;
    var discountAmount   = hasDisc ? baseBeforeDisc * (discPct / 100) : 0;
    var finalCost        = hasDisc ? baseBeforeDisc - discountAmount : baseBeforeDisc;

    // ── Populate main result rows ─────────────────────────────────────────────
    document.getElementById('r-bytes-input').textContent      = formatBytes(totalBytes);
    document.getElementById('r-tib-input').textContent        = inputTiB.toExponential(4) + ' TiB';
    document.getElementById('r-candidates').textContent       = '× ' + finalCandidates + ' models'
      + (overrideApplied ? ' (manually overridden; computed was ' + computedCandidates + ')' : '');
    document.getElementById('r-total-processed').textContent  = totalProcessedTiB.toExponential(4) + ' TiB';
    document.getElementById('r-std-price').textContent        = '$' + stdPrice + ' / TiB';
    document.getElementById('r-cost-usd').textContent         = '$' + fmtMoney(costUSD);

    var convRow = document.getElementById('r-conv-row');
    if (hasConv) {
      document.getElementById('r-conv-label').textContent = 'Cost (× ' + convRate + ' conversion rate)';
      document.getElementById('r-cost-conv').textContent  = fmtMoney(costConv);
      convRow.style.display = '';
    } else {
      convRow.style.display = 'none';
    }

    var discRow = document.getElementById('r-discount-row');
    if (hasDisc) {
      document.getElementById('r-discount-label').textContent = 'Discount (' + discPct + '%)';
      document.getElementById('r-discount-val').textContent   = '− ' + fmtMoney(discountAmount);
      discRow.style.display = '';
    } else {
      discRow.style.display = 'none';
    }

    var currSym = hasConv ? '' : '$';
    document.getElementById('r-final-cost').textContent = 'Estimated cost: ' + currSym + fmtMoney(finalCost);

    // ── Calculation steps note ────────────────────────────────────────────────
    var n = 1;
    var steps = [];
    steps.push(n++ + '. raw scan: ' + formatBytes(totalBytes)
      + ' = ' + inputTiB.toExponential(4) + ' TiB');
    steps.push(n++ + '. ml tier factor (ARIMA_PLUS): × ' + ML_TIER_FACTOR);
    steps.push(n++ + '. candidate models (multiplier): × ' + finalCandidates
      + (overrideApplied
          ? ' (manually overridden; computed value from AUTO_ARIMA_MAX_ORDER = '
            + (orderIdx + 1) + ' was ' + computedCandidates + ')'
          : ' (from AUTO_ARIMA_MAX_ORDER = ' + (orderIdx + 1) + ')'));
    steps.push(n++ + '. total processed: '
      + inputTiB.toExponential(4) + ' TiB × ' + ML_TIER_FACTOR + ' × ' + finalCandidates
      + ' = ' + totalProcessedTiB.toExponential(4) + ' TiB');
    steps.push(n++ + '. standard BQ unit price: $' + stdPrice + ' / TiB');
    steps.push(n++ + '. cost (USD): '
      + totalProcessedTiB.toExponential(4) + ' TiB × $' + stdPrice
      + ' = $' + fmtMoney(costUSD));
    if (hasConv) {
      steps.push(n++ + '. currency conversion: $' + fmtMoney(costUSD)
        + ' × ' + convRate + ' = ' + fmtMoney(costConv));
    }
    if (hasDisc) {
      steps.push(n++ + '. discount (' + discPct + '%): '
        + fmtMoney(baseBeforeDisc) + ' × ' + discPct + '% = ' + fmtMoney(discountAmount)
        + '  →  final cost: ' + fmtMoney(baseBeforeDisc)
        + ' − ' + fmtMoney(discountAmount) + ' = ' + fmtMoney(finalCost));
    }
    document.getElementById('r-steps').innerHTML =
      '<strong>calculation steps:</strong>' + steps.join('<br>');

    resultEl.style.display = 'block';
  };

  function setupListeners() {
    // Model type toggle (show/hide d-row)
    var modelTypeEl = document.getElementById('model-type');
    var dRow        = document.getElementById('d-row');
    if (modelTypeEl && dRow) {
      modelTypeEl.addEventListener('change', function () {
        dRow.style.display = (this.value === 'single') ? '' : 'none';
      });
    }
    // Override checkbox toggle
    var overrideCheck    = document.getElementById('override-check');
    var overrideInputRow = document.getElementById('override-input-row');
    if (overrideCheck && overrideInputRow) {
      overrideCheck.addEventListener('change', function () {
        overrideInputRow.style.display = this.checked ? '' : 'none';
      });
    }
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', setupListeners);
  } else {
    setupListeners();
  }
})();
</script>

---

## Pricing reference

| Parameter | Value |
|-----------|-------|
| Region (default) | europe-west2 (London) |
| Standard BQ unit price | $7.8125 / TiB |
| ARIMA_PLUS tier factor | × 50 (applied to volume) |
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
