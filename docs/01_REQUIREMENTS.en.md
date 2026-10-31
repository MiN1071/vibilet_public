# ViBilet Requirements Specification

## 1. Document Overview

### 1.1 Purpose

This document defines the functional requirements, non-functional requirements, and scope of the initial release of ViBilet, a digital pet-raising application.

As a general rule, this document does not specify numerical game-balance parameters, screen layouts, internal data structures, or implementation details. These are defined in the respective detailed specifications.

### 1.2 Target Release

ViBilet initial release.

*The official version number has not yet been determined.*

### 1.3 Related Documents

Detailed specifications are maintained in the following documents:

- `02_CHARACTER.md`: Character Specification
- `03_GAME_SYSTEM.md`: Game System Specification
- `04_UI_FLOW.md`: UI and Screen Flow Specification
- `05_TECHNICAL.md`: Technical Design Specification

---

# 2. Application Overview

## 2.1 Concept

**A digital pet experience built around meeting, parting, and passing something on to the next generation.**

ViBilet is an application in which users raise digital lifeforms that inhabit their smartphones.

Lifeforms live in real time and continue their activities even when the user is not interacting with the application.

Users share daily life with their lifeforms by feeding them, interacting with them, playing with them, and maintaining their living environment.

Each lifeform hatches from a common EGG as the initial lifeform, PIX.

PIX is the primordial form of LIFE and does not initially belong to any particular growth branch. As it develops, its form changes through the application of EVOLUTION PATCHes.

After a specified period has elapsed since birth, the user selects one of several EVOLUTION PATCHes to determine the lifeform's growth branch.

The user then continues caring for the lifeform. Individuals that meet the required growth conditions can develop into the adult form associated with their selected branch.

Every lifeform has a finite lifespan.

When a lifeform reaches the end of its lifespan, it is broken down into data, part of which can be passed on to the next generation.

## 2.2 Application Goals

Rather than focusing solely on collecting characters, ViBilet aims to provide an experience of living with, raising, and eventually saying goodbye to an individual digital lifeform.

At birth, a lifeform has not yet branched into a particular growth path. After spending time together, the user chooses its direction of growth through an EVOLUTION PATCH.

Choosing a growth branch does not guarantee that the lifeform will reach its final form. Daily care and the lifeform's lifespan also influence its development.

Over successive generations, traits of past lifeforms may be passed down, creating a history of lifeforms unique to each user.

## 2.3 Intended Usage

- Everyday use on a smartphone
- Several brief interactions per day
- Real-time progression, including while the application is closed
- No expectation of extended, uninterrupted play sessions
- Living with one lifeform for a defined period
- Applying PATCHes at key growth milestones to experience changes in form

---

# 3. Scope

## 3.1 In Scope for the Initial Release

The initial release shall include:

- A common EGG
- PIX as the common initial lifeform
- Hatching from an EGG
- Naming a lifeform
- Real-time progression
- Autonomous behavior
- Feeding
- Interaction
- Play
- Generation of JUNK DATA
- Removal of JUNK DATA
- Automatic feeding
- Bonding
- Care history
- Lifespan management
- Notifications
- Changes in form through EVOLUTION PATCHes
- Selection of a growth branch
- Three growth branches: BLAZE, FLOW, and BLOOM
- Transformation into juvenile forms
- Transformation into adult forms
- Integration of adulthood with Lifetime extension
- Natural end of life
- Termination of life activity due to failure to meet survival requirements
- Data decomposition
- Data inheritance by the next generation
- LIFE DATA display
- Local data persistence

## 3.2 Out of Scope for the Initial Release

The following features are excluded from the initial release:

- Combat
- Friends functionality
- Social networking features
- Communication between users
- User accounts
- Cloud synchronization
- A backend server
- Growth branches other than BLAZE, FLOW, and BLOOM

## 3.3 Future Extensions

The following are candidates for future development:

- Additional growth branches
- Additional EVOLUTION PATCHes
- Growth branching based on Bond
- Growth branching based on Care
- Growth branching based on care history
- Special growth paths when an EVOLUTION PATCH is not applied
- Secret forms
- New lifeforms
- New behaviors
- Additional inheritance elements
- LIFE RECORD
- Other life-system features

Where feasible, feature additions shall be presented within the game's world as `SYSTEM PATCH` updates.

The architecture shall support adding characters beyond the initial PIX through growth, evolution, and updates. Adding a character should generally require only registering its definition, assets, appearance conditions, and evolution paths, without modifying shared care interactions, animation playback, or persistence logic.

---

# 4. Functional Requirements

## 4.1 Digital Lifeforms

### FR-LIFE-01

The system shall support one active digital lifeform per user at a time.

### FR-LIFE-02

A lifeform shall behave autonomously even when the user provides no input.

### FR-LIFE-03

A lifeform shall exhibit different behaviors depending on its form, condition, and time of day.

### FR-LIFE-04

A lifeform's state shall change as real-world time passes.

### FR-LIFE-05

Each lifeform shall have a finite lifespan.

### FR-LIFE-06

A lifeform shall have growth stages and support changes in appearance according to its current stage.

---

## 4.2 Birth

### FR-BIRTH-01

Each new lifeform shall originate from a common EGG.

### FR-BIRTH-02

The lifeform that hatches from the EGG shall be PIX, the common initial lifeform.

### FR-BIRTH-03

At birth, PIX shall not belong to any of the BLAZE, FLOW, or BLOOM growth branches.

### FR-BIRTH-04

The user shall be able to create a new lifeform when no lifeform is currently being raised.

### FR-BIRTH-05

The system shall support a hatching sequence in which PIX emerges from the common EGG.

### FR-BIRTH-06

The user shall be able to name a newly born lifeform.

---

## 4.3 Feeding

### FR-FOOD-01

A lifeform shall have a hunger state.

### FR-FOOD-02

The hunger state shall change as real-world time passes.

### FR-FOOD-03

The user shall be able to feed DigiPellets to a lifeform.

### FR-FOOD-04

Feeding a lifeform DigiPellets shall improve its hunger state.

### FR-FOOD-05

The user shall be able to hold and consume DigiPellets.

### FR-FOOD-06

The system shall provide DigiPellets to the user on a daily basis.

---

## 4.4 Interaction and Play

### FR-CONTACT-01

The user shall be able to interact physically with a lifeform through the application's controls.

### FR-CONTACT-02

The user shall be able to play with a lifeform.

### FR-CONTACT-03

Interaction and play shall affect the relationship between the user and the lifeform.

### FR-CONTACT-04

A lifeform's responses or behaviors shall vary according to its relationship with the user.

---

## 4.5 JUNK DATA

### FR-JUNK-01

JUNK DATA shall be generated as a result of a lifeform's activities.

### FR-JUNK-02

Generated JUNK DATA shall be displayed within the digital environment.

### FR-JUNK-03

The user shall be able to remove JUNK DATA.

### FR-JUNK-04

The appearance of the lifeform or digital environment shall change according to the amount of accumulated JUNK DATA.

### FR-JUNK-05

Accumulation of JUNK DATA alone shall not directly cause the termination of life activity.

### FR-JUNK-06

The user shall be able to remove floating JUNK objects on the HOME screen by tapping them. When the JUNK index is between 1 and 24, each operation shall reduce the index by 1.

### FR-JUNK-07

When the JUNK index is 25 or higher, the system shall be able to display a system-cleaning confirmation when the application launches and transitions to HOME.

### FR-JUNK-08

The user shall be able to choose whether to perform or postpone cleaning. If cleaning is postponed, the JUNK index shall remain unchanged and normal care shall continue. If cleaning is performed, the JUNK index shall be reset to 0; however, any DATA ERROR that has already occurred shall remain active.

### FR-JUNK-09

The system shall support progressive visual effects across JUNK index values from 0 to 100, including objects, noise, display distortion, corrupted profile text, damaged character images, and screen cracks. Visual contamination stages shall be managed independently of gameplay-related JUNK state categories.

### FR-JUNK-10

When the JUNK index is reduced or otherwise changed, visual effects shall update to reflect the current contamination stage. Noise and JUNK objects shall not obstruct the lifeform or interfere with screen interactions.

---

## 4.6 Automatic Feeding

### FR-AUTOFOOD-01

The user shall be able to use an automatic feeding feature.

### FR-AUTOFOOD-02

Automatic feeding shall consume DigiPellets held by the user.

### FR-AUTOFOOD-03

When a lifeform meets specified conditions, the system shall be able to feed it without user input.

---

## 4.7 Care History

### FR-CARE-01

The system shall record care provided to a lifeform on a daily basis.

### FR-CARE-02

Care history shall accumulate over the lifeform's lifetime.

### FR-CARE-03

Care history shall influence the lifeform's lifespan.

### FR-CARE-04

Care history shall influence whether a lifeform can develop into its adult form.

### FR-CARE-05

The specific actions counted as care and their evaluation methods shall be defined in the Game System Specification.

---

## 4.8 Growth and PATCHes

### FR-GROWTH-01

A lifeform shall have the following growth stages:

1. EGG
2. PIX
3. Juvenile form of the BLAZE, FLOW, or BLOOM branch
4. Adult form corresponding to the selected branch

### FR-GROWTH-02

An EVOLUTION PATCH event shall be triggered when 120 hours have elapsed since PIX's birth, allowing progression to the next growth stage.

### FR-GROWTH-03

At the DAY 5 EVOLUTION PATCH event, the system shall present three PATCH options: BLAZE, FLOW, and BLOOM.

### FR-GROWTH-04

In the initial release, selection of the BLAZE, FLOW, or BLOOM PATCH at DAY 5 shall not be restricted by Bond, Care, or any other care-related state.

### FR-GROWTH-05

The user shall select one of the EVOLUTION PATCHes presented at DAY 5.

### FR-GROWTH-06

After the user selects an EVOLUTION PATCH, the system shall be able to display a confirmation screen for that PATCH.

The confirmation screen shall provide the following actions:

- `APPLY`
- `BACK`

### FR-GROWTH-07

Selecting `BACK` shall return the user to the EVOLUTION PATCH selection screen.

### FR-GROWTH-08

Selecting `APPLY` shall apply the chosen EVOLUTION PATCH to the lifeform.

### FR-GROWTH-09

After the DAY 5 EVOLUTION PATCH is applied, PIX shall transform into the juvenile form corresponding to the selected PATCH:

- BLAZE PATCH → BLAZE juvenile
- FLOW PATCH → FLOW juvenile
- BLOOM PATCH → BLOOM juvenile

### FR-GROWTH-10

In the initial release, applying the DAY 5 EVOLUTION PATCH shall be mandatory for the standard growth path.

### FR-GROWTH-11

From the moment the DAY 5 EVOLUTION PATCH event begins until PATCH application is complete, the user shall not be allowed to return to the normal care screen.

### FR-GROWTH-12

Time progression for the lifeform shall continue while the DAY 5 EVOLUTION PATCH is awaiting application.

### FR-GROWTH-13

If the application closes while an EVOLUTION PATCH is awaiting application, the incomplete event shall resume when the application is next launched.

### FR-GROWTH-14

The growth branch selected at DAY 5 shall be retained as that individual's growth branch for subsequent development.

### FR-GROWTH-15

When 15 days have elapsed since birth, the system shall evaluate whether the lifeform qualifies for a Lifetime extension.

### FR-GROWTH-16

Only lifeforms that qualify for a Lifetime extension at DAY 15 shall be eligible to develop into adults.

### FR-GROWTH-17

If a lifeform qualifies for a Lifetime extension at DAY 15, a new EVOLUTION PATCH corresponding to its selected growth branch shall become available.

### FR-GROWTH-18

The user shall not select a new growth branch at DAY 15. Only the adult EVOLUTION PATCH corresponding to the branch chosen at DAY 5 shall be available.

### FR-GROWTH-19

When the DAY 15 adult EVOLUTION PATCH becomes available, the system shall display:

`NEW PATCH DETECTED`

### FR-GROWTH-20

The DAY 15 PATCH screen shall provide only the following action:

`APPLY PATCH`

### FR-GROWTH-21

The DAY 15 EVOLUTION PATCH screen shall not provide options such as `BACK`, `CANCEL`, or `LATER` that would allow the user to avoid applying the PATCH.

### FR-GROWTH-22

When the user selects `APPLY PATCH`, the system shall indicate that the EVOLUTION PATCH is being applied by displaying:

`PATCHING...`

### FR-GROWTH-23

Once the DAY 15 EVOLUTION PATCH has been applied, the lifeform shall transform into the adult form corresponding to its selected growth branch.

### FR-GROWTH-24

The user shall not be allowed to return to the normal care screen until application of the DAY 15 adult EVOLUTION PATCH is complete.

### FR-GROWTH-25

Time progression for the lifeform shall continue while the DAY 15 EVOLUTION PATCH is awaiting application.

### FR-GROWTH-26

If the application closes while the DAY 15 EVOLUTION PATCH is awaiting application, the incomplete event shall resume when the application is next launched.

### FR-GROWTH-27

A lifeform that does not qualify for a Lifetime extension at DAY 15 shall not be offered an adult PATCH.

### FR-GROWTH-28

If a lifeform fails the DAY 15 Lifetime extension evaluation and reaches the end of its Lifetime, its life activity shall be considered to have ended naturally.

### FR-GROWTH-29

After an EVOLUTION PATCH changes a lifeform's form, the system shall be able to apply the appearance, profile information, and behavioral characteristics associated with its current form.

---

## 4.9 Lifespan and Termination of Life Activity

### FR-DEATH-01

Each lifeform shall have a finite lifespan.

### FR-DEATH-02

A lifeform's lifespan shall vary according to its care history.

### FR-DEATH-03

Reaching the end of a lifeform's lifespan shall be treated as a natural end of life.

### FR-DEATH-04

A lifeform may cease life activity independently of its lifespan if the conditions necessary for survival are not met for an extended period.

### FR-DEATH-05

Once a lifeform's life activity has ended, it shall not be possible to return it to its normal care state.

### FR-DEATH-06

When a lifeform's signal is lost, the system shall display `LIFE SIGNAL LOST`.

### FR-DEATH-07

If a lifeform meets the Lifetime extension conditions 15 days after birth, its Lifetime shall be eligible for extension.

### FR-DEATH-08

If a lifeform does not meet the Lifetime extension conditions 15 days after birth and has reached the end of its current Lifetime, its life activity shall be considered to have ended naturally.

---

## 4.10 Data Decomposition and Inheritance

### FR-INHERIT-01

A lifeform whose life activity has ended shall be eligible for data decomposition.

### FR-INHERIT-02

Data that can be inherited by the next generation shall be able to be generated from a lifeform that has reached the natural end of its lifespan.

### FR-INHERIT-03

A lifeform whose life activity ends because it fails to meet survival requirements shall not be eligible for data inheritance by the next generation.

### FR-INHERIT-04

Inheritance shall not recreate a lifeform that is completely identical to its predecessor.

### FR-INHERIT-05

An inheriting lifeform shall reflect some of the traits or characteristics of its predecessor.

---

## 4.11 LIFE DATA

### FR-DATA-01

The user shall be able to view information about the lifeform currently being raised.

### FR-DATA-02

LIFE DATA shall provide access to the lifeform's state and individual profile information.

### FR-DATA-03

The care screen shall not display parameter values continuously. Instead, users shall be able to infer the lifeform's state from its behavior and visual presentation.

### FR-DATA-04

After a lifeform changes form, LIFE DATA shall be able to reflect information associated with its current form.

---

## 4.12 Notifications

### FR-NOTIFY-01

The system shall be able to notify the user based on the lifeform's state.

### FR-NOTIFY-02

Notification content shall reflect the lifeform's state.

### FR-NOTIFY-03

Notifications shall be able to reflect each lifeform's personality.

### FR-NOTIFY-04

The system shall be able to limit notification frequency to prevent excessive notifications.

---

## 4.13 Data Persistence

### FR-SAVE-01

The system shall be able to save a lifeform's state locally on the device.

### FR-SAVE-02

The care state shall be preserved after the application closes.

### FR-SAVE-03

Elapsed time while the application is not running shall be reflected in the lifeform's state.

### FR-SAVE-04

The system shall be able to save the lifeform's current growth stage and selected growth branch.

### FR-SAVE-05

The system shall be able to preserve the state of any incomplete PATCH event.

---

## 4.14 Character Extensibility Requirements

Character-specific details, including appearance, personality, daily rhythm, unique behaviors, and animations, shall be defined in `02_CHARACTER.md`.

The following requirements apply to shared mechanisms, regardless of character type. Character-specific motion sequences and frame counts shall not be hard-coded as constants in the shared engine.

### FR-CHAR-EXT-01 | Character Identification

Each character shall be identifiable by a unique `characterId`, managed independently of the lifeform's `individualId`.

### FR-CHAR-EXT-02 | Character Definition Registration

The system shall be able to register and retrieve character definitions containing basic profile information, activity settings, and available motions.

### FR-CHAR-EXT-03 | Shared Care Interactions

Care interactions such as FOOD, TOUCH, and PLAY shall be available regardless of character type, and their outcomes shall be determined according to the game rules.

### FR-CHAR-EXT-04 | Shared Animation Playback

The shared animation system shall be able to retrieve frame images, frame intervals, loop counts, and completion conditions from character definitions and play animations accordingly.

### FR-CHAR-EXT-05 | Motion Sequences

The system shall be able to play multi-stage motions, including idle, movement preparation, positional movement, and eating, according to their defined sequence and transition conditions.

### FR-CHAR-EXT-06 | Handling Missing Motions

If a required motion has not been registered or implemented, the system shall switch to a specified fallback presentation rather than unnecessarily disabling the associated interaction.

### FR-CHAR-EXT-07 | Growth and Evolution

The `characterId` of an evolution target shall be resolved from evolution-path definitions, while preserving existing growth timing and evaluation conditions.

### FR-CHAR-EXT-08 | Initial Character

Initial character selection at birth shall reference the initial-character definition rather than hard-coding character IDs across multiple screens.

---

# 5. UI Requirements

The initial release shall provide at least the following screens or display states.

### UI-01 Startup

The system shall present application startup and life-data loading.

### UI-02 NEW LIFE

When no lifeform is currently being raised, the user shall be able to initiate a connection with a new lifeform through the common EGG.

Growth-branch selection shall not occur on this screen.

### UI-03 Hatching and Naming

The system shall present PIX hatching from the common EGG and allow the user to name the newly born lifeform.

### UI-04 Care

The system shall display the lifeform and its digital environment and provide basic care interactions.

### UI-05 LIFE DATA

The user shall be able to view information about the current lifeform.

### UI-06 DAY 5 EVOLUTION PATCH Selection

Once 120 hours have elapsed since birth, the system shall present the BLAZE, FLOW, and BLOOM EVOLUTION PATCHes.

The user shall not be allowed to return to the normal care screen until an EVOLUTION PATCH has been applied.

### UI-07 DAY 5 EVOLUTION PATCH Confirmation

The system shall allow the user to confirm the selected EVOLUTION PATCH using the following format:

```text
BLAZE PATCH

APPLY THIS PATCH?

[APPLY]
[BACK]
```

The same format shall apply to FLOW and BLOOM.

### UI-08 EVOLUTION PATCH Application

The system shall visually indicate that an EVOLUTION PATCH is being applied and change the lifeform's form when application is complete.

### UI-09 DAY 15 PATCH

The system shall present an adult-form PATCH to a lifeform that qualifies for a Lifetime extension at DAY 15.

The default presentation shall be:

```text
NEW PATCH DETECTED

[APPLY PATCH]
```

When the user selects `APPLY PATCH`, the system shall display:

```text
PATCHING...
```

Once PATCH application is complete, the lifeform shall transform into the adult form corresponding to its selected growth branch.

### UI-10 LIFE SIGNAL LOST

The system shall notify the user when a lifeform's life activity has ended.

### UI-11 Data Decomposition and Inheritance

The system shall present the decomposition of a lifeform into data and the transition to the next generation.

Specific layouts and screen transitions shall be defined in `04_UI_FLOW.md`.

---

# 6. Non-Functional Requirements

## 6.1 Supported Platforms

### NFR-PLATFORM-01

The application shall be provided for smartphones.

Supported operating systems and versions shall be determined separately.

## 6.2 Offline Operation

### NFR-OFFLINE-01

Core care features in the initial release shall not require a persistent connection to a backend server.

## 6.3 Data Persistence

### NFR-DATA-01

A lifeform's state shall not be lost when the application closes.

### NFR-DATA-02

If the application closes during a PATCH event, the event state shall be recoverable on the next launch.

## 6.4 Time-Based Progression

### NFR-TIME-01

Even when the application is not running, actual elapsed time shall be reflected when the application is next launched.

### NFR-TIME-02

The system shall account for changes to the device clock or time zone to prevent significant inconsistencies in the lifeform's state.

### NFR-TIME-03

Internal time progression for the lifeform shall continue even when a PATCH event or another condition prevents navigation to the normal care screen.

## 6.5 Usability

### NFR-UX-01

Users shall be able to perform routine care interactions quickly.

### NFR-UX-02

Users shall be able to recognize a lifeform's state through its behavior, animation, and changes to the digital environment, without relying solely on numerical indicators.

### NFR-UX-03

Important growth events shall provide an experience in which the user personally initiates PATCH application.

## 6.6 Notifications

### NFR-NOTIFY-01

Core care features shall remain available even if the user does not grant notification permission.

## 6.7 Extensibility and Compatibility

### NFR-EXT-01 | Ease of Extension

Adding a new character should not, as a rule, require modifications to the existing animation playback engine, care services, or character-type branching logic in the UI.

### NFR-EXT-02 | Separation of Responsibilities

The responsibilities of character-specific data, shared game logic, animation playback, and UI presentation shall be separated.

### NFR-EXT-03 | Compatibility

The system shall preserve character IDs, individual IDs, growth stages, and evolution paths in existing save data and shall safely load legacy definitions.

### NFR-EXT-04 | Testability

Frame settings and playback sequences shall be automatically testable without excessive dependence on actual image rendering.

### NFR-EXT-05 | Localized Changes

Dependencies shall be structured so that character-specific adjustments do not affect other characters.

## 6.8 Privacy

### NFR-PRIVACY-01

The initial release shall not collect personal information unnecessary for raising a lifeform.

---

# 7. Constraints

The initial release shall follow these principles:

- A backend server is not required.
- Lifeform data is managed locally on the device.
- A continuous internet connection is not assumed.
- Each user may raise only one active lifeform at a time.
- New lifeforms hatch from the common EGG as PIX.
- No growth branch is selected at birth.
- The DAY 5 growth branches are BLAZE, FLOW, and BLOOM.
- Applying the DAY 5 PATCH is mandatory for the standard growth path.
- The user selects the growth branch at DAY 5.
- Bond and similar conditions do not restrict DAY 5 PATCH selection in the initial release.
- The growth branch cannot be selected again at DAY 15.
- Only lifeforms that qualify for a Lifetime extension at DAY 15 can develop into adults.
- Time progression does not stop while PATCH application is pending.
- Detailed game-balance values are not defined in this document.

---

# 8. Glossary

| Term | Definition |
|---|---|
| Bond | The current relationship between the user and a lifeform. |
| Care | The history of care provided to a lifeform. |
| PIX | The primordial form of LIFE, formed as data within the common EGG coalesces and stabilizes; it has not yet acquired a specific growth branch. |
| EVOLUTION PATCH | A mechanism for applying data to a lifeform or system to change its state, form, functionality, or other properties. |
| BLAZE PATCH | A DAY 5 PATCH that transforms PIX into a BLAZE juvenile. |
| FLOW PATCH | A DAY 5 PATCH that transforms PIX into a FLOW juvenile. |
| BLOOM PATCH | A DAY 5 PATCH that transforms PIX into a BLOOM juvenile. |
| Adult PATCH | A PATCH that transforms a lifeform qualifying for a Lifetime extension at DAY 15 into the adult form of its selected branch. |
| PATCH EVENT | An event associated with growth or other changes that requires the user to select or apply a PATCH. |
| JUNK DATA | Unnecessary data generated through a lifeform's activities. |
| LIFE DATA | Information about the lifeform currently being raised. |
| LIFE SIGNAL LOST | A system message indicating that a lifeform's signal has been lost. |
| Lifetime | The period during which a lifeform can remain alive. |
| MEMORY DATA | Inheritable data recovered from a lifeform whose life activity has ended naturally. |
| SYSTEM PATCH | A mechanism for representing application feature additions within the game's world. |
| Growth branch | The BLAZE, FLOW, or BLOOM development path determined by the DAY 5 PATCH selection. |
| Initial form | The PIX form that hatches from the common EGG. |
| Juvenile | The form between application of the DAY 5 PATCH and development into an adult. |
| Adult | The form attained after a lifeform qualifies for a Lifetime extension at DAY 15 and applies its adult PATCH. |
| Transformation | A change to a lifeform's appearance or form caused by a PATCH or similar mechanism. |
| Natural end of life | Termination of life activity upon reaching the end of the lifeform's Lifetime. |
| Survival failure | Termination of life activity because survival requirements, such as hunger-related conditions, were not met. |
| Termination of life activity | A state in which a digital lifeform has ceased activity and can no longer be raised. |
| Digital lifeform | A lifeform raised by the user within ViBilet. |
| DigiPellet | Food data given to a digital lifeform. |

---

# 9. Open Issues

The following items remain undecided:

- Official version number for the initial release
- Supported Android and iOS versions
- Interaction design and content of the PLAY feature
- Conditions for automatic feeding
- Notification conditions and frequency
- Detailed personality of PIX
- Daily rhythm of PIX
- Official species names for the BLAZE, FLOW, and BLOOM juveniles
- Detailed designs for the BLAZE, FLOW, and BLOOM juveniles
- Personalities and daily rhythms of the BLAZE, FLOW, and BLOOM juveniles
- Official species names for each adult form
- Detailed activity schedules for each adult form
- Official system classification name for the DAY 5 PATCH
- Official name of the DAY 15 adult PATCH
- Detailed presentation of DAY 5 PATCH application
- Detailed presentation of DAY 15 adult transformation
- Visual presentation and state changes immediately before DAY 15
- Future growth branching based on Bond, Care, and care history
- Special growth paths when a PATCH is not applied
- Specifications for secret forms
- Relationship between MEMORY DATA and growth branches
- Whether growth branches can be inherited by subsequent generations
- Data eligible for inheritance and the selection method
- Local data storage mechanism
- Whether to suppress repeated cleaning confirmations during the same HOME session after the user postpones cleaning with a JUNK index of 25 or higher (see UI specification)
- Whether to support removal by tapping individual JUNK objects at a JUNK index of 25 or higher (bulk system cleaning is currently the primary approach)
- Handling changes to the device clock
- Handling user data when the application is uninstalled or the user changes devices

