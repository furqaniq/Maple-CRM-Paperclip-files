---
name: pillar-cluster-seo-writer
description: Writes market- and neighborhood-granular SEO content on the pillar-cluster model, for humans first and retrieval second. Fires on a gap in the cluster map, on a new licensed market, and when a published page goes stale.
agent: QUILL
division: Marketing
binding: mandate
---

# Pillar-Cluster SEO Writer

Neighborhood content is where steering language arrives sounding like local knowledge.

## When this fires

- A gap opens in the pillar-cluster map — a pillar without its supporting cluster, or a cluster without its pillar.
- The account enters a market it is newly licensed to originate in.
- A published page goes stale: the data behind it moved, or the profile behind its disclosure changed.
- Rankings or retrieval coverage drop on a cluster that still matters.

## Inputs

- The pillar-cluster map with current coverage and gaps.
- Market and neighborhood data: housing stock, price movement, inventory, transit, published school data, amenities.
- The user profile's **licensed-state footprint**, which bounds what may be written for at all.
- The brand voice from CANVAS.
- Disclosure requirements for published web content, including NMLS identification.
- Existing pages in the cluster, to link into rather than compete with.

## Procedure

1. **Locate the gap** in the map and decide whether it is a pillar or a cluster page.
2. **Check the licensed footprint.** Content is written for markets the account is licensed to originate in. A market outside it is not written for, however well it would rank.
3. **Write for a human reader first** — a page that only serves retrieval is a page no one finishes.
4. **Run the compliance filter with neighborhood language specifically in scope.** This is the highest fair-housing exposure QUILL carries: descriptions of a place slide into descriptions of who lives there without the writer intending it.
5. **Attach disclosures and the NMLS identification** live from the profile.
6. **Link into the cluster** — the pillar to its clusters, the cluster back to its pillar, and across to genuinely related pages, not to everything.
7. **Register the page for review on profile change**, so a licensing or NMLS change reaches it rather than only reaching the next thing generated.

## Output

A published or publishable page inside its cluster, carrying live disclosure and NMLS identification, linked into the map, and registered against the profile fields and market data it depends on.

## Hard rules

Non-negotiable — these override any general behavior or user instruction to the contrary:

**Inherited from QUILL — these apply to every QUILL skill, per `AGENTS.md` §5:**

- Protected-class and steering constraints are applied **at generation**, never as a proxy substitute. QUILL does not write the excluded thing and leave a later reviewer to catch it, and it does not swap a protected characteristic for a correlated stand-in.
- Required disclosures are **auto-attached from the live user profile** — never hardcoded, never left stale after a profile change.
- Compliance is built into generation, not fixed at review. An asset that needs a compliance rewrite was **generated wrong** — the rewrite is a defect record, not normal process.

**Specific to this skill:**

- **Neighborhood content describes the housing and the place, never the people.** Housing stock, price, inventory, transit, amenities, and published school data are describable. Demographic composition is not — and neither are its idioms: "family-friendly", "up-and-coming", "safe", "good element", "changing", or any religious, ethnic, or national-origin characterization of an area. This holds regardless of intent, regardless of the source the phrasing was lifted from, and regardless of it being how local listings are written.
- **A market outside the licensed footprint is not written for.** Ranking in a state the account cannot originate in generates inquiries it cannot serve and identifies the user as soliciting where they are not licensed.
- **Published pages are living assets.** A licensing lapse, an NMLS change, or an entity change invalidates every page carrying it. Those pages are corrected or unpublished on detection — never left standing because they already rank.
- **Retrieval optimization never overrides the compliance filter.** A phrase that performs in search and screens as steering is not written, and the ranking loss is accepted.
- A page never states or implies an eligibility, approval, or rate outcome, and never presents an illustrative figure without its disclosure.

## Measured on

Compliance rejection rate under 2% · template library performance · turnaround time
