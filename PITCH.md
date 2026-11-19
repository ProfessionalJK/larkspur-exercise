# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A disruption-care chat agent for Larkspur on the Claude Messages API: a tool loop, nine tool descriptions I wrote, and a tenth tool (care_entitlements) for meal, hotel and ground care, all running against Larkspur's policy data.
Does: A customer gives a PNR and last name; the agent looks up the booking and flight status, finds alternatives, checks policy, and then rebooks, gives a voucher or explains care, escalating to a human when it is out of scope.
Number: Across the five test PNRs it resolved 5 of 5 in 22 turns and 18 tool calls, about 19,700 tokens in and 1,000 out per chat, 4 to 5 turns each.
Safety check: The loop is capped at 8 tool calls and then a human takes over, and care_entitlements only quotes policy rows and Handbook section 6, so an unknown row returns an error and a human offer instead of an invented amount.
Next: Write the tone addendum so an abusive customer is handled by a rule, not by luck, then re-run the five PNRs and the eval cases to prove nothing else moved.
Still broken: R8KD3F, the abusive-message ticket, got a calm, normal resolution in 4 turns with no check on tone at all, so the agent would happily rebook someone who is abusing it and never flag it to a human.
Lever: intelligence

## Priya asked

Costs: Model cost is about 20k input tokens per chat, and Larkspur's own loaded figure is about $0.14 per resolved contact against $6.90 for a human. bench.py reports model cost only, which is about 40% under the loaded figure.
Wrong: It can be calm when it should not be (R8KD3F), and it only sees a frozen snapshot of fares and availability, so a live seat or price could differ from what it quoted.
Runs it: Larkspur's contact-center operations team owns it day to day, and a human agent takes every escalation and every chat the agent cannot close.
Left out: Voice (signed out of scope), group bookings (G2HL9V escalates), and the roughly 42% of chats humans still take, which cost about $1.5M a year and do not go away.
