# UK Small Business First-Pass Acquisition Model

This document adapts the earlier decision-model blueprint for UK small businesses that typically file abridged or filleted accounts with Companies House. It focuses on publicly available balance sheet data and adds UK-specific checks, terminology, and follow-up requests.

## 1. Inputs from UK abridged/filleted accounts
- **Balance sheet items**: Cash, trade debtors, inventories, prepayments, trade creditors, accruals/other creditors, bank loans/overdrafts, HP/lease liabilities, directors' loans, tangible fixed assets (at cost and NBV), intangible assets, total assets, total liabilities, net current assets/liabilities, capital and reserves.
- **Notes (if filed)**: Maturity analysis of debt (due within/after one year), security/charges, related-party balances (director/shareholder loans), contingent liabilities, average employee numbers.
- **Contextual metadata**: Filing date/period length, sector tag (services/light manufacturing/e-commerce/contracting), legal form (Ltd/LLP), any audit qualification.

## 2. Core UK-specific metrics and hard stops
- **Liquidity**
  - Current ratio = Current assets ÷ Current liabilities (target ≥1.2; hard-stop <0.8).
  - Quick ratio = (Cash + Trade debtors) ÷ Current liabilities (target ≥0.9; hard-stop <0.6 for services, <0.75 for product/inventory-heavy).
  - Net current assets trend: If current liabilities exceed current assets, flag as negative working capital.
- **Leverage & solvency**
  - Gearing = (Bank loans + HP/leases + overdraft + director loans (credit)) ÷ Shareholders' funds. Hard-stop if >1.5x unless strong cash/recurring revenue; caution >1.0x.
  - Interest cover proxy = EBITDA proxy ÷ (Interest paid/finance costs if disclosed). Hard-stop if <2x (or interest not disclosed but high leverage present).
  - Negative equity: Hard-stop unless director loans are subordinated and business shows strong cash.
- **Cash conversion/working capital quality**
  - Debtor intensity = Trade debtors ÷ Revenue proxy. If no revenue, approximate using debtors vs. creditors: flag if debtors >> creditors and cash low (slow collections risk).
  - Inventory dependence = Inventory ÷ Current assets; flag >40% for services, >60% for retail/e-commerce without matching cash.
  - Creditor stretch = Trade creditors ÷ (Trade debtors + cash). Flag if >1.2x unless the sector is supplier-financed (construction subcontractors, retail on terms).
- **Asset quality**
  - Tangible asset concentration = Tangible fixed assets (NBV) ÷ Total assets. For asset-light sectors, flag >40% unless revenue model supports it.
  - Capitalised development/brands (intangible assets): Flag if large vs. equity or if amortisation not visible.
- **Compliance and disclosure**
  - Audit qualification, emphasis of matter, or overdue filings: Hard-stop or high-risk flag.
  - Charges registered at Companies House not matched to balance sheet debt: flag for follow-up.

## 3. Scoring bands (example defaults)
- **Liquidity (35%)**: Current ratio (15%), quick ratio (15%), net current assets sign (5%).
- **Leverage (25%)**: Gearing (15%), interest cover proxy (10%).
- **Asset/working capital quality (25%)**: Debtor intensity (10%), inventory dependence (10%), creditor stretch (5%).
- **Governance/filing quality (15%)**: Timeliness of filings (5%), audit status (5%), charge-to-debt consistency (5%).
- **Hard stops override scores**: negative equity without clear subordination, audit qualification, current ratio <0.8, gearing >1.5x without strong liquidity, overdue filings >3 months.

## 4. Decision outcomes
- **Go**: Score ≥70 with no hard-stop, and at most one medium flag.
- **Conditional**: Score 50–69 or any medium flags; proceed only with satisfactory answers to follow-up requests.
- **No-Go**: Score <50 or any hard-stop triggered.

## 5. Information-request list (UK-focused)
When outcome is Go/Conditional, request concise items tailored to UK filings:
- **Financials**: Full P&L for last 2 years, current YTD management accounts, cash flow statement if available, revenue by segment/top 10 customers.
- **Working capital detail**: AR/AP aging (30/60/90), inventory breakdown and turnover, seasonality, payment terms with key suppliers/customers.
- **Debt & charges**: Debt amortisation schedule (bank/HP/leases/overdraft), interest rates and covenants, copies of debentures/charges, confirmation of any subordinated director loans.
- **Contracts & exposure**: Largest customer/supplier concentrations, churn or renewal profile (if recurring), order book/backlog.
- **Compliance**: Details of any audit qualifications, contingent liabilities, HMRC time-to-pay agreements, outstanding litigation.
- **People/ops**: Headcount by function (matches statutory “average employees” note), key-man dependencies, lease terms for premises.

## 6. Second-pass metrics using requested data
- Gross margin, EBITDA margin, and YoY growth.
- Cash conversion = (EBITDA – CapEx – ∆working capital) ÷ EBITDA.
- Customer concentration: % of revenue from top 1/3/5 customers; hard-stop if >50% from top 1 without contract durability.
- Recurring revenue share and churn/renewal metrics for subscription-like models.
- Free cash flow leverage = Net debt ÷ EBITDA; hard-stop if >3.0x unless highly recurring revenue.

## 7. Sector-specific tuning (UK examples)
- **Services/consulting/contracting**: Lower tolerance for negative working capital; quick ratio cut-off 0.6–0.7; watch for PSC/IR35 risk in contractor-heavy models.
- **Light manufacturing**: Higher inventory tolerance; emphasise CapEx and HP/lease leverage; check energy cost exposure.
- **E-commerce/retail**: Focus on inventory turnover, returns rate, marketplace dependence (Amazon/eBay), and payment-provider reserves/holdbacks.
- **Facilities/maintenance contracts**: Look for recurring revenue, contract lengths, SLA penalties, and mobilisation costs.

## 8. Implementation notes (Sheet or web app)
- Store metric weights, thresholds, and sector overrides in a JSON/YAML config for easy tuning.
- Include UK-specific validation hints: currency in GBP, period length (12 months unless stated), Companies House filing date, and whether small-company exemptions were used.
- Add a "data sufficiency" indicator when revenue or P&L is missing—show reliance on proxies.
- Log decision rationale with config version to maintain auditability if thresholds change.

## 9. Testing scenarios (UK-focused)
- Negative equity with large director loan (credit balance) and high creditors: expect No-Go unless subordinated.
- High quick ratio driven by large debtors but low cash and stretched creditors: expect Conditional with AR aging request.
- Inventory-heavy e-commerce with thin cash: expect Conditional pending turnover/returns data.
- Service business with clean balance sheet, low debt, and on-time filings: expect Go.
- Overdue filings by >3 months or audit qualification: expect No-Go regardless of score.
