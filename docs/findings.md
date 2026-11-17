# Findings, interpretation and recommendations

## Dataset scope and analytical grain

- `SALES`: 159 records, 100 unique accounts, years 2019–2021.
- `ACCOUNTS`: 188 account reference rows. All sales accounts match the reference table.
- `FORECASTS`: 135 forecast records, all for 2022.
- Two `SUPPORT` values are missing in `SALES`. The customer-level query treats missing support components as zero when summing that component, but does not impute `TOTAL` or `PROFIT`.
- The source does not contain a unique order ID; avoid labeling a record as an order or a transaction without confirmation of the source grain.

## 1. Selected customer: Adabs Entertainment (2020)

| Category | Revenue | Profit | Profit margin |
|---|---:|---:|---:|
| Chairs | $1,835,672 | $605,772 | 33.00% |
| Electronics | $2,100,000 | $756,000 | 36.00% |

Electronics generated more revenue and profit than Chairs for this selected account and year. `SUPPORT` revenue was $500,000 for Electronics and $0 for Chairs. This **does not establish** product reliability or customer support quality; service packaging, pricing and reporting practices are alternative explanations.

## 2. Geography

The country query reports *mean revenue, units and profit per sales record*, not country totals or population-adjusted market performance. Australia and Singapore have high means in this dataset, but differences in account composition and record counts limit conclusions about market demand. Further work should compare country totals, unique accounts and time-normalized metrics before recommending geographic investment.

## 3. Commit-stage accounts

Accounts with a **combined Commit-stage forecast above $500,000** are selected, and their *historical* sales profit is aggregated by industry. This is **not forecast profit**.

| Industry | Historical profit from eligible accounts | Category |
|---|---:|---|
| Consulting | $1,846,700 | High |
| Retail | $1,381,959 | High |
| Technology | $1,309,161 | High |
| Healthcare | $1,168,570 | High |
| Hospitality | $889,699 | Normal |
| Law | $858,244 | Normal |
| Finance | $432,000 | Normal |

`High` is defined as strictly greater than $1,000,000, following the assignment threshold. More context about the number of accounts and opportunity conversion rates would be needed to make investment decisions.

## 4. Actual profit (2021) vs pipeline forecast (2022)

| Category | 2021 actual profit | 2022 pipeline forecast | Forecast minus actual, as % of actual | Opportunity age min–max |
|---|---:|---:|---:|---:|
| Break room | $22,115,484 | $33,107,609 | +49.70% | 2–378 |
| Chairs | $2,214,099 | $4,343,857 | +96.19% | 3–173 |
| Desks | $4,800,070 | $8,681,586 | +80.86% | 3–383 |
| Electronics | $6,037,529 | $9,929,645 | +64.47% | 3–383 |

These figures are **not year-over-year actual growth**: 2022 values are pipeline forecasts, and the two measures may not be directly comparable. The `OPPORTUNITY_AGE` units are not documented in the supplied CSV, so no unit (such as days) is assumed.

## 5. Independent profitability study

| Metric | Result |
|---|---|
| Total sales records | 159 |
| Unique sales accounts | 100 |
| Historical annual revenue, 2019 | $55,016,808 |
| Historical annual revenue, 2020 | $73,141,247 |
| Historical annual revenue, 2021 | $100,840,810 |
| Highest revenue industry | Retail ($36,059,921) |
| Highest profit margin industry | Healthcare (38.03%) |
| Lowest profit margin industry | Banking (30.50%) |
| Highest mean profit per sales record | Automotive ($769,853) |

The dataset supports the descriptive observation that high-revenue industries do not necessarily have the highest margins. It **does not** establish the causes of the differences (competition, pricing policy, cost structure or product mix).

## Business recommendations (hypotheses to validate)

1. **Prioritize further investigation**, not automatic investment, into high-margin industries such as Healthcare; compare account counts, acquisition costs and capacity.
2. **Review margin drivers** in Banking and Insurance by product and customer mix before changing prices.
3. **Audit pipeline assumptions** in Chairs, where the 2022 forecast is almost twice 2021 realized profit; examine conversion rates and forecast calibration.
4. **Investigate the commercial value of support revenue** by product category, with service cost and ticket-volume data before making claims about profitability or quality.
5. **Improve the data model** with a unique order identifier, a reliable transaction date, cost breakdown and actual 2022 results to evaluate forecast accuracy.

## Verification

Aggregate figures were recalculated from the uploaded CSVs with Python/Pandas. The corrected Snowflake SQL has been reviewed but not executed on a Snowflake warehouse in this environment. Run the queries against an authorized copy of the source tables to verify SQL-engine-specific behavior.
