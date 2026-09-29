# ALERT for G — COA Check needs two keys before it can go live

2026-09-29 · written early in the COA Check session (13) so you can act while the build runs.

## What's missing

- **The Vercel project has no Anthropic key and no Turnstile secret.** `vercel env ls` listed no environment variables at all. There's no key in the repo, the shell or `~/.config/kalios/` either.
- **There's no Turnstile site key.** Nothing in the repo or the reports has one, and I can't create a Cloudflare widget myself.

Without these, `/api/coa` fails closed ("the check is unavailable"). The fixture run can't call the model, so the brief's gate ("preview only until the fixtures pass") stays shut. Production stays as it is today: no `/coa`, and no links to it.

## What I'm doing meanwhile

I'm building everything and testing all of it except the live model call:

- the function, `scorer.js`, the page, and every placement;
- fixtures;
- the test script, run against a mocked API.

Then I deploy a **preview**. Once the keys are in Vercel, two steps remain: the fixture run against the preview, then the production deploy.

One change I already made: Vercel's **Preview** environment has `TURNSTILE_SECRET_KEY` set to Cloudflare's public test secret (`1x0000000000000000000000000000000AA`). That secret accepts only Cloudflare's dummy token, so the test script can run against a preview. Previews sit behind Vercel's login. Production is untouched.

## Action for G (about 10 minutes)

1. **Anthropic Console** → API keys → create a key in the workspace that has your $25 cap.
2. **Cloudflare** → Turnstile → Add widget:
   - name: `Kalios COA`;
   - hostnames: `www.kalios.health` and `kalios.health`;
   - widget mode: **Managed**.

   Copy the site key and the secret key.
3. **Vercel** → kalios project → Settings → Environment Variables:
   - `ANTHROPIC_API_KEY` = the Anthropic key, for **Production and Preview**, marked Sensitive;
   - `TURNSTILE_SECRET_KEY` = the Turnstile secret, for **Production** only, marked Sensitive. Leave the Preview test value alone.
4. **The Turnstile site key** is public and goes in the page. Either paste it into `coa/index.html` in place of `TURNSTILE_SITE_KEY_PENDING`, or give it to me at the start of the next session.

After that, what's left is a short run: fixtures against the preview, then production per the checklist and IndexNow. The session report (`2026-09-29-coa-check.md`) will list the exact commands.
