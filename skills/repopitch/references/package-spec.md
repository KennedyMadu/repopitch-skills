# RepoPitch Package Formatting & Tone Rules

## Tone & Copywriting Principles
- **Plain, direct language:** Zero corporate buzzwords or developer jargon. Translate technical architecture into client outcomes (e.g., "Automated compliance notices" instead of "cron-triggered database mutations").
- **Radical honesty:** On tough questions (new product, data storage location, cancellation, manual vs. automated steps), state the exact truth clearly. Candid answers build more trust with sophisticated buyers than vague claims.

## `repopitch.json` Schema
```json
{
  "meta": {
    "project_name": "string",
    "audience_type": "client | portfolio_investor | b2b_buyer | vc_investor",
    "recipient_label": "string",
    "generated_at": "YYYY-MM-DD",
    "brand_name": "optional, Pro plan only",
    "brand_color": "optional #rrggbb, Pro plan only"
  },
  "executive_summary": ["Optional: 3 short key points"],
  "comparison": [
    { "problem": { "title": "Today", "detail": "Verified friction" },
      "solution": { "title": "With us", "detail": "Verified outcome" } }
  ],
  "guarantees": ["Optional: explicit, verifiable commitments"],
  "hook": {
    "headline": "Outcome-focused headline tailored to the recipient",
    "subheadline": "1-sentence summary of the core value proposition"
  },
  "problem_bullets": [
    { "title": "Friction point", "detail": "Concrete time, compliance, or fee cost" }
  ],
  "solution_steps": [
    { "step": 1, "title": "Workflow step", "outcome": "Verified client benefit", "timeframe": "optional, e.g. Week 1" }
  ],
  "calculator": {
    "enabled": true,
    "label": "Fee & Time Savings Calculator",
    "inputs": { "volume": 5, "unit_value": 600 },
    "baseline_comparison": "Traditional provider cost/friction",
    "our_model": "Verified pricing/fee formula from codebase",
    "baseline_rate_pct": 8,
    "our_rate_pct": 3,
    "our_flat_fee": 0
  },
  "verified_facts_and_trust": [
    "Verified compliance, security, or operational safeguards"
  ],
  "cta": {
    "button_text": "Book 15-Minute Walkthrough",
    "url": "Verified URL from codebase"
  }
}

```

## Objections Matrix Minimum Coverage
Every generated call script must include verified, honest `They say` / `You say` rows covering:
1. Switching friction ("We already have a system / agent")
2. Early-stage credibility ("Who else uses this?")
3. Lock-in & exit ("What if we leave or you shut down?")
4. Data hosting, privacy & fund handling
5. Pricing & hidden fees
6. "Just send me some info" (pivoting directly to the custom RepoPitch interactive deck link)

## Deck Rendering Rules
1. **Calculator defaults match the pitch:** the calculator's default inputs (property count, rent, sale price) must equal `calculator.inputs` in `repopitch.json` and any number in the headline, so the deck shows the headline saving immediately on load. Run the formula in Node and confirm the default total before shipping.
2. **One heading layer only:** each section heading is a single plain `<h2>`. No duplicate, ghosted or shadow heading text behind or above it (no `::before`/`::after` copies, `text-shadow`, or absolutely positioned repeats). Grep the deck for these before shipping.
