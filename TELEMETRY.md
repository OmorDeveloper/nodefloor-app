# Telemetry

NodeFloor collects a small set of **anonymous** usage events so we can
understand adoption (how many people launch the app, which features get used)
and make the product better. This document is the complete, authoritative
contract: **if an event or property is not listed here, the app does not send
it.** The implementation lives in [`src/main/analytics.ts`](src/main/analytics.ts)
and enforces this list as a hard allowlist — the code and this file are kept in
lockstep. You can also verify it from outside the app: point a proxy at it, or
read what PostHog received under your own install id.

## What is sent

Every event carries only these common properties:

| Property | Example | Notes |
| --- | --- | --- |
| `app_version` | `0.4.2` | The app's own version |
| `os` | `darwin` / `win32` / `linux` | Platform, nothing more |
| `arch` | `arm64` / `x64` | CPU architecture |

The events:

| Event | Extra properties | When |
| --- | --- | --- |
| `first_run` | — | Once, the first time the app ever starts |
| `app_launched` | — | Each app start |
| `update_applied` | `from_version`, `to_version` — version strings, or `unknown` for an install older than this event; `via` — one of `auto` (the app's own updater installed it), `manual`, `unknown` | Once, on the first start after the app's version changes |
| `agent_spawned` | `provider` (CLI engine name, e.g. `claude`, `codex`) | An agent terminal is spawned |
| `feature_used` | `feature` — one of `slack_trigger`, `webhook_trigger`, `hire_install`, `voice_dictation` | At most once per feature per app session |
| `session_ended` | `duration_bucket` — one of `<5m`, `5-30m`, `30m-2h`, `2-8h`, `8h+` | On quit (coarse bucket, never raw duration) |

## What is never sent

No prompts. No agent transcripts or output. No file paths, repo names, branch
names, or hostnames. No email addresses, account identifiers, machine
identifiers, or API keys. Nothing free-form — the property allowlist in
`analytics.ts` drops anything not in the tables above.

## How it stays anonymous

- Events are sent to [PostHog](https://posthog.com) (itself open source) with
  `$process_person_profile: false`, which makes them **anonymous events**: no
  person profile is created and no identity is stored.
- The only identifier is a **random UUID** minted on first run and stored in
  the app's user-data directory (`telemetry-install-id`). It is not derived
  from your machine, and deleting the app's data deletes it.
- The only other thing stored for telemetry is the app version this install
  last ran (`telemetry-last-version`, beside the install id) — it exists so
  `update_applied` can name the version you came from, and it is deleted with
  the app's data in the same way.
- To set `via`, the app reads its own update log (`updater.log`, kept in the
  same user-data directory) to see whether the version it is now running is the
  one its updater downloaded and you asked it to restart into, and whether that
  restart is what actually installed it: the log names each version as it
  starts, so a build other than this one starting afterwards means something
  else did the installing. Only that one-word result leaves the machine — no
  line, path or message from that log is ever sent.
- IP-based geolocation is used only to derive a country for aggregate stats;
  PostHog does not retain the IP on the event.

## Retention

Events are kept for **12 months** and then deleted. The privacy policy states
the same period; if one changes the other has to change in the same release, or
the two documents are making different promises about the same data.

## Opting out

Any one of these fully disables telemetry:

1. **Settings → General → Anonymous usage stats → off** (or uncheck "Share
   anonymous usage stats" during onboarding). Takes effect immediately.
2. Set the standard [`DO_NOT_TRACK`](https://consoledonottrack.com)
   environment variable (any value other than `0`). Respected unconditionally.
3. **Block the endpoint.** Telemetry is best-effort and never retried, so
   blocking `*.posthog.com` at your firewall costs you nothing in the app.
   (The PostHog key is injected only in official release CI, so builds made
   outside it send nothing at all.)

## Licence checking

**This build performs a licence check once a day**, and only if you have entered
a licence key. With no key, the app never contacts the licence server.

What is sent: your licence key, a device identifier, and (unavoidably) your IP.
Nothing else — no prompts, no code, no paths, no agent output.

The device identifier is a **random value generated on your machine** that never
leaves it; what is transmitted is a hash of it combined with your licence key.
So it is not a hardware or IP fingerprint, and two licences on one machine
produce two identifiers we cannot link back together.

**It fails open.** If the server is unreachable, or the response cannot be
verified against the signing key compiled into the app, the app keeps working.
An explicit, correctly signed suspension does not close the app either — it
stops new agents being added, so work in progress is never interrupted.

Device records are kept while a licence is active and deleted 60 days after a
device stops checking in.

To turn it off: clear the key in **Settings → Connections → Licence**. The full
detail, including the lawful basis, is in the privacy policy.

## Self-hosting note

PostHog is open source and self-hostable. Official builds point at PostHog
Cloud (US); the endpoint is a build-time setting (`POSTHOG_HOST`), so the
project can move to a self-hosted instance without any code change.
