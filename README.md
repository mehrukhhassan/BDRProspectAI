# BDRProspectAI

An AI prospecting agent built on [Dust](https://dust.tt) that turns a company name and a target persona into a sourced, ready-to-use outbound brief in about two minutes.

Built by **Mehrukh** | B.S. Economic Data Analytics, DePaul University

---

## Why I built it

Good outbound starts with good account research: knowing what a company is doing right now, who actually buys, and which hook will land. That research is slow to do by hand, and generic AI answers are fast but unreliable. I wanted an agent that is fast **and** honest about what it knows versus what it is guessing.

## How it works

**Input:** a company name + a target persona (e.g. `Gong. Target persona: VP of Sales.`)

**Output:** a structured brief with six sections:

| Section | What it contains |
|---|---|
| Company snapshot | Product, customers, scale, funding, leadership, competitors |
| Growth signals | Dated trigger events, each with a note on why it matters to a seller |
| Buyer personas | Buying committee mapped as economic buyer, champion, evaluator, blocker |
| Cold email opener | Short email built on one verified, less-obvious hook |
| Cold call opener | Talk track ending in a qualifying question |
| Sources | Every claim linked to its source |

**Tools used:** Dust agent with web search and web browsing.

## Design principles

1. **Label the evidence.** Every claim is marked as verified (company source), estimate (third-party data), or unverified (aggregator, LinkedIn, secondhand).
2. **Primary sources first.** Hiring claims are checked against the company's own careers page or applicant tracking system before appearing in a brief.
3. **Skip the obvious hook.** Funding announcements are the hook every rep uses. The agent prioritizes hiring reqs, launches and engineering posts instead.
4. **Challenge the persona.** If the target persona is unlikely to own the budget, the agent says so and builds a referral path into the outreach.
5. **Date everything.** Undated or stale signals (older than ~12 months) are flagged or excluded.

## Sample runs

| Account | Persona | Runtime | Notable judgment call |
|---|---|---|---|
| Ramp | Not specified | 1:51 | Advised against the funding-round hook; sequenced SDR managers before the SVP of Sales |
| Retool | VP of Engineering | 1:59 | Could not confirm the exact title; flagged build-vs-buy as the main objection |
| Gong | VP of Sales | 1:39 | Flagged that infrastructure budget likely sits in R&D; wrote a referral-friendly close |
| Hebbia | Chief of Staff | 1:31 | Found no SDR/BDR reqs; could not confirm a Chief of Staff exists and said so |

Full sample briefs are in [`/samples`](./samples).

## Change log: the verification fix

**v1 issue.** In the Ramp brief, the agent claimed Ramp was hiring SDR managers across SMB, mid-market and enterprise, based on job-board aggregators.

**How it was caught.** I asked the agent for its source. On re-checking Ramp's official careers board, the SMB and Enterprise roles were live, but the Mid-Market posting had been removed six months earlier. The aggregator was surfacing a dead listing.

**Fix.** The email opener was rewritten using only confirmed facts, and a new rule was adopted: verify hiring against the company's own applicant tracking system, label aggregator-only claims as unverified.

**Result.** In the following runs, the agent reported "hiring: not verified" when it could not reach a primary source (Gong), rejected an outdated hiring story, and verified directly against the careers page when it could (Hebbia).

**Lesson.** AI makes research fast. A human still has to ask "how do you know that?"

## Known limitations

- **No seller context.** The agent infers what the seller offers, so openers include a `[what you do]` placeholder.
- **Citation accuracy.** Outputs still need a human check; one brief attributed a statistic to the wrong source.
- **LinkedIn-sourced names and titles** are only as reliable as the profile.
- **Verification was reactive in v1**, added after an error was caught.

## Roadmap (v2)

- [ ] Add seller product and value proposition as a third input for fully personalized openers
- [ ] Build primary-source verification into the agent instructions from the start
- [ ] Add a self-check step: confirm every citation matches its claim before output
- [ ] Generate a 3-touch sequence (email, call, LinkedIn) per persona
- [ ] Export briefs in a CRM-ready format

## Repo structure

```
/README.md
/prompts/agent-instructions.md   Dust agent instructions
/samples/ramp.md
/samples/retool.md
/samples/ramp-verification-audit.md
/samples/gong.md
/samples/hebbia.md
```

## Disclaimer

Briefs are generated from public sources and are for research and portfolio purposes. No outreach was sent to the people or companies named. Company facts reflect sources available in September 2026 and may have changed.
