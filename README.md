# GA4 Customer Audiences

**From clicks to campaigns:** turning raw website behaviour into customer audiences a marketing team can launch, and **validating** them before they go live.

Built to practise customer-data work for airline and e-commerce marketing: SQL on a large real dataset, segmentation, machine learning, and checking AI-generated code against the data.

**Data:** Google Analytics 4 events from the Google Merchandise Store (Google's public, obfuscated GA4 sample in BigQuery), Nov 2020 to Jan 2021, about 4 million events. Real user behaviour from a real online shop.

**Tools:** BigQuery (SQL, window functions) · Python (pandas, scikit-learn) · Looker Studio · Google Tag Manager + GA4

---

## Objective

Turn three months of real website behaviour into **validated customer audiences** that a marketing team could use in a campaign, and show how to check both the data and the AI tools used along the way.

## Goals

| # | Goal | Status |
|---|---|---|
| 1 | Build user attributes from raw events with SQL **window functions** in BigQuery | in progress |
| 2 | Define **5–6 campaign audiences**, each with a campaign idea | to do |
| 3 | **Validate** every audience: sizes, overlaps, revenue reconciliation, data-quality issues | to do |
| 4 | **Clustering** and a **purchase-propensity model** in Python | to do |
| 5 | Use an AI assistant for part of the SQL and **document where it was wrong** and how validation caught it | to do |
| 6 | **Dashboard** of audiences and value in Looker Studio | to do |
| 7 | **Event tracking with consent** (GA4 + Google Tag Manager) on my live app [Blitz Fathersday](https://github.com/sabrinapg/blitz-fathersday) | to do |

## 3-day sprint

Built in a 3-day sprint, from first query to finished project.

| Day | Date (2026) | Delivered |
|---|---|---|
| **Day 1** | Thu 19 Nov | Goals 1–3: SQL pipeline in BigQuery, audiences, validation. Repo public. |
| **Day 2** | Fri 20 Nov | Goals 4–6: clustering and propensity model in Python, AI-check log, Looker Studio dashboard |
| **Day 3** | Sat 21 Nov | Goal 7: consent-based GA4 + GTM tracking on Blitz Fathersday. One-page campaign brief, 2-minute demo video, final README. |

## Build log

*What got done each day, and what I learned. Updated as I go.*

- **Day 1**:
- **Day 2**:
- **Day 3**:

---

## The question

> *Which groups of visitors should a marketing team target, with what message, and can we trust those groups?*

## How it works

```
raw GA4 events  ->  sessions  ->  user attributes  ->  audiences  ->  validation
   01_events        02_sessions     03_users           04_audiences    05_validation
```

| Step | What it does | SQL techniques |
|---|---|---|
| `01_events.sql` | Flattens the raw nested GA4 export into one clean table | `UNNEST` on nested arrays, wildcard tables |
| `02_sessions.sql` | One row per session, with each user's journey over time | `ROW_NUMBER`, `LAG`, running `SUM` window functions |
| `03_users.sql` | One row per user: recency, frequency, value, funnel steps | aggregations, `NTILE` value quartiles |
| `04_audiences.sql` | Assigns every user to exactly one audience | `CASE` rules, ordered by priority |
| `05_validation.sql` | Checks the audiences before they're used | sizes and shares, uniqueness, revenue reconciliation, data-quality checks |

## Audiences

| Audience | Rule | Campaign idea | Size |
|---|---|---|---|
| Repeat buyer | 2+ purchases | Loyalty, early access (top value quartile) | *fill in* |
| Lapsed one-time buyer | 1 purchase, more than 30 days ago | Win-back offer | *fill in* |
| Recent one-time buyer | 1 purchase in the last 30 days | Second-purchase nudge | *fill in* |
| High-intent non-buyer | Added to cart, visited in the last 30 days, never bought | Cart reminder | *fill in* |
| Browser | 3+ product views, no cart | Product recommendations | *fill in* |
| Low engagement | Everyone else | No targeting (saves budget) | *fill in* |

## Validation results

*Fill in from `05_validation.sql`:*
- Every user is in exactly one audience: ✅ / ❌
- No buyers in non-buyer audiences: ✅ / ❌
- Revenue reconciles between users and raw events: ✅ / ❌ (difference: ...)
- **Data quality findings:** *e.g. X purchases have no revenue, Y events have no session id. What I did about it, and why it matters for a campaign.*

## What I'd do next (v2)

- **Clustering (k-means)** on the user attributes, to see whether data-driven segments match the rule-based audiences
- **A propensity model**: the probability a non-buyer will purchase, to rank the high-intent audience
- **Checking AI-written SQL:** where an AI assistant got the queries wrong, and how the validation caught it
- **Looker Studio dashboard** of audience sizes and value

## Run it yourself

1. Open the [BigQuery sandbox](https://console.cloud.google.com/bigquery) (free, no credit card).
2. Create a dataset named `ga4_audiences`.
3. Run the files in order: `01` to `05`.
