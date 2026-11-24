# RepoPitch skills

Turn any codebase into a client pitch. `/repopitch` audits your repo for verified
product mechanics, pricing and constraints, then builds:

- an interactive client microsite (hosted, with a live calculator and open alerts)
- a cold email sequence
- a 15-minute call script with an honest objections matrix

## Install

```bash
npx skills add kennedymadu/repopitch-skills
```

## Hosted pitches (optional)

1. Get an API key at https://repopitch-web.vercel.app (Starter Pack $9 for 5 pitches, or Pro $29/month).
2. Add it to your project's `.env` or `.env.local`:

   ```bash
   REPOPITCH_API_KEY=rp_live_...
   ```

3. Run `/repopitch` in your coding agent. The skill posts `repopitch.json` to the
   RepoPitch API and puts the returned link into the email and call script.

Without a key, the skill writes a standalone `repopitch-deck.html` instead.

Keep your key out of version control. Set `REPOPITCH_API_URL` only if you self-host the backend.

## Layout

```
skills/repopitch/
  SKILL.md
  references/package-spec.md   # repopitch.json schema and tone rules
```
