# 0001-my-slack-proxy — SPEC

## Outcome

Every message the proxy observes resolves to exactly one observable action: ignore, auto-reply, cover (surface or reply), or escalate. O-1 through O-5 are those branches and the loop that closes an escalation.

### O-1: irrelevant-message-filtered
When an observed message is judged not relevant to the user, the proxy posts nothing and raises no notification to the user. Silence is the observable signal — the message never reaches the user's attention.

### O-2: obvious-direct-ask-auto-answered
When a message directed to the user has a clear, low-stakes answer, the proxy posts a reply on the user's behalf without first involving the user.

### O-3: uncaught-relevant-message-covered
When a message that is not directed to the user is judged relevant, the proxy either surfaces it to the user or posts a reply on the user's behalf. Exactly one of the two happens; the message is never passed over with no action.

### O-4: unclear-or-high-stakes-message-escalated
When a relevant message's right action is unclear, or its stakes are high, the proxy asks the user and posts no outbound reply on that message until the user responds.

### O-5: escalation-resolved-per-user-direction
When the user answers an escalation, the proxy carries out the action the user directed — send as-is, send after the user's edit, or drop — and takes no divergent action.

## Invariants

### INV-1: every-relevant-message-handled
Every message judged relevant resolves to exactly one of: a posted reply, a surface to the user, or an escalation to the user. No relevant message is silently dropped.

### INV-2: output-always-disclosed
No message the proxy posts is presented as authored by the user. Every proxy-posted message is identifiable as coming from the proxy, never from the user directly.

### INV-3: plain-register-toward-user
In every message it posts, the proxy refers to the user in plain, matter-of-fact language — no honorifics, no deferential or subordinate framing of the user.

### INV-4: output-in-user-voice
Replies the proxy posts on the user's behalf read in the user's own tone and writing style.

### INV-5: output-in-conversation-language
The proxy writes each message in the working language of the surrounding conversation. It operates in English and adapts to the other languages actually used in the workspace.

### INV-6: business-appropriate-output
Every outbound action the proxy takes is appropriate to send in a professional workplace.

### INV-7: context-grounded-actions
The proxy's actions are consistent with the organizational context available to it. It does not post answers that contradict or ignore accessible, relevant context.

### INV-8: continuous-observation
The proxy observes relevant Slack activity continuously, with no monitoring gap tied to whether the user is online or active.

### INV-9: timely-first-response
A relevant, actionable message receives its first action or surfacing within a bounded response window, independent of the user's availability. The numeric target is an operational SLO fixed at DESIGN time.

## Non-goals
- The proxy does not complete the substantive work a request implies; its contract ends at the first action (reply, surface, escalate, route).
- The proxy does not make or commit to consequential decisions on its own — those resolve through escalation (O-4).
- The proxy does not act outside the slice of Slack judged relevant to the user; it is not a workspace-wide assistant.
