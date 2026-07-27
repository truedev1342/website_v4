# website_v4

**Status: not deployable as-is. No setup, configuration, run, or API documentation is provided in this file **

## What this is

A Flask + MongoDB control panel that drives pools of automated websocket clients into
live cam rooms on MyFreeCams and Chaturbate, routed through user-supplied HTTP proxies,
in order to inflate the viewer count displayed for a given performer.

The web UI accepts a target performer's username and a proxy list. A worker layer opens
some multiple of fake guest sessions per working proxy, spreads them across the target
site's chat servers, and holds them open with a heartbeat loop. A MongoDB collection
tracks live session counts, per-target run/stop state, and global caps on connections
and session lifetime.

The targets are third-party production platforms and other people's accounts. This
violates those platforms' terms of service, and the proxy rotation and server spreading
exist to make the traffic harder for them to attribute and block. Inflated viewer counts
affect discovery ranking and earnings on these sites, so the effects land on real people
who are not party to any of it.

## Repository contents

| Path | Role |
| --- | --- |
| [server_api.py](server_api.py) | Flask app: admin UI routes and the internal control API |
| [backend.py](backend.py) | MongoDB access layer, proxy validation, heartbeat and cap bookkeeping |
| [bot3.py](bot3.py) | MyFreeCams worker |
| [chaturbate_bot.py](chaturbate_bot.py) | Chaturbate worker |
| [temp_api.py](temp_api.py) | Scratch/experimental variant, not imported by the app |
| [static/](static/) | Vendored Argon/Bootstrap theme assets and dashboard JS |
| [templates/](templates/) | Dashboard, login, and table views |
| `log.txt` | Committed runtime log (see above) |

## Scope of this README

This file describes what the repository is and what state it is in. It deliberately does
not include installation, configuration, deployment, or endpoint documentation, because
supplying that is what would make this operable by someone other than its author, and
what it does to third parties does not warrant that.

The Flask-plus-MongoDB scaffolding underneath — the proxy-pool health checker, the
thread supervisor with per-target start/stop, and the heartbeat-based liveness tracking —
is a general worker-orchestration pattern with legitimate uses, and could be documented
in full once separated from the payload it currently drives.
