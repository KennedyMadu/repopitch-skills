---
name: repopitch
description: "Audits a codebase for verified product mechanics, pricing, and technical constraints, then generates a complete commercial deal package: an interactive client/investor deck, signal-based outbound emails, a 15-minute walkthrough script, and an honest objections matrix."
---

# RepoPitch — Full-Stack Commercial & Proposal Engine

When the user invokes `/repopitch` or asks to create a pitch, client proposal, sales campaign, or walkthrough kit from their codebase, execute these four phases in order:

## Phase 1: Codebase Verification Audit (Zero Hallucinations)
Before drafting any sales asset, inspect the repository to establish verified ground truth:
- **Core Capabilities & Workflows:** Read `README.md`, primary routes, onboarding flows, and automated features.
- **Pricing, Fees & Calculators:** Read billing/Stripe configs, fee constants, pricing pages, and calculation logic.
- **Infrastructure & Constraints:** Check database/hosting regions, auth/cancellation behavior, privacy terms, and data export endpoints.
- **Proof Rule:** Never invent customer metrics, pilot counts, or testimonials. If no live proof exists in the repo or user prompt, lead strictly with verified product mechanics and transparent founder terms.

## Client-Facing Language Rule (strict)
Everything the recipient reads or hears is client-facing: `repopitch.json`, the deck, the outbound emails, and the words a founder says on a call. In that text, NEVER include:
- **File paths or code names:** no `src/...` paths, function or constant names (e.g. `pmQuote`, `baseFeeCents`), table names, env vars or route names.
- **Cloud or infrastructure vendor names:** no Supabase, Vercel, Stripe, Resend, Anthropic, AWS, etc. Say what matters to the buyer instead ("secure servers in Singapore", "your card payments are handled by a payment provider").
Use plain, persuasive language about outcomes and fees. Phase 1 findings stay internal: use them to check facts, then translate them. Honesty still applies: if the truth is awkward (for example, data is stored outside Australia), state it plainly without the vendor name, and offer the provider list on request. Internal notes (pre-send checklists, "gaps to fix") may name files and vendors, but must be clearly separate from text the client will see.

## Hosted Calculator Rule
The hosted calculator multiplies ALL `calculator.inputs` together to get its base amount, then applies the rates. Inputs must multiply to the amount the rates apply to (for example properties × weekly rent × 52 weeks = annual rent). Never add an input, such as a sale price, that is not part of that product. Input keys become on-screen labels (`rental_properties` shows as "Rental properties"), so name them in plain words.

## Buyer-Adapted Pitch Sections
Every pitch is written for one buyer type (landlord, investor, agency principal, enterprise, VC) and uses ALL the sections the hosted renderer supports, each rewritten for that buyer. Their words, their money, their risks. A generic pitch with the same text for everyone is a failure.
- `meta`: `brand_name` and `brand_color` (`#rrggbb`) set to the seller's brand; `recipient_label` describes the buyer in one line.
- `hook`: the buyer's outcome in one headline, then one plain sentence on how.
- `executive_summary` (up to 6 strings): what they get, what it costs, what is not done yet. Short enough to read in 30 seconds.
- `comparison` (up to 6 `{problem:{title,detail}, solution:{title,detail}}` pairs): the buyer's current pain next to the concrete fix, pair by pair. Use this in place of, or alongside, `problem_bullets`.
- `solution_steps`: each step has a `timeframe` ("15 minutes", "Day one", "Agreed with you"). Only state a timeframe the product or process really delivers. When it is not fixed, say so ("Agreed with you").
- `guarantees` (up to 12 strings): commitments that are true in the product or contract today (fee terms, exit terms, who holds money, what the software will never do without approval). No guarantee that depends on a result.
- `calculator`: include only when a real financial comparison fits the buyer (fees, price, savings) and every number is verified. Otherwise omit it. Never invent a baseline to make a calculator work.
- `verified_facts_and_trust`: the awkward truths stated plainly (new product, data location, what is not built or not live yet).
- Before pitching a feature, confirm in the code that THIS buyer type can use it. A feature that exists only for the seller's own internal use, or only in test mode, is not offered to a customer. Put it in the pre-send summary for the founder instead.
- All of the above stays in plain client-facing language, with zero file paths, function names or cloud vendor names (see the Client-Facing Language Rule).
- Never overwrite an existing `repopitch.json`: check `git status`/the file first, and save a pitch for a different buyer as `repopitch-<buyer>.json`.

## Phase 2: Target & Deal Scoping
If not already provided by the user, ask briefly for:
1. **Target Persona:** Who is receiving this? (e.g., landlord, property seller, multi-property portfolio investor, B2B agency principal, enterprise buyer, or investor).
2. **Deal Context & Numbers:** Specific volume/value metrics (e.g., number of properties/doors, rent/sale value, or current software pain) and the primary call-to-action (e.g., reply, 15-minute walkthrough, or pilot signup).

## Phase 3: Generate the 4-Part Commercial Package
Read `references/package-spec.md` and generate:
1. **`repopitch.json` + Presentation (`repopitch-deck.html` or Hosted URL):**
   - **Render Pitch step:** if `REPOPITCH_API_KEY` is present in `.env` or `.env.local`, load it (never print it) and run:
     ```bash
     curl -sS -X POST "${REPOPITCH_API_URL:-https://repopitch.vercel.app}/api/render" \
       -H "Authorization: Bearer $REPOPITCH_API_KEY" \
       -H "Content-Type: application/json" \
       --data @repopitch.json
     ```
     Parse the `url` field from the JSON response and use it as `{{deck_url}}` everywhere it appears in the outbound email and call script. On a non-2xx response (401 invalid key, 402 no credits, 400 invalid payload), report the `error` message and fall back to the standalone `repopitch-deck.html`; never leave `{{deck_url}}` unfilled.
   - The hosted calculator only shows the full savings comparison when `calculator.baseline_rate_pct` and `calculator.our_rate_pct` are set (plus `our_flat_fee` if there is a flat fee, default 0). Fill them from verified pricing in Phase 1; if pricing is not verified, omit all three rather than guessing.
   - Otherwise, render a standalone, responsive `repopitch-deck.html` with print-to-PDF support and a working JavaScript ROI/fee savings calculator tailored to the prospect's numbers.
2. **Outbound Campaign (`founder/repopitch-[target]-outbound.md`):**
   - Variables table, 4-row Signal Bank (*Signal → Observation → Bridge*), 3 lowercase subject lines, ~150-word plain-text Email 1 linking to the RepoPitch deck, a 4-touch follow-up schedule, and local anti-spam compliance placeholders (sender identity, physical address, plain-text opt-out).
3. **15-Minute Walkthrough & Objections Playbook (`founder/repopitch-[target]-call-script.md`):**
   - 3-minute pre-call prep checklist, 30-second agenda setter, timed 15-minute call structure (*0:00–2:00 Open, 2:00–5:00 Discovery, 5:00–12:00 Tailored Demo, 12:00–14:00 Offer, 14:00–15:00 Close*), and an honest **Objections Matrix** (`They say` / `You say`) grounded in Phase 1 technical truths.

## Phase 4: Pre-Send Action Summary
Conclude with a concise bulleted list of:
- Unfilled placeholders (sender address, sending domain, live spot count).
- Codebase or configuration gaps discovered during Phase 1 that need attention before promising them to a client.
