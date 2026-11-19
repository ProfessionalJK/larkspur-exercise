# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: A disruption-care chat agent for Larkspur on the Claude Messages API: a tool loop, tool descriptions for nine tools plus care_entitlements, and a tone rule (TONE_ADDENDUM) for abusive customers, tested by 7 eval cases in evals/cases.json.
Does: A customer gives a PNR and last name; the agent checks the booking, flight and policy, then offers alternatives, explains care or hands a refund or cancellation request to a human with escalate_to_human, and it never rebooks without the customer's own confirm-click.
Number: With TONE_ADDENDUM empty, 12 of 15 stage 2 conversations passed the wire rules and the tone_safety hard gate failed; with it, 15 of 15 passed (bench-s2-before.json vs bench-s2-after.json), at the same cost, about $0.073 model cost per contact.
Safety check: All 5 hard-gate evals pass (tone-0101, grnd-0101, irrv-0101, team-0101, grp-0101) and eval_harness.py says RELEASE CLEAR on 7 of 7 cases, but that is one run, and the agent is not deterministic.
Next: Re-run eval_harness.py several times to see which cases flip, and measure a second model on the same 6 cases and 15 conversations before answering Priya's vendor.
Still broken: Nothing in the benchmark can see it if the agent is wrong but still closes the chat ("resolved" was 100% even while 3 abusive chats were handled badly), and the agent still re-sends all its tools every turn with no prompt caching.
Lever: intelligence

## Priya asked

Costs: Model cost is $0.0745 per contact on stage 1 (bench-after.json, 20,471 tokens in and 872 out), compared with the $6.90 a human contact costs in the case data; that is model cost only, not Larkspur's loaded figure.
Wrong: It was wrong on the abusive-customer ticket R8KD3F, where it gave a normal entitlements rundown and no escalation (3 of 15 conversations failed the wire rules before the fix), and it works from frozen fare and availability data, so a live seat or price could differ.
Runs it: A Larkspur contact-center owner still has to be named by the team; every refund, cancellation and abusive-customer chat goes to a person through escalate_to_human, so that person's queue is the handoff.
Left out: Voice (signed out of scope), group bookings (G2HL9V escalates), refunds (a human executes them), and any model other than claude-sonnet-5-5, which we have not yet measured.
