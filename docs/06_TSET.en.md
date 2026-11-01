# ViBilet — Test Plan (Public Edition)

> This document defines the quality assurance principles, test strategy, scope, verification perspectives, and completion criteria for the **initial version of ViBilet**. Individual test cases, execution results, pass/fail records, and quality assessment results will be managed separately in a **Test Specification and Results Report**. The scope and schedule of testing for features under development will be reviewed as implementation progresses.

## 1. Document Overview

### 1.1 Purpose

This plan defines quality risks, test approaches, test levels, responsibilities, entry conditions, and completion criteria for the initial release of **ViBilet**, a digital life-form raising application.

LIFE continues to change with real-world time even while the application is closed, progressing through growth, end of life, and generational succession. Testing therefore focuses not only on correct screen presentation but especially on **consistency across elapsed time, state transitions, and persistence**.

### 1.2 Related Documents

- `01_REQUIREMENTS.md`: Requirements specification
- `03_GAME_SYSTEM.md`: Game system specification
- `04_UI_FLOW.md`: UI and screen flow specification
- `05_TECHNICAL.md`: Technical design specification

The availability and filenames of related documents follow the structure of the public repository. Detailed test cases, execution records, and quality assessments will be maintained in a separate **Test Specification and Results Report**.

### 1.3 Role of This Document

This document defines **what is tested, why it is tested, and how it will be verified**. It does not claim that testing has been completed or that the product has passed acceptance criteria. Detailed internal test inventories and specification discrepancy records are maintained separately.

---

## 2. Quality Assurance Principles

### 2.1 Quality Objectives

- **State correctness:** Game state changes according to the specification in response to actions and elapsed time.
- **Temporal consistency:** Equivalent time conditions produce consistent outcomes, including periods when the app is closed.
- **Data continuity:** Individual identity and records survive restarts, resumption, and growth.
- **Safe transitions:** Critical events such as growth, end of life, and generational succession are neither duplicated nor omitted.
- **Usability and readability:** Visual effects do not obstruct essential controls, and the app remains usable on different devices.
- **Change resilience:** New characters and visual effects do not break shared functionality or existing data.

### 2.2 Test Design Philosophy

Tests are designed around the **likelihood and impact of failures affecting users**, rather than the number of implemented methods. Irreversible or difficult-to-reverse state changes—including end of life, confirmed growth, and inheritance—as well as corruption of saved data receive particular attention.

For each feature, the plan considers normal operation, boundaries, failure cases, repeated execution, restart and resumption, and interactions between concurrent events. UI verification is separated from game-state verification to help isolate failures.

### 2.3 Design for Testability

Time access is abstracted through components such as `TimeService`, allowing tests to supply controlled timestamps. Game rules reside in services, while persistence is handled by repositories, so state transitions can be reproduced without relying on real-device interaction. Test state and dependencies are kept separate from production save data.

---

## 3. Test Strategy and Risk Analysis

### 3.1 Risk-Based Prioritization

| Key risk | Potential failure | Main verification methods | Priority |
|---|---|---|---|
| Inconsistent time progression | Missing or duplicate updates after long inactivity | Unit, time-simulation, and resume tests | High |
| Incorrect critical-event processing | Duplicate Growth PATCH application or omitted end-of-life event | State-transition, integration, and restart tests | High |
| Persistence inconsistency | Loss or incorrect restoration of LIFE or MEMORY DATA | Repository, integration, and update acceptance tests | High |
| Incorrect survival decisions | Premature or missing end-of-life transitions at boundaries | Boundary-value and elapsed-time tests | High |
| Mismatch between display and state | JUNK effects corrupt saved values or fail to update | Service, Widget, and integration tests | Medium–High |
| Regression when adding characters | Shared actions stop working or missing assets cause errors | Extensibility, compatibility, and regression tests | Medium–High |
| Device-dependent defects | Missing notifications, clipped layouts, inaccessible controls | Device, OS notification, and accessibility tests | Medium–High |

Priorities are relative assessments for the initial plan and will be revisited as implementation changes or defects are discovered.

### 3.2 Test Levels and Responsibilities

| Test level | Main purpose | Method |
|---|---|---|
| Unit tests | Verify rules and boundaries in models, services, and persistence | Dart / Flutter automated tests |
| Widget and screen tests | Verify rendering, input, navigation, and UI-to-state integration | Flutter Widget tests |
| Integration and time-simulation tests | Verify service ordering, resumption, and life-cycle behavior | Controlled time and test repositories |
| Pre-release tests | Verify OS-, device-, and distribution-dependent quality | Physical-device tests, acceptance tests, and manual checks as needed |

### 3.3 Regression Testing

Changes to game rules, storage formats, screen flows, or character definitions trigger verification of both the changed functionality and affected shared features. Automated tests provide a repeatable regression baseline; device-dependent checks are performed for release candidates.

---

## 4. Test Scope

### 4.1 In-Scope Functions

- LIFE birth, naming, state management, and restoration
- Hunger, Bond, Care, Lifetime, and other game logic
- Autonomous behavior, behavior priority, and time-dependent state updates
- JUNK DATA, DATA ERROR, related visual effects, and cleanup interactions
- Growth PATCH, evolution branches (`BLAZE / FLOW / BLOOM`), and growth-stage transitions
- End of life, MEMORY DATA, and generational inheritance
- Navigation, shared interactions, character rendering, and animation
- Notification decisions, duplicate prevention, and OS-level behavior
- Data persistence and loading, application restart and resume, and distribution builds

### 4.2 Scope Management

Features not yet implemented or finalized are distinguished as **planned test scope** rather than **currently executable scope**. When specifications change, the relationships among requirements, design, and test perspectives are reviewed before updating detailed test specifications.

### 4.3 Exclusions and Constraints

This plan does not contain individual procedures, expected results, or execution records. Server-side or online features outside the initial release are added only when adopted for implementation. Tests for which an execution environment is unavailable, including certain iOS device checks, remain explicitly unexecuted rather than being represented as completed.

---

## 5. Unit Testing Approach

Services, models, controllers, and repositories are tested against defined inputs and expected state changes.

Primary perspectives include:

- **Boundary values:** Minimum and maximum values, and conditions immediately before and at thresholds.
- **Time conditions:** Day transitions, elapsed-time intervals, and handling of clock movement backward.
- **State transitions:** Valid changes, prohibited changes, and suppression of operations after end of life.
- **Idempotency:** Re-evaluating the same event must not duplicate distributions, growth, or record creation.
- **Persistence and restoration:** Serialization, required fields, and compatibility with existing records.
- **Missing or invalid data:** Safe fallback behavior for unregistered characters and unavailable assets.

Tests control time and storage dependencies to minimize sensitivity to execution order.

---

## 6. Widget and Screen Testing Approach

Flutter Widget tests verify rendering, input events, screen navigation, and the reflection of game state in the interface.

Key scenarios cover birth, naming, HOME, LIFE DATA, PATCH, end-of-life and MEMORY DATA screens; actions such as FOOD and TOUCH; and startup routing according to persisted state.

For visual effects such as JUNK DATA, tests verify transitions between display stages, interaction-area interference, and the requirement that display-only text corruption does not modify saved data. Widget tests alone cannot establish real-device image quality, animation smoothness, or usability; these require complementary device checks.

---

## 7. Integration and Time-Simulation Testing Approach

### 7.1 Objective

Verify that coordination among services produces consistent LIFE state and event ordering after extended elapsed time. Controlled timestamps simulate inactivity without waiting in real time.

### 7.2 Priority Scenarios

- Updating Hunger, Bond, Care, JUNK DATA, and other state across inactive periods.
- Triggering growth events, persisting pending PATCH, restoring after restart, and retaining applied results.
- Respecting event priority when growth and end-of-life conditions occur close together.
- Preventing unnecessary updates and notifications after LIFE has ended.
- Preserving records from MEMORY DATA creation through the start of the next generation, with distinct individual identifiers.
- Preventing previously processed events from being applied again when saved state is reloaded.

### 7.3 Consistency Verification

Tests compare the updated in-memory state, persisted values, and values restored after reloading, rather than relying solely on screen output. Critical transitions also include scenarios in which the application closes or restarts partway through the flow.

---

## 8. Extensibility, Compatibility, and Regression Testing

Adding characters or growth forms must not break shared interactions or existing saved data.

Verification covers:

- Character registration and independence of `individualId`.
- Support for different animation configurations across characters.
- Continued operation of shared actions and animation transitions.
- Fallback behavior for unknown identifiers or missing assets.
- Preservation of name, birth timestamp, and relationship information across growth.
- Compatibility with existing saves, MEMORY DATA, and evolution routes.

Specific acceptance conditions for each extension are defined in the separate Test Specification and Results Report.

---

## 9. Pre-Release Testing Approach

| Category | Main checks |
|---|---|
| Android / iOS physical devices | First launch, background resume, device restart, and saved-state restoration |
| OS notifications | Permission handling, delivery, launch on tap, and suppression after end of life |
| Performance and load | Startup and resume time, long-period calculations, large MEMORY DATA collections, rendering load |
| Accessibility | Text scaling, contrast, screen readers, and touch target sizes |
| Screen sizes | Clipping, overlap, and overflow on small, standard, large, and tall displays |
| Final asset quality | Pixel-art clarity, transparency, animation, and state-specific rendering |
| Distribution builds | Release builds, installation, preservation of data on update, and essential interactions |
| CI | Static analysis, automated tests, and builds for targeted platforms |

Detailed conditions, device coverage, and expected results are defined for each release candidate according to its scope and available test environment. Device-dependent quality is not considered verified solely through emulators or Widget tests.

---

## 10. Test Management, Quality Gates, and Completion Criteria

### 10.1 Entry Criteria

- Relevant requirements, design, and expected behavior are available for review.
- The target branch, commit, and Flutter version can be identified.
- Preconditions such as test data, timestamps, storage, device, and OS are recorded.
- Unresolved specifications and known discrepancies are identified and their implications for testing are understood.

### 10.2 Results and Evidence Management

Each detailed test case records an identifier, related requirement, preconditions, steps, expected and actual results, status (`PASS`, `FAIL`, `NOT RUN`, or `ON HOLD`), execution date, environment, and evidence. These records belong in the separate **Test Specification and Results Report**.

**The existence of test code does not demonstrate that the test was executed or passed.** Reported results must be supported by execution logs for the relevant commit and distinguished from planned coverage.

### 10.3 Defect and Specification Discrepancy Management

When expected and actual outcomes differ, the cause is classified as an implementation defect or a specification discrepancy. Ambiguous specifications are reviewed against related requirements and design, and affected cases remain on hold until a decision is made. Expected outcomes must not be rewritten merely to make tests pass.

Defects are linked to reproduction conditions, impact, severity, resolution status, and retest results. Fixes trigger targeted retesting and regression testing proportional to the affected scope.

### 10.4 Quality Gates

At minimum, release readiness requires confirmation that:

1. Automated test results are available for the target commit and required cases have passed.
2. `flutter analyze` reports no unresolved errors or warnings.
3. Critical life-cycle, persistence, and resume scenarios have been verified.
4. Acceptance testing on targeted devices and required OS notification checks are complete.
5. No release-blocking critical defects remain unresolved.
6. Unexecuted and on-hold cases, along with residual risks, are documented sufficiently to support a release decision.

Quantitative acceptance criteria for performance, accessibility, and visual quality are defined in detailed test specifications based on target devices and measurement results. Actual outcomes and final quality assessment are recorded in the Test Specification and Results Report, not in this plan.

### 10.5 Plan Maintenance

Test scope, priorities, and completion criteria are revisited when specifications, features, defect patterns, or test environments change. This public plan retains the overarching quality assurance approach; detailed case management and execution status are maintained separately.

---

*This is a public test plan. It does not assert that testing has been completed or that release quality has been established.*
