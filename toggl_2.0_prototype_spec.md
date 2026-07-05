# Toggl 2.0 — Time Variance Surfacing: Requirements Specification

Meelis Burget
05 July 2026

Job stories with Gherkin acceptance criteria + non-functional requirements

---

## JS1 — Logged-entry crossing toast

**Job story:** When I save a time log that pushes a project's logged hours to or past 90% or over 100% of its estimate, I want a notification, so I know without navigating anywhere.

```gherkin
Feature: Logged-entry crossing toast

  Scenario: Logged time crosses into the warning band
    Given a project with an estimate of 40 hours and 34 logged hours
    And the project's previous logged band is "below-90"
    When I save a time log that brings logged hours to 36
    Then a toast renders with the warning
    And the toast reads "[Project name]" / "36 of 40 hours logged · 90%"
    And the project's previous logged band updates to "90-100"

  Scenario: Logged time crosses into the error band
    Given a project with an estimate of 40 hours and 39 logged hours
    And the project's previous logged band is "90-100"
    When I save a time log that brings logged hours to 41.5
    Then a toast renders with the error
    And the toast reads "[Project name]" / "41.5 of 40 hours logged · 104%"
    And the project's previous logged band updates to "over-100"

  Scenario: A later save within an already-crossed band produces no toast
    Given a project's previous logged band is "over-100"
    When I save another time log that keeps logged hours at or above 100% of estimate
    Then no toast renders

  Scenario: No estimate set
    Given a project has no estimate set
    When I save a time log against that project
    Then no toast renders, regardless of hours logged

  Scenario: Toast auto-dismisses
    Given a toast has rendered
    When the auto-dismiss timeout elapses
    Then the toast disappears without user action
    And there is no manual close control on the toast
```

---

## JS2 — Planned-entry crossing toast

**Job story:** When I save a planned entry that pushes a project's planned hours past 100% of its estimate, I want a toast distinct from JS1's, so I know I'm trending over before hours are actually spent.

```gherkin
Feature: Planned-entry crossing toast

  Scenario: Planned time crosses the estimate
    Given a project with an estimate of 40 hours and 30 planned hours
    And the project's previous planned band is "below-100"
    When I save a planned entry that brings planned hours to 44
    Then a toast renders with the warning tone and a calendar icon
    And the toast reads "[Project name]" / "44 of 40 hours planned · 110%"
    And the project's previous planned band updates to "over-100"

  Scenario: A single save crosses both a logged and a planned threshold
    Given a save simultaneously pushes logged hours across a threshold and planned hours past 100%
    When the save completes
    Then both toasts render, stacked
    And neither toast is merged into the other or suppressed
```

Note: the warning tone here is identical to JS1's 90–100% tier. Color does not distinguish planned-crossing from logged-90%-crossing — icon and copy text are the only distinguishing signal.

---

## JS3 — Persistent sidebar banner

**Job story:** When I open the app and any project is currently logged past 100% of estimate, I want a persistent, collapsible indicator in the sidebar, so I see it before navigating anywhere.

```gherkin
Feature: Persistent over-estimate banner

  Scenario: Banner appears when a project is over estimate
    Given at least one project has logged hours above 100% of its estimate
    When I open or return to the app
    Then a banner renders in the sidebar reading "Over estimate (N)"
    And N equals the count of projects at or above 100% logged

  Scenario: Expanding the banner
    Given the banner is collapsed
    When I click the banner
    Then it expands to list each over-estimate project
    And each row shows the project name followed by "[total logged] of [estimated] hours logged · [percent]%"
    And each row links to that project's Dashboard tab, "Time progress" view

  Scenario: Banner has no permanent dismiss
    Given the banner is expanded or collapsed
    Then there is no close control and no "don't show again" option
    And collapsing the banner only reduces it to the one-line strip

  Scenario: Banner does not render when nothing is over estimate
    Given no project is logged at or above 100% of estimate
    When I open the app
    Then the banner does not render

  Scenario: Banner reappears next session if unresolved
    Given a project remains at or above 100% logged
    And the banner was collapsed in a prior session
    When I open a new session
    Then the banner renders again, expanded, based on current state — not on any memory of the prior session's collapse
```

---

## JS4 — Deep-link generalization

**Job story:** When I click a toast, the banner, or any surface showing the "[X] of [Y] hours logged/planned · Z%" pattern, I want to land on that project's Dashboard, Time progress view, so I can see what drove the number without rebuilding any view myself.

```gherkin
Feature: Deep link to project dashboard, time progress view

  Scenario: Clicking any variance-stat surface opens the correct dashboard view
    Given a project's "[X] of [Y] hours logged/planned · Z%" stat is shown on the Projects list, the Project Overview widget, a toast, or the sidebar banner
    When I click that stat
    Then I land on that project's Dashboard tab
    And the "Time progress" view is selected, not the default "Budget progress" view
```

---

## JS5 — Variance column tooltip

**Job story:** When I hover the Variance column, I want an explanation of the sign convention, so I don't have to infer it.

```gherkin
Feature: Variance column tooltip

  Scenario: Hovering the Variance column
    When I hover or focus the Variance column header or a cell in that column
    Then a tooltip renders reading: "Difference between project's estimated and logged time / + means logged under 100% / - means logged over 100% / empty means logged 100%"

  Scenario: Exactly 100% logged produces an empty cell, not zero
    Given a project has logged hours exactly equal to its estimate
    Then the Variance column cell for that project renders empty
```

---

## Non-functional requirements

- Toast visual treatment — neutral white/light card, colored icon in a tinted circle, colored stat text — must be visually distinct from the existing full-tint success/failure toasts. This is informational content about the user's data, not a system success/failure event.
- Colors are sourced from the live app's own tokens, not invented: warning `rgb(105,69,0)`, error `rgb(138,7,1)`.
- Color is never the sole distinguishing signal anywhere in this feature — icon and copy carry meaning wherever two states share a color (planned-crossing vs. logged-90%-crossing).
- No new terminology is used beyond what's already in the live product: "estimate," "logged," "planned," "variance."