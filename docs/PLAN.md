# pac-track — Plan

Who funds whom in US politics, with sources, in neutral terms.

Domain: pac-track.com · Planning whiteboard: https://claude.ai/artifact/EbvKiJ8nyjYshKGzdc82YR

## Product principles

- **Audience:** voters.
- **Neutral, factual copy.** Numbers, dates, sources and stance verbs (supports, opposes, contributed, spent). No adjectives, no loaded language, no attitude.
- **Tags are positions with evidence, never labels.** Example: "Supports [bill/policy]. Source: group's 2025 legislative scorecard, item 4."
- **No sign-in to read.** Email-verified identity only for contributors (proposing tags, voting) and email digests.
- **Open source, AGPL-3.0.**

## Roadmap

### Phase 1: Federal
- OpenFEC bulk ingest: candidates, committees, individual contributions, committee-to-candidate, independent expenditures (Schedule E), conduit earmarks
- api.congress.gov: members, bills, roll-call votes
- `unitedstates/congress-legislators`: cross-source member IDs
- Senate LDA: lobbying filings
- Officials & Candidates, Races, Donors, Bills & Votes, Lobbying pages
- Issue lenses + tag pipeline (v1)
- Global search, zip/address lookup

### Phase 2: Arizona
- See The Money API (Arizona Secretary of State filings, nightly refresh)
- Open States v3 + bulk data: AZ legislators, bills, votes

### Phase 3: More states
- One connector per state, ordered by data quality
- Data Coverage page: per-state grid of sources, freshness and gaps

### Phase 4: Secondary data
- Financial disclosures
- Stock trades

### Phase 5: County & city
- Local offices, long term

## Decisions

- Officeholders and challengers, each labeled (incumbent, challenger, former).
- Current cycle and career totals in the same UI.
- Direct contributions, outside spending and conduit earmarks shown as **separate** amounts, never summed into one.
- Individual donor names shown only above a threshold; employer aggregation below it.
- FEC donor data is never used for solicitation or commercial purposes (52 U.S.C. § 30111(a)(4)), including digests.
- Tags re-verified continuously: event-triggered (source page change, new filing, new scorecard) plus a rolling sweep with a maximum tag age. Monthly AI spend cap.
- Rebuttals from officials and campaigns: maintainer notified first; approval by the maintainer or an approved reviewer.

## Issue lenses

v1, ranked by Pew Research Center's April 2026 "very big problem" survey and Pew's January 2026 economic-concerns survey:

1. Money in politics
2. Health care costs
3. Cost of living
4. Housing & homelessness
5. Government ethics
6. Budget & taxes
7. Immigration & border
8. Drug addiction
9. Gun policy
10. Crime & public safety
11. Climate & energy
12. Jobs & wages

Next tier: civil rights & LGBTQ equality · foreign policy & defense (incl. Israel) · education · Social Security & Medicare · abortion

## UI

- Classic layout: sidebar nav + global header with search.
- Search returns grouped results (people, donors, bills, committees) and detects zip codes and addresses.
- Sidebar: Officials & Candidates · Races · Donors · Issues · Bills & Votes · Lobbying · Disclosures & Trades · Methodology · Coverage · Corrections
- Every view's state lives in the URL (shareable permalinks instead of saved searches).
- Component library and design system: **TBD (Jaiden decides before coding UI).**

### Feature list
- Rep page reorganized by the user's chosen issue lenses
- Money next to votes: contributions from groups with documented positions on an issue, next to the official's votes on that issue's bills
- Timeline: contributions and votes on one axis, gaps shown as plain numbers
- Compare any two officials or any two donors
- Snapshot stamps: "Data as of [date], FEC filing [ID]" on every number
- CSV export, embeddable widgets, shareable cards
- RSS feeds per official, donor or issue; email digests without accounts (confirmation link + unsubscribe token)

## Core systems

### Tag pipeline
1. Contributor proposes or flags a tag, with a source URL.
2. Agent fetches the source, confirms the claim against it, archives a copy, writes a short neutral summary.
3. Maintainer or approved reviewer approves.
4. Each applied tag shows its full history (proposer, agent findings, approval date).

Votes decide what gets reviewed first, never what is true.

Tag schema: issue, stance (supports / opposes), evidence URL, archived copy, date, verification status, history.

### Donor entity resolution
Merge donor records across sources ("John Smith, Acme" ≈ "J. Smith, ACME Inc") with a confidence score and a public "report a bad merge" flow.

### Roles
- Maintainer (Jaiden)
- Approved reviewers
- Contributors (email-verified)
- Readers (anonymous)

## Data sources

| Source | Covers |
|---|---|
| OpenFEC API + bulk files | Federal money |
| api.congress.gov | Federal bills & votes |
| unitedstates/congress-legislators | Member IDs across sources |
| Senate LDA | Federal lobbying |
| See The Money (AZ SOS) | Arizona money |
| Open States (Plural) v3 + bulk | State legislators, bills, votes |
| State disclosure agencies | Other states' money |

Not available: OpenSecrets API (registrations closed April 2025), ProPublica Congress API (shut down), FollowTheMoney (state data not updated after 2024).

## Data volume and hosting (estimates)

- FEC 2004–2026: ~356M rows, of which ~282M are individual contributions. ~20 GB as a compressed DuckDB file; estimated 125–195 GB in Postgres with indexes (55–90 GB for the last three cycles).
- **Architecture:** raw transactions stored as Parquet in object storage and processed with DuckDB during the nightly ingest. Postgres holds only what the app serves: rollups by recipient × donor × cycle, plus above-threshold donor rows. Estimated 10–30 GB.
- **Cost:** roughly $40–90/month on Supabase (Pro plan + Small/Medium compute + disk), plus AI costs for tag verification.

## Open items

- Component library + design system
- Donor-name threshold (dollar amount)
- Tag max age for the rolling re-check sweep
- Monthly AI spend cap
- Reviewer onboarding process
