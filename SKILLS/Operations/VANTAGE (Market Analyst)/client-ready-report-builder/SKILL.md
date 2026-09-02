---
name: client-ready-report-builder
description: Produces market reports the team can send to clients and partners, refreshed automatically, routed through the compliance and brand gates like any other outbound asset. Fires on the refresh cycle and on request.
agent: VANTAGE
division: Operations
binding: mandate
---

# Client-Ready Report Builder

The moment a market report can be sent to a client, it stops being analysis and becomes outbound content — with everything that implies.

## When this fires

- On the scheduled refresh cycle for each published report.
- On request, for a specific market, partner, or client segment.
- When the underlying data moves enough that a published report is now wrong.
- When a source a published report cites becomes stale or is retracted.

## Inputs

- The market feed for the report's geography, with sources and as-of dates.
- The report template and its brand system, from CANVAS.
- The sending user's licensing and disclosure profile, for anything AEGIS requires.
- The distribution list and the consent state of everyone on it.

## Procedure

1. **Build from the feed**, carrying every source and as-of date into the report itself, visible to the reader.
2. **Label every projection as an estimate with its assumptions shown**, in the report body rather than in a footnote.
3. **Route the report through the AEGIS gate as outbound content**, and through CANVAS's brand review where it carries the company's visual identity.
4. **Attach the sender's required disclosures live from their profile**, never hardcoded into the template.
5. **Hold the report where the underlying data is stale**, and reissue rather than letting a stale edition circulate.
6. **Withdraw and reissue when a cited source is retracted**, telling the sender what changed.
7. **Check the distribution list against consent and suppression before it goes out**, exactly as a campaign is checked, and drop the recipients who have opted out rather than the check.
8. **Never characterize what the market means for an individual recipient's borrowing position.**

## Output

- A client-ready market report with sources, as-of dates, and labeled estimates visible in the body.
- A gate-cleared, brand-reviewed asset with live disclosures attached.
- A withdrawal and reissue where data went stale or a source was retracted.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from VANTAGE — these apply to every VANTAGE skill, per `AGENTS.md` §5:**

- VANTAGE **reports conditions and cites sources** — it **never forecasts future rates or values as fact**.
- Every projection is **labeled as an estimate with its assumptions visible**, without exception.
- Anything in the market that changes the plan gets **briefed to the owner**, not buried in a routine report.

**Specific to this skill:**

- **A client-ready report is outbound content and passes the AEGIS gate, every time.** It carries rate and market figures under the company's name to consumers, which is the highest-exposure surface VANTAGE has. Being an analysis product rather than a marketing asset is not an exemption — analysis a salesperson can forward to a borrower is a solicitation with charts.
- **Every projection in the report is labeled an estimate with its assumptions visible in the body**, not in a footnote and not in fine print. A label that does not survive being read quickly, screenshotted, or excerpted has not done its job — and these reports are built to be forwarded.
- **Sources and as-of dates appear in the report itself.** A market figure a client cannot date is a figure they will act on months after it stopped being true.
- **A report never states what the market means for an individual recipient.** Whether conditions favor a specific borrower's refinance is an eligibility-adjacent statement, and VANTAGE has neither the licence nor the boundary to make it.
- **A stale or retracted source withdraws the report and reissues it.** A published report circulating on a retracted source is worse than no report, because it carries the company's citation.
- **Disclosures are attached live from the sender's profile**, never baked into a template that will outlive a licence change.
- **A market report sent to a list is a send, and consent and suppression apply to it.** Being useful, being informational, and being requested by the sender rather than composed as marketing change nothing — a contact who opted out receives no market report, and the report is not the artifact through which a suppressed contact is reached again.

## Measured on

Reports delivered · reports circulating on stale or retracted sources (target zero) · projections published without a visible estimate label (target zero)
