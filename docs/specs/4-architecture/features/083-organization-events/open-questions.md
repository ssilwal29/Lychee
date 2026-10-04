# Open Questions – Feature 083

Open questions for Feature 083 – Organization Events. Log every high- and medium-impact question here (table row + Question Details entry) before asking the user; see [open-questions-format.md](../../spec-guidelines/open-questions-format.md). Once answered, fold the outcome into `spec.md` (and an ADR when architecturally significant), then mark the entry resolved. The specification and implementation plan await these decisions.

## Active Questions

| Question ID | Feature | Priority | Summary | Status | Opened | Updated |
|-------------|---------|----------|---------|--------|--------|---------|
| Q-083-01 | 083 – Organization Events | High | Hierarchy and media placement | Open | 2026-10-04 | 2026-10-04 |
| Q-083-02 | 083 – Organization Events | High | Membership, ownership, and visibility | Open | 2026-10-04 | 2026-10-04 |
| Q-083-03 | 083 – Organization Events | High | Create-next-event behavior and copied settings | Open | 2026-10-04 | 2026-10-04 |
| Q-083-04 | 083 – Organization Events | Medium | Event metadata and lifecycle | Open | 2026-10-04 | 2026-10-04 |
| Q-083-05 | 083 – Organization Events | High | Frontend coverage | Open | 2026-10-04 | 2026-10-04 |

## Question Details

### Q-083-01 – Hierarchy and media placement

**Context:** The requested organization → series → event/album → media structure fits the existing nested album tree, but the allowed contents and enforcement are unspecified.

- **Option A (recommended):** Typed album nodes with organization → series → event enforced on creation and moves; media only in event albums. Ordinary albums outside this hierarchy remain unchanged.
  - Pro: Predictable navigation and enforceable structure.
  - Con: No organization/series-level media or event subalbums.
- **Option B:** Typed nodes with event subalbums and media at every level.
  - Pro: Supports more varied galleries.
  - Con: More complex hierarchy validation and navigation.
- **Option C:** Labels only on ordinary albums, without structural enforcement.
  - Pro: Smallest structural change.
  - Con: Cannot guarantee the requested hierarchy.

### Q-083-02 – Membership, ownership, and visibility

**Context:** Albums currently have individual owners. User groups expose member/admin roles, while album permissions separately grant upload, edit, delete, move, download, and full-photo access. They do not define the requested organizer/contributor/viewer organization roles.

- **Option A (recommended):** Explicit organization membership with organizer/contributor/viewer roles; retain an accountable individual album owner. Organizers manage membership and events, contributors upload to events, viewers read. Organization content is private to members unless an organizer explicitly shares an event; membership changes apply to all organization descendants.
  - Pro: Clear roles and consistent organization-wide access.
  - Con: Requires organization-scoped authorization beyond current group roles.
- **Option B:** Reuse existing group membership and per-album grants without new organization roles; keep individual owners and explicit album sharing.
  - Pro: Less new authorization machinery.
  - Con: Does not provide consistent organizer/contributor/viewer semantics across the organization.
- **Option C:** Organization-owned albums with fully separate tenant administration and scoped administrator access.
  - Pro: Stronger independent organization administration.
  - Con: Broad ownership and global-administrator policy changes.

**Boundary:** Option A is not isolation from instance administrators. Strong tenant isolation would need a separate, explicit policy.

### Q-083-03 – Create-next-event behavior and copied settings

**Context:** The request defers automatic recurrence, but does not specify how the next date is chosen or which settings are approved for copying.

- **Option A (recommended):** An organizer chooses the new title and date/time. Create an empty sibling event in the same series; copy location, timezone, description, sorting, and layout only. Apply current organization membership policy; do not copy media, cover/header references, event status, passwords, or public/external shares.
  - Pro: Simple, explicit creation without accidentally copying media or access grants.
  - Con: The next date is entered manually.
- **Option B:** Option A plus a stored weekly/monthly/yearly series cadence that suggests the next date for organizer confirmation; no background scheduling.
  - Pro: Less repetitive date entry.
  - Con: Requires month-end and timezone/DST decisions.
- **Option C:** Automatically generate occurrences from recurrence rules.
  - Pro: Fully scheduled series.
  - Con: Expands the requested initial scope into the separately deferred recurrence engine.

### Q-083-04 – Event metadata and lifecycle

**Context:** Actual event dates must be independent of photo timestamps. Required fields, duration, lifecycle states, and upcoming/past classification are unspecified.

- **Option A (recommended):** Required start date/time and IANA timezone, optional end date/time and free-text location; statuses scheduled/completed/cancelled. Upcoming/past is based on start time, with cancelled events marked separately.
  - Pro: Explicit event ordering and timezone semantics.
  - Con: Does not support all-day or undated draft events.
- **Option B:** Option A plus all-day events and draft status, with dates required before scheduling.
  - Pro: Broader event workflows.
  - Con: More validation and UI branches.
- **Option C:** Date-only events with location, no times or lifecycle status.
  - Pro: Simplest archive workflow.
  - Con: Omits the requested timezone/status behavior.

### Q-083-05 – Frontend coverage

**Context:** Lychee has v7 and v8 gallery frontends. The request includes new navigation but does not identify which frontends need creation, editing, membership, and event-listing interfaces.

- **Option A (recommended):** Deliver the organization/series/event interfaces in v8 plus backend APIs; no new v7 interfaces.
  - Pro: Focuses on the current Nuxt UI frontend.
  - Con: New organization workflows require v8.
- **Option B:** Deliver equivalent interfaces in both v7 and v8.
  - Pro: Both frontends expose the feature.
  - Con: Two component systems and interaction flows to maintain and test.
- **Option C:** Backend APIs only initially.
  - Pro: Smaller first delivery.
  - Con: Defers the requested gallery navigation and management interfaces.

---
*Last updated: 2026-10-04*
