# ViBilet Game System Specification (Public Edition)

> **Document status:** Public Portfolio Edition / Development in Progress  
> **Scope:** Core game-system design and primary behaviors  
> **Note:** This document is an excerpted and reorganized public version of an internal specification under development. Numerical balance settings, detailed decision criteria, internal implementation details, and some unresolved items have been omitted. Inclusion of a feature does not imply that it has been fully implemented or tested.

## 1. Document Overview

### 1.1 Purpose

This document outlines the game system and key design principles of **ViBilet**, a digital life-form raising application.

Rather than simply managing numerical parameters, ViBilet aims to let users share their lives with a digital life form (referred to as **LIFE**) that continues to change in real time.

The document covers:

- LIFE state management and autonomous behavior
- Care mechanics tied to real-world time
- Feeding, interaction, and environmental maintenance
- Growth and EVOLUTION PATCH
- Lifespan and the end of life
- MEMORY DATA and inheritance across generations

### 1.2 Related Documents

- `01_REQUIREMENTS.md`: Requirements specification
- `04_UI_FLOW.md`: Screen and navigation specification
- `05_TECHNICAL.md`: Technical specification
- `06_TEST.md`: Test plan

The availability and filenames of related documents depend on the structure of the public repository.

---

## 2. Core Game Loop

```text
EGG
  |
  v
PIX (shared initial form)
  |
  v
Naming and care
  |
  +--> Feeding / Interaction / Environment maintenance
  |                |
  |                +--> Changes to LIFE's state and relationship
  |
  v
EVOLUTION PATCH (select a growth branch)
  |
  +--> BLAZE
  +--> FLOW
  +--> BLOOM
  |
  v
Continue caring for the juvenile LIFE
  |
  v
Growth and lifespan evaluation based on Care
  |
  +--> Requirements met: grow into an adult and continue living
  |
  +--> Requirements not met: natural end at current lifespan
  |
  v
End of life
  |
  v
Data decomposition and lifetime record
  |
  v
MEMORY DATA (when eligible)
  |
  v
Next-generation EGG
```

Every new LIFE hatches from a shared EGG and initially takes the common form called **PIX**. As it grows, it develops the appearance of a branch selected by the user. A change in form represents the growth of the **same individual**, not its replacement.

---

## 3. LIFE State Management

### 3.1 Main Parameters

| Parameter | Purpose | Presentation |
|---|---|---|
| `Hunger` | Current hunger level | Expressed as `SATIETY` |
| `Bond` | Current relationship with the user | Expressed as a relationship state |
| `Care` | Accumulated care throughout life | Generally hidden |
| `Lifetime` | Period during which LIFE can remain alive | Generally hidden |
| `Age` | Time elapsed since birth | Shown in LIFE DATA |
| `Growth Stage` | Current stage of development | Reflected in appearance and profile |
| `Growth Branch` | Selected growth path | Reflected in appearance and traits |
| `JUNK DATA` | Contamination in the digital environment | Reflected through visual effects |
| `DATA ERROR` | Presence of a data anomaly | Reflected in behavior and display |
| `inheritedTraits` | Characteristics inherited from a previous generation | Reflected in behavior where applicable |

Instead of exposing every internal value, the system primarily communicates LIFE's condition through **appearance, expressions, behavior, notifications, and changes to its environment**.

### 3.2 Growth Stages

```text
EGG -> PIX -> JUVENILE -> ADULT
```

- **EGG:** State before normal care begins.
- **PIX:** Shared initial form with no assigned growth branch.
- **JUVENILE:** Young form that acquires a growth branch through an EVOLUTION PATCH.
- **ADULT:** Mature form reached when the required growth conditions are met.

### 3.3 LIFE DATA

LIFE DATA presents fields such as `NAME`, `SPECIES`, `AGE`, `LIFE RHYTHM`, `PERSONALITY`, `CONDITION`, `BOND`, `SATIETY`, and `MEMO`.

A change in form preserves the individual's name and birth time while updating form-dependent information such as species, personality, and daily rhythm.

---

## 4. Real-World Time and Activity States

### 4.1 Real-Time Progression

LIFE's passage of time does not depend on whether the application is running. Time continues while the app is closed, and the relevant changes are applied when the app is opened again.

Elapsed time since birth, hunger, growth checks, and lifespan are managed using stored timestamps and the current time.

### 4.2 Activity States

LIFE has the following activity states based on its daily rhythm:

| State | Meaning |
|---|---|
| `ACTIVE` | Period of primary activity |
| `DROWSY` | Transition period between activity and sleep |
| `SLEEPING` | Period of primary sleep |

Daily rhythms may vary by form, and the current activity state is derived from the current time.

### 4.3 Priority of State Expressions

Hunger, rejection, data anomalies, and other special conditions can take precedence over ordinary behavior. This allows LIFE's reactions to reflect its current condition.

---

## 5. Feeding and Digi Pellets

`Hunger` is an internal value representing LIFE's hunger, which changes over time. Users can reduce hunger by feeding LIFE Digi Pellets.

- Digi Pellets are a care resource with a tracked inventory.
- A daily allocation is provided, with safeguards against duplicate distribution on the same day.
- Automatic feeding is designed to operate according to settings and available inventory.
- Prolonged failure to meet essential survival needs can lead to the end of life.

Distribution amounts, consumption quantities, hunger adjustments, and end-of-life thresholds are omitted from this public edition.

---

## 6. Bond: The Relationship with LIFE

`Bond` represents the **current relationship** between LIFE and the user. It is managed separately from `Care`, which represents the history of care over LIFE's lifetime.

| Relationship state | Description |
|---|---|
| `DISTANT` | Keeps its distance |
| `FAMILIAR` | Has become accustomed to the user |
| `CLOSE` | Shows affection or familiarity |
| `BONDED` | Shares a strong bond of trust |
| `REJECTING` | Refuses interaction with the user |

Interactions such as TOUCH and PLAY affect the relationship. Extended periods without interaction or an unsuitable environment can negatively affect it.

`REJECTING` is not permanent. The system is designed to allow the relationship to be repaired through continued care and attempts to interact. Rejection itself is treated as one way LIFE can express its will.

---

## 7. Care: A Lifetime of Caregiving

`Care` evaluates the care LIFE receives throughout its life. Daily evaluation covers three areas:

| Area | Evaluation |
|---|---|
| `FOOD` | Whether LIFE received sufficient food |
| `INTERACTION` | Whether LIFE had opportunities to interact |
| `CLEAN` | Whether an appropriate living environment was maintained |

Achievements are tracked on a daily basis so that repeatedly triggering the same action cannot increase the evaluation without limit. Accumulated Care contributes to lifespan extensions and growth decisions.

---

## 8. JUNK DATA and DATA ERROR

### 8.1 JUNK DATA

JUNK DATA is unwanted data generated through LIFE's activities. It functions as an environmental-maintenance mechanic adapted to a digital life form.

As JUNK DATA accumulates, visual effects appear in stages, including floating objects on the HOME screen, noise, corrupted-looking profile displays, and altered character imagery.

### 8.2 Cleaning and Effects on LIFE

Users can remove JUNK DATA through cleaning actions. Neglecting the environment affects Care and Bond.

### 8.3 DATA ERROR

If contamination persists, LIFE may develop a DATA ERROR. This can cause unusual behavior and visual effects, but it does not directly cause the end of life.

Once a DATA ERROR has occurred, recovery requires a corrective PATCH rather than cleaning alone.

Detailed contamination stages, thresholds, and cleaning procedures are omitted from this public edition.

---

## 9. Autonomous Behavior

Even without user input, LIFE acts autonomously according to its activity state and internal condition.

Initial shared actions include `IDLE`, `WALK`, and `SLEEP`. The architecture allows character-specific actions and reactions to be added.

Behavior decisions take into account activity state, hunger, Bond, JUNK DATA, DATA ERROR, and inherited Traits. The animation configuration for each character is separated from the shared behavior-control logic.

---

## 10. Growth and EVOLUTION PATCH

### 10.1 Growth Branches

At a growth event, PIX receives an EVOLUTION PATCH and develops along one of three branches:

| PATCH | Branch |
|---|---|
| `BLAZE` | Fire |
| `FLOW` | Water |
| `BLOOM` | Plant |

### 10.2 Continuity Through Growth

Growth events occur based on elapsed time since birth. Even when a PATCH changes LIFE's appearance and profile, its individual identity, name, Age, Bond, Care, and other care-related states are preserved.

### 10.3 Adulthood

Growth into an adult is linked to a Lifetime extension evaluation based on Care over a defined period. A LIFE that meets the conditions can develop into the adult form of its selected branch.

An **EVOLUTION PATCH**, which changes LIFE's form, is distinct from a **SYSTEM PATCH**, which represents new functionality being added to the application within its fictional world.

---

## 11. Lifetime and End of Life

### 11.1 Lifetime

LIFE has a finite lifespan. Its final lifespan is not fully fixed at birth; it can be extended in stages based on Care.

Exact lifespan durations, extension increments, and required Care Rates are omitted from this public edition.

### 11.2 Reasons for the End of Life

| Reason | Internal state | Meaning |
|---|---|---|
| Natural end | `NATURAL_END` | LIFE has reached its lifespan |
| Failure of life support | `LIFE_SUPPORT_FAILURE` | Essential survival conditions were not met |

Once life ends, care actions are disabled and LIFE's lifetime data is finalized. The ending sequence and eligibility for inheritance depend on the reason for the end of life.

---

## 12. MEMORY DATA and Generational Inheritance

### 12.1 Lifetime Records

For a LIFE that reaches a natural end, its lifetime can be recorded as MEMORY DATA.

MEMORY DATA is designed to include the individual's name, final form, growth history, care records, and characteristics that may be eligible for inheritance.

### 12.2 Inheritance by the Next Generation

The next generation is not a copy of its predecessor. It is a new individual born from the shared EGG.

When inheritance is selected, some aspects of the previous LIFE's personality or behavioral tendencies may remain as **Traits** in the next generation. In contrast, name, age, hunger, Bond, Care, and growth stage begin as the new individual's own state.

The goal is not to recreate the previous LIFE, but to convey the experience of **recognizing traces of a previous life within a new one**.

Specific Trait selection and application rules are omitted from this public edition.

---

## 13. Notifications and LIFE DATA

### 13.1 Notifications

LIFE is designed to send local notifications in response to hunger, requests for interaction, data anomalies, and growth events.

Notifications are reevaluated as state changes, and duplicate notifications for the same event are suppressed. Disabling notifications does not stop in-game time.

### 13.2 LIFE DATA

LIFE DATA is a screen for viewing information about the current individual.

Rather than exposing internal state values directly, it translates them into understandable representations such as relationship status and satiety.

Visual corruption caused by JUNK DATA does not alter the underlying saved data.

---

## 14. Daily Updates and State Consistency

Daily updates handle states managed on a calendar-day basis, including food distribution, Care evaluation, Bond change limits, and notification counts.

The app does not need to be running when the date changes. At the next launch, the system determines which periods have not yet been processed.

Daily updates are handled separately from growth and Lifetime evaluations based on birth timestamps. This separation helps preserve state consistency after long periods without opening the app or after exiting during an event.

---

## 15. Key Design Priorities

ViBilet's game system emphasizes the following principles:

1. **Consistency with real-world time:** LIFE's time progresses even when the app is closed.
2. **Separation of state and presentation:** Internal values are translated into behavior and visual effects rather than exposed directly.
3. **Continuity of the individual:** Growth and changes in form do not reset the history of care.
4. **Separation of caregiving and relationships:** Care and Bond are managed as distinct concepts.
5. **Finite life and inheritance:** LIFE eventually reaches an end, while MEMORY DATA creates connections across generations.
6. **Extensibility:** Character-specific behavior, profiles, and animations are separated from shared game logic.

---

*This document is a public-facing design overview for portfolio purposes. Exact decision thresholds, detailed implementation specifications, and unresolved development items are maintained in the private internal specification.*
