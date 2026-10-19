# Setup notes

Built 18 October 2026. No keys or passwords in this file — those live in
`.env.local` (on this computer only) and in Supabase's secret storage.

## Where things are

- **App:** https://open-brain-express-seven.vercel.app
- **Supabase project ref:** `xcjyaugcnsdrlfxczaxd`
- **Vercel project:** `open-brain-express` (team "stebe")
- **Local folder:** `C:\Users\jboya\open-brain-express` (deliberately outside OneDrive)

## Deployed edge functions

- enrich-thought
- capture-youtube
- capture-url
- search-brain
- weekly-digest
- open-brain-mcp (deployed with `--no-verify-jwt`)

## Settings

- `LLM_MODEL` = `anthropic/claude-haiku-5.5`, chosen for ~50+ saves/week, a
  mix of notes, YouTube and long articles. Estimated $0.50–$1.50/month.
- Supadata is set up for YouTube transcripts.
- `OWNER_USER_ID` is set to the account created on the live site.
- Schedules: `weekly-brain-digest` (Sundays 08:00 UTC) and `keep-brain-awake`
  (Sundays and Wednesdays 09:00 UTC).

## Repairs made during the build

1. **Enrichment trigger.** This project has no `supabase_functions` schema
   (Database Webhooks was never switched on), so the trigger from
   Session-2-Build.md failed. Replaced it with a small function,
   `public.notify_enrich_thought()`, that calls enrich-thought directly
   through `pg_net`. It sends the same `{ type, table, record }` shape.
2. **Empty tags on longer saves.** Claude Haiku 5.5 "thinks" by default, and
   that thinking counted against `max_tokens`, so longer content came back
   empty. Added `reasoning: { enabled: false }` to the OpenRouter call in
   `supabase/functions/_shared/ai.ts`.
3. **"pdfjsLib is not defined".** The browser blocked the PDF library from
   cdnjs (tracking prevention or an ad blocker). The library now ships with
   the app in `vendor/`, so nothing external has to load.

## Worth knowing

- Some websites block cloud servers, so the Link tab can't read them
  (jillcarnahan.com article pages, for example). Copy the text into the Write
  tab instead.
- On Windows, use `npx.cmd` instead of `npx` in PowerShell. Script execution
  is disabled on this computer, and `npx.cmd` works without changing that.
