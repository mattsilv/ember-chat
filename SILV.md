# My Ember deploy

My copy of [Ember](https://github.com/NYLLON-SOFTWARE/ember-cloudflare), self-hosted on Cloudflare.
This file is mine: upstream doesn't have it, so updates never conflict with it. Don't edit Ember's
own README or CLAUDE.md, because those would conflict.

| | |
|---|---|
| Live | https://chat.silv.app (health: `/up` → 200) |
| Fallback | https://ember-chat.silv.workers.dev |
| Repo | `mattsilv/ember-chat`, cloned at `~/gh/ember` |
| Worker | `ember-chat`, with R2 bucket `ember` and D1 `ember` (unused: storage is `do-sqlite`) |
| Deploys | Workers Builds deploys every push to `main` |
| Upstream docs | [`docs/self-hosting.md`](docs/self-hosting.md) |

## Updating

1. Every Monday, **Update Ember** (`.github/workflows/update-ember.yml`) opens a PR "Update Ember to vX.Y.Z".
   To check sooner: `gh workflow run update-ember.yml -R mattsilv/ember-chat`
2. Merge the PR, and it deploys.
3. `git pull` here.

If the PR is a draft, it has conflicts with my changes. Fix them on its branch, then merge.
To revert an update, revert the merge commit. Migrations only add, so that's safe.
I'm on the 0.5.0 beta, so the first PR arrives with the next stable release.

## Admin

- Ember settings: sign in → Admin (rail) → Workspace settings.
- Forgot the setup password: Worker → Settings → Variables and Secrets → `EMBER_SETUP_PASSWORD`.
- Build token `ember-chat build token` is trimmed to Workers Scripts, Account Settings, User Details
  and Memberships. If a build fails on permissions, check it first. **Edit** it, never Roll.

## Gotchas

- New subdomain won't resolve? Cloudflare is almost never the cause; a cached NXDOMAIN is.
  See machine-ops `docs/solutions/infrastructure/cloudflare-subdomain-nxdomain-negative-cache-2026-11-10.md`.
- Sessions are per address: after a domain change, everyone signs in again.
- Don't add `routes` to `wrangler.jsonc`. The custom domain lives on the Worker's Domains tab.
