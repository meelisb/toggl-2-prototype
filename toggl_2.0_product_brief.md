# Toggl 2.0 — Time Variance Surfacing Product Brief

05 July 2026

## Problem

- Toggl 2.0 calculates project variance (planned vs estimated time) correctly but never surfaces not uses it to provide the user time intelligence.
- Variance lives only on the Projects page and Project Dashboard, time status also on Project Overview — three places showing the fact, none of them triggering anything when the number moves.
- No alert when a logged entry crosses the estimate or when a planned entry commits the project over estimate before anything's spent.
- Even a user who checks manually gets a number with nowhere to go — no link from "you're over" to "here's what drove it."
- Toggl 2.0 Support bot calls variance "an early-warning system for scope creep." It isn't a warning system enough if it only confirms what already happened.


## Target users

- **Individual Contributor**: scope creep is the named core challenge. Variance-crossing is this scenario. Protects the client relationship, not just margin. Main target group.
- **Freelancer**: variance feeds the profitability decision. Real, but one input among several — less direct fit than IC.


## Why this matters

- Short term: the user learns about an overrun before the client conversation or the invoice, not after, can immediately see what drove it and react to it.
- Long term: this is what "time intelligence" is supposed to mean — data that acts on the user's behalf, not data waiting to be looked up. Surfacing it and connecting it to its own cause in context makes it an intelligence.
- W0 retention: an alert is a return trigger, a passive projects' table the user has to remember to open is not. The persistent banner is the stronger return trigger, a toast message is transient and easy to miss if the user isn't looking at the screen when it fires; a banner is still there next time the app opens.


## Direction

- No new report, no new page. Data and calculation are already correct — the fix is delivery.
- Using maximally existing features and design elements.
- Three moments:
  1. At the moment of crossing estimation — a toast message. Fires on both logged crossing (90% or higher; over 100 % tiers) and planned crossing (a separate, earlier signal).
  2. At next session open — a persistent, collapsible sidebar banner, visible before any deliberate navigation. Collapses to a one-line strip, never fully dismissible due to a recurring risk signal.
  3. From all existing interfaces already showing the "X of Y hours logged/planned · Z%" pattern — a link to the project dashboard with task break.
- The new toast tone is distinct from the existing system success/failure toasts — those mean "the app did something," this means "here's information about your data."


## Non-goals

- Invoicing — no generator in 2.0, CSV/Track export exists.
- Profitability cost modeling — per-member/per-project rate scheduling already exists; remaining gap (no non-labor cost line) is a finance-tooling problem.
- General onboarding redesign — different metric (activation, not W0).
- Any backend or data-model change.


## Success criteria (prototype)

- Entry crosses a threshold → visible signal, same view, no navigation.
- Leading and trailing signals read as different severity even where they share a color — copy and icon carry the distinction the color no longer does.
- Already-over-estimate project → signal visible on session open, before any click, and still there on the next open if unresolved.
- Every signal links to the project's Dashboard Time progress view in one click — success means the user can explain the overrun, not just receive an alert about it.
- Toast fires once per threshold crossing, not once per save against an already-crossed project — a signal that repeats until it's ignored isn't a success.
- Project within estimate → no signal. No false positives, including the exact-100% case (empty, not zero).
- Core logging flow: same number of steps as today.


## Goals / metric

- No W0 baseline is available internally. Hypothesis: surfacing variance at the point of crossing, backed by a persistent return trigger, increases W0 return rate for accounts with at least one estimated project.
- No verified external benchmark to anchor a number.


## Risks

- Stale or careless project estimates undermine the signal.
- The mirror risk: a user can silence the over-estimate banner by raising the estimate, with zero engagement with the actual scope problem. The design can't currently tell that apart from a legitimate re-estimation that makes future estimates more realistic. Not solved here but worth mentioning.
- Alert fatigue from multiple near-estimate projects — mitigated by firing toasts only on 90% or 100% crossing once, not on every save, but not eliminated for accounts running many concurrent over-budget projects.
