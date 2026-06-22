# 0001-my-slack-proxy — An always-on Slack proxy that covers for you and guards your attention

## Problem

Relevant Slack conversations and decisions stall whenever you're heads-down, in a meeting, or offline — colleagues wait on a reply only you can give, and momentum is lost. At the same time, staying reachable means wading through a firehose where most messages don't actually need you, so deciding what deserves your attention becomes a daily tax. You pay on both sides: things wait on you when you're away, and triaging the noise drains you when you're present. Today the only way to cover both is to be personally present in Slack all the time — which doesn't scale and steadily burns the very attention it's meant to protect. The pain lands on you first, and on everyone whose work is blocked waiting for you.

## Outcome

An always-on proxy watches the Slack activity that's relevant to you, filters out the noise, and takes a competent first action in your voice — answering the obvious, covering or surfacing the uncaught-but-relevant, and asking you only when judgment is genuinely required. It is a disclosed, distinct entity — openly not you and never impersonating you — that nonetheless carries your context, knowledge, and voice. It represents you in a plain, matter-of-fact register: no servile deference or honorifics when it speaks about you, and equally no pretending to be you. Its remit is the first action and the attention gate; it does not silently make consequential calls or complete the substantive work behind a request — those it hands back to you.

User stories:

- **Attention gate** — irrelevant messages never reach you; only what genuinely needs you surfaces. The filtering is itself a primary outcome, not a side effect.
- **Answer the obvious** — when a message is addressed to you and the right reply is obvious, the proxy replies so you never have to touch it.
- **Cover your absence** — when a message isn't addressed to you but is relevant, the proxy surfaces it to you, or replies on your behalf when a reply is clearly warranted, so threads don't stall while you're away.
- **Ask, don't guess** — when something is relevant but the right action isn't obvious, or the stakes are high, the proxy asks you rather than acting.
- **Disclosed, plain-spoken identity** — its messages are unmistakably from your proxy and never impersonate you; when it refers to you it stays plain and matter-of-fact — no honorifics, no fawning, none of a subordinate's deference toward you.
- **As informed as you** — the proxy draws on the same organizational context and knowledge you would when forming an action, so its first actions reflect what you'd actually do.
- **Sounds like you, in the room's language** — fluent in the working language(s) of the workspace and matched to your tone; built in English first, able to adapt to the languages actually used around you.

System policies:

- **Business-appropriate conduct** — every first action is safe to send in a professional workplace; the proxy carries a working-professional attitude by default.
- **Restraint over reach** — whenever relevance, clarity, or stakes are uncertain, the proxy errs toward surfacing or asking rather than acting. That restraint is the safety valve that makes its autonomous first actions trustworthy.

It's working when nothing relevant slips (every message that needed you is caught and either acted on or surfaced), you spend measurably less attention triaging and responding, relevant messages get a competent first response quickly even when you're unavailable, and the proxy's actions match your judgment and tone closely enough that you rarely correct them.

## Non-goals

- Not a general-purpose chatbot or workspace-wide assistant — it acts only within the slice of Slack relevant to you.
- Not an autonomous decision-maker on consequential matters — it takes the first action and escalates the substance.
- Not a stand-in for you doing the actual downstream work a request implies — its job ends at the competent first action (reply, acknowledge, route, gather, surface).
- Not an impersonation of you — it is always a disclosed, distinguishable agent.
