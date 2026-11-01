# ViBilet Technical Design Specification (Public Edition)

> **Document status:** Public Portfolio Edition / Development in Progress  
> **Scope:** ViBilet Ver. 0.1  
> **Note:** This document is a public adaptation of the internal technical design specification, focusing on architecture and design decisions. It includes both implemented mechanisms and planned or extensible designs. Exact game-balance values, internal decision thresholds, and unresolved implementation details are omitted. Refer to the source code and tests to verify the current implementation status.

## 1. Document Overview and Design Principles

### 1.1 Purpose

ViBilet is a mobile application for raising a digital life form called **LIFE**, whose state changes with real-world time. This document describes the technical architecture and allocation of responsibilities used to realize the game-system requirements in Flutter and Dart.

The main design principles are:

- Separate UI presentation from game logic so visual changes have limited impact on life-state updates.
- Abstract time retrieval to support elapsed-time processing while the app is closed and reproducible testing.
- Centralize persistence and restoration through repositories to isolate storage dependencies.
- Distinguish persisted state from derived values to reduce inconsistencies.
- Preserve the identity and state of an individual LIFE as its appearance evolves.
- Minimize changes to shared logic when adding new characters.

### 1.2 Related Documents

- `01_REQUIREMENTS.md`: Requirements specification
- `03_GAME_SYSTEM.md`: Game system specification
- `04_UI_FLOW.md`: Screen and navigation specification
- `06_TEST.md`: Test plan

The availability and filenames of related documents depend on the structure of the public repository.

---

## 2. Technology Stack and Architecture

- **Framework:** Flutter
- **Language:** Dart
- **Target platform:** Mobile devices
- **Data management:** Primarily local storage

The application is conceptually divided into the following responsibilities:

```text
User Interaction / Presentation (UI)
                ↓
           Controller
                ↓
       Game Logic / Services
                ↓
          State / Models
                ↓
            Repository
                ↓
          Local Storage
```

This diagram simplifies the principal dependencies; it does not imply that every class is called in a strictly linear sequence. Controllers handle requests from the UI, services implement game rules and state transitions, and repositories manage persistence. Widgets should not directly implement persistence operations or core game rules.

## 3. Data Models and State Management

### 3.1 LIFE State

`LifeState` is the central model containing the information required to restore an individual LIFE and continue its life cycle.

| Data category | Primary responsibility |
|---|---|
| Identity | Individual identifier, name, current character identifier |
| Time | Birth timestamp, last update timestamp |
| Life state | Hunger, relationship, caregiving history, lifespan, life status |
| Growth | Growth stage, evolution branch, pending PATCH |
| Environment | JUNK DATA, DATA ERROR, and related conditions |
| Inventory and inheritance | Digi Pellets and generation-related information |

Specific field names and data types are determined by the implementation.

### 3.2 Individual Identity vs. Character Form

The identifier of an individual LIFE and the identifier of its currently displayed character form serve different purposes.

```text
Same LIFE (individual identity preserved)
    PIX
     ↓ Growth PATCH
    Juvenile form: BLAZE / FLOW / BLOOM
     ↓ Maturation
    Adult form of the same branch
```

Growth is modeled as a change to an existing individual's form and profile, not as the creation of a new individual. State that should remain continuous—including its name, birth timestamp, and relationship—is preserved.

### 3.3 Derived Values

Values such as `Age`, `DAY`, `SATIETY`, and the display-oriented `CONDITION` are generally derived from persisted state rather than stored redundantly when they can be calculated. This reduces the risk of inconsistencies between multiple saved values.

## 4. Time Management and Offline Progression

### 4.1 Abstracting Time Retrieval

Time-dependent logic uses an abstraction such as `TimeService` rather than depending directly on the system clock.

```dart
abstract class TimeService {
  DateTime now();
}
```

The production implementation provides the actual time, while tests can supply controlled timestamps. This supports verification of long periods of inactivity and boundary conditions.

### 4.2 Processing Time While the App Is Closed

In ViBilet, time continues to pass for LIFE even when the application is not running. The design does not require a continuously executing background game loop. Instead, it calculates elapsed time from the last recorded update and applies the necessary state changes.

```text
Persisted Last Update Time
             ↓
       Retrieve Current Time
             ↓
       Calculate Elapsed Time
             ↓
     Update Time-Dependent State
             ↓
          Persist State
```

Updates may affect hunger, relationships, environmental conditions, growth events, and lifespan. Update ordering and boundary conditions are handled by the game logic.

### 4.3 Birth-Based Time Displays

`Age` and the `DAY` indicator on HOME are calculated from the same birth timestamp. A calendar date change alone does not trigger growth; actual elapsed time is used. The birth timestamp remains unchanged after evolution, preserving continuity across forms.

## 5. Game Logic and Simulation

Game rules are separated into services according to responsibility.

| Domain | Primary responsibility |
|---|---|
| Hunger / Feeding | Update hunger and apply feeding effects |
| Bond | Represent the current relationship between LIFE and the user |
| Care | Evaluate caregiving over the individual's lifetime |
| JUNK DATA | Manage accumulation, cleanup, and interactions with abnormal conditions |
| Behavior | Determine activity states and select behavior |
| LifeSimulation | Coordinate multiple state updates based on elapsed time |

`LifeSimulation` coordinates time-driven processing rather than placing every rule inside a single large Widget. The game logic is responsible for producing consistent results when states have different priorities or updates interact.

Displayed conditions are generally derived from models or service results. For example, `CONDITION` is presentation information derived from existing life states, not an additional independent health parameter.

## 6. Growth and Life Cycle Management

### 6.1 Growth PATCH

Growth events are managed using elapsed time, the LIFE's state, and pending PATCH information. The evolution branches are `BLAZE`, `FLOW`, and `BLOOM`.

```text
PIX
 ↓
Evaluate Growth Event
 ↓
Select and Apply PATCH
 ↓
Juvenile Form of Selected Branch
 ↓
Evaluate Maturation Event
 ↓
Adult Form of the Same Branch
```

The UI handles PATCH screens and visual sequences, while game logic determines eligibility and performs state changes. Pending events are persisted so that they can be restored after the application closes.

### 6.2 End of Life

Each LIFE has a finite lifespan and may also reach the end of its life through unmet survival requirements. End-of-life decisions belong to life cycle management and are kept separate from screen transitions and visual effects.

### 6.3 MEMORY DATA and Generational Inheritance

Information inherited across generations is managed separately from the current individual's state. When records from a previous generation are used for the next, the new LIFE starts with a distinct identity and birth timestamp rather than being treated as the same individual. Detailed generation and inheritance conditions are omitted from this public edition.

## 7. Data Persistence and Restoration

Saving and loading are centralized in repositories, limiting the game logic's dependence on any particular storage mechanism.

Persisted data primarily includes identity, life state, birth and update timestamps, growth and PATCH state, environmental conditions, inventory, and generation-related records. Reconstructible display values and temporary animation state are excluded unless there is a specific reason to persist them.

On startup or resumption, the conceptual restoration sequence is:

```text
App Startup / Resume
         ↓
Load Persisted State
         ↓
Apply Elapsed-Time Updates
         ↓
Persist Updated State
         ↓
Check Life Cycle and Growth Events
         ↓
Display the Appropriate Screen
```

Pending PATCH events and end-of-life transitions may take priority over the ordinary HOME screen. Clear boundaries between state updates and persistence help prevent duplicate processing or repeated application of an event.

## 8. Character Rendering and Asset Management

Shared character-rendering logic is separated from form-specific definitions and animations. Profiles such as `CharacterProfile` provide species information, activity rhythms, personality, descriptive text, and display-related configuration.

The rendering layer selects assets according to the current character identifier and state, and plays motions such as idle, movement, eating, touch reactions, and sleeping. Common frame and playback configuration reduces duplicated control logic across forms.

Pixel-art rendering must avoid unintended interpolation that blurs sprite edges. Visual corruption caused by JUNK DATA is primarily represented through presentation layers or asset selection rather than by modifying persisted character data.

Compatibility when adding characters and fallback rendering when assets are missing are also considered as part of extensibility and reliability.

## 9. Separation of UI and Game Logic

Temporary visual state is distinguished from persistent life state.

| UI responsibilities | Game logic responsibilities |
|---|---|
| On-screen position and facing direction | Individual identity and life status |
| Animation frames | Hunger, Bond, and Care |
| Tap and transition effects | Lifetime and growth stage |
| Noise, particles, and visual corruption | JUNK DATA and DATA ERROR |
| Temporary navigation state | Pending PATCH state |

For example, an animation in which LIFE approaches the user after a tap is separate from the relationship change caused by a `TOUCH` interaction. Similarly, removing visual noise from the screen and updating the persisted JUNK DATA value are distinct responsibilities.

This separation reduces the effect of visual changes on game rules and makes test boundaries easier to define.

## 10. Testability and Quality Design

ViBilet emphasizes separating logic into units that can be automatically tested, especially for elapsed-time processing and state transitions.

Key verification areas include:

- **Time boundaries:** Inject controlled timestamps to test day transitions and long elapsed periods.
- **State updates:** Verify changes caused by feeding, relationships, care, and environmental conditions.
- **Growth events:** Verify event triggering, PATCH application, and restoration of pending events.
- **Life cycle:** Verify transitions for natural end of life, survival failure, and the start of a new generation.
- **Persistence:** Verify restoration after restart, prevention of duplicate updates, and consistency with derived values.
- **Presentation:** Verify asset selection, JUNK DATA visual effects, and interactions with user input.

Service unit tests, Widget tests, and integration tests are used according to the responsibilities under test. Device-specific behavior, notifications, rendering quality, and accessibility require additional verification under the separate test plan.

---

## Design Summary

ViBilet's technical design centers on **maintaining a consistent identity and state for a LIFE that changes with real-world time, across application restarts and changes in form**.

By combining time abstraction, separation of persisted and derived state, responsibility-specific services, repository-based persistence, and shared character rendering, the architecture aims to support ongoing feature development and reliable verification.

*This document is a public technical design overview for portfolio purposes. Detailed internal specifications and unresolved development decisions are maintained in private design documents.*
