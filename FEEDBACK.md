# Client review: Larkspur disruption-care agent

**To:** Room 1  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-11-19 04:41

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. PITCH.md claims 5 of 5 resolved in 22 turns and 18 tool calls, but readout-trace.json holds one run of 4 turns and 5 tool calls.**

The only committed wire evidence is readout-trace.json: 4 API turns, 5 tool calls, 19268 tokens in, 999 out. The five-PNR totals in PITCH.md (22 turns, 18 calls, about 19,700 in) have no file in this repository behind them. There is also no evals/cases.json, so 'resolved' has no stated pass criterion.

Run python3 run.py --all --trace and paste the totals footer so the 22 and the 18 can be checked.

**2. readout-trace.json shows 0 cache read, 0 written and no cache_control anywhere in agent.py, with 19268 input tokens on a 4-turn chat.**

The system prompt and a tool list of 10 schemas are re-sent on every turn through the system= argument and tools=tools in run_agent(). Nothing in the diff marks any of it for caching, and the trace shows a hit ratio of None. Whatever model is in use, those input tokens are billed in full each turn.

Run python3 run.py --tool-tax and paste its output next to the readout-trace.json token line.

**3. The run in readout-trace.json never called care_entitlements, the one tool you built, and bench-after.json is not among the files.**

The tools called were lookup_booking, get_flight_status, next_available_day, check_policy, search_alternatives. Your diff adds care_entitlements to LOCAL_TOOLS, and the trace shows no call to it. Banked gates stop at 2.2, so 3.1 and 4.1 have nothing banked, and no before and after comparison exists in what I can see.

Run python3 bench.py --compare on your before and after labels and paste the output.

**4. TONE_ADDENDUM in agent.py is still 0 characters, and R8KD3F is the failure your own PITCH.md names.**

PITCH.md says R8KD3F got a calm, normal resolution in 4 turns with no check on tone. Neither a model name nor a longer thinking budget fills an empty TONE_ADDENDUM or adds an escalation rule for abuse. The prompt and tool descriptions are where that rule has to live, and the diff has nothing there.

Run python3 run.py R8KD3F --trace and paste the tool sequence and final answer.

**5. Your diff to run_agent() fixes message history and answer capture, and nothing in the repo shows the before and after behavior.**

The diff changes the assistant message from text_of(response) to response.content and moves answer = text_of(response) outside the loop. Those are real changes to what the loop returns, and they apply to every PNR. Eval cases and a verifier pass would show whether the other four resolutions moved, and verify.py differs from the shipped pack, so a pass from it carries less weight.

Run python3 verify.py 1.2 and paste the full output, and list which given files you edited.

## Your four answers

Four of the lines in your PITCH.md are answers to me rather than to your
verifier, and somebody on your side wrote them between our sessions. I read
those beside the code, not instead of it. Where an answer is carrying a number,
I have said whether the repository backs it.

| My question | Your answer | My read |
| --- | --- | --- |
| **What it costs** | Model cost is about 20k input tokens per chat, and Larkspur's own loaded figure is about $0.14 per resolved contact against $6.90 for a human. bench.py reports model cost only, which is about 40% under the loaded figure. | **Not supported by the repository.** The $0.14 against $6.90 and the 'about 40% under' figure have no artifact in the repo; bench-after.json and the loaded-cost source are not visible. The 20k input tokens roughly matches the 19268 in readout-trace.json, but that is one run. |
| **When it is wrong** | It can be calm when it should not be (R8KD3F), and it only sees a frozen snapshot of fares and availability, so a live seat or price could differ from what it quoted. | **Answered.** R8KD3F is a named case and it matches your own Still broken line. The frozen snapshot caveat is specific, though evals/cases.json would be needed to show how often it matters. |
| **Who runs it** | Larkspur's contact-center operations team owns it day to day, and a human agent takes every escalation and every chat the agent cannot close. | **Thin.** 'Larkspur's contact-center operations team' names a team and no person, and TEAM.md or the escalate_to_human queue argument would be where an owner and a handoff path are written down. |
| **What you left out** | Voice (signed out of scope), group bookings (G2HL9V escalates), and the roughly 42% of chats humans still take, which cost about $1.5M a year and do not go away. | **Not supported by the repository.** The '42% of chats humans still take' and '$1.5M a year' appear nowhere in the files I can see, so evals/cases.json or a source file would have to back them. The voice and G2HL9V omissions are specific, and the group escalation is plausible given escalate_to_human's description. |

All four answered. Bring the artifact behind each one to our next meeting, not the sentence.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (286 lines)`
- `PITCH.md`
- `TEAM.md`
- `readout-trace.json`
- `readout.html (evidence block)`
- `PITCH.md (4 of 4 answers to Priya)`
- given files that differ from the shipped pack: `verify.py`, `eval_harness.py`, `bench.py`, `readout.py`, `setup.py`, `pod_sync.py`, `run.py`, `review.py`, `support/__init__.py`, `support/client.py`, `support/data.py`, `support/models.py`, `support/tools.py`, `support/trace.py`, `support/offline.py`, `support/mock_backend.py`, `support/mcp_client.py`, `support/mcp_server.py`, `support/mcp_selftest.py`, `support/access.py`, `support/keyscan.py`, `support/review.py`

Reviewer: `claude-sonnet-5-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
