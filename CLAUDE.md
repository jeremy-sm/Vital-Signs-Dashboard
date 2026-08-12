# CLAUDE.md — Vital-Signs-Dashboard

Context for AI assistants working in this repo. Read this before making changes.

## What this is

"Know Your Numbers — Vital Signs Dashboard", a single-page tool for **Success Metrics**, a
financial coaching business for nurse and healthcare entrepreneurs. It is the trend-tracking
companion to the one-off snapshot calculator: where the snapshot is a single reading, this
is meant to show the same vitals over time.

**Status as of 2026-08-12: built and deployed, but NOT live.** The Vercel project is marked
not-live and has no custom domain. Last commit was April 2026. Before doing feature work
here, confirm with Jeremy whether this ships as-is, gets merged into the snapshot project,
or gets archived. That decision is an open item in `Tech Notes.md`.

**Stack:** static `index.html`, no build step. Loads Lucide icons from unpkg CDN.
Sibling project: `business-vital-signs-snapshot` (live, and the more mature of the two).

## The owner

Jeremy is a solo operator, non-technical by his own description. Be concrete: exact
click-paths and exact commands, not concepts. He prefers concise writing, commas or
semicolons over em dashes, and minimal corporate buzzwords. Flag tradeoffs honestly.

## Infrastructure: read this before touching anything network-related

Full reference:

```
~/Documents/Claude/Projects/Success Metrics Business Development/Tech Notes.md
```

The three facts that cause the most wasted time:

1. **DNS for mysuccessmetrics.com is managed inside Kajabi**, not Cloudflare, not
   Squarespace. The domain uses Kajabi's nameserver method, so the zone lives in Kajabi's
   Cloudflare tenancy. Jeremy has no Cloudflare account. Records go in
   Kajabi → Settings → Domain → `>` next to the domain → `+ Custom Record`.
2. **The Squarespace DNS panel is inert.** It accepts records and does nothing.
3. **The Vercel CNAME target is account-specific**: `92c51fdcaa4dbb1d.vercel-dns-016.com`,
   not the generic `cname.vercel-dns.com`. Copy the exact value Vercel shows; drop the
   trailing dot.

## If you add a Kajabi form to this tool

These were established by direct testing on 2026-08-12 against the sibling project's form.
Do not re-derive them.

- **The `authenticity_token` is not validated.** No-token and garbage-token POSTs both
  succeeded. A stale token is not a failure mode.
- **No CORS headers** on either the OPTIONS preflight or the POST. You cannot read the
  response status from JavaScript.
- **`frame-ancestors` excludes subdomains.** Kajabi's thank-you page sends
  `frame-ancestors 'self' ... https://www.mysuccessmetrics.com`, which does not cover
  `tools.mysuccessmetrics.com`. Posting data works; embedding Kajabi responses in an
  iframe does not. Do not design around framing Kajabi content.
- **The endpoint accepts anonymous cross-origin POSTs with no rate limiting.** Include a
  honeypot field.

**Reuse the submit handler from `business-vital-signs-snapshot/index.html`** rather than
writing a new one. It uses `fetch(..., {mode:'no-cors'})` with an `AbortController`
timeout, and the comments there explain why two simpler approaches were tried and rejected.

## Deploying

- Push to `main` → Vercel deploys to production automatically.
- Push any other branch → preview build at a stable branch alias.
- Preview URLs require a Vercel login (deployment protection is
  `all_except_custom_domains`). Custom domains stay public. That is intended.

## URL structure

`tools.mysuccessmetrics.com` is a **hub** for multiple tools, each at a path. The snapshot
already occupies `/snapshot` via `vercel.json`. If this dashboard ships, the intended home
is `tools.mysuccessmetrics.com/dashboard`.

Note the architectural wrinkle: one Vercel project serves one hostname. Serving both tools
under the same `tools.` host means either merging them into one project, or fronting them
with rewrites. Discuss with Jeremy before restructuring; do not silently re-architect.

## Testing

Any Kajabi form submission hits the **live** form. There is no sandbox. Use
`info+something@mysuccessmetrics.com` and prefix names with `ZZ-` so test records are easy
to find and purge.

## Brand

Colors and fonts are CSS custom properties at the top of `index.html` (`--sm-green`,
`--sm-teal`, `--sm-amber`, League Spartan for titles, Libre Baskerville for body). Use the
variables; do not introduce new hex values. Voice is a calm coach: a reading, not a verdict.

## Do not

- Add a build step, framework or dependency without discussing it first.
- Put financial or tax advice in the copy.
- Ask users for account numbers, passwords or tax IDs.
- Change DNS anywhere other than Kajabi.
