# ViBilet — UI and Screen Flow Specification (Public Edition)

> This document provides a public overview of the UI design principles, user experience, major screen responsibilities, and navigation flows for **initial version of ViBilet**. It is not a complete implementation specification. Features and visual effects still under development may change.

## 1. Document Overview

This document describes the UI/UX design principles, key screens, navigation flows, and separation of responsibilities between the UI and game logic in **ViBilet**, a digital life simulation and raising app.

It summarizes the internal detailed screen specification. It intentionally omits exhaustive interaction conditions, numeric thresholds, animation frame details, and unresolved implementation decisions.

###1.1 Related documents
- `03_GAME_SYSTEM.md`: Game system specification
- `05_TECHNICAL`: Technical specification

## 2. Design Principles

### 2.1 Put LIFE at the Center

HOME is designed not as an action menu, but as **a place where LIFE lives autonomously within a digital environment**. Users observe LIFE's movements and responses, approaching and interacting when needed.

- Use a dark digital environment, grids, and data particles as core visual elements.
- Place the pixel-art LIFE and its behavior at the center of the screen.
- Keep persistent heads-up display (HUD) elements minimal, and group detailed information on a separate screen.
- Communicate status through appearance, behavior, noise, and visual changes—not only numbers.

### 2.2 Experience Growth as the Same LIFE

Growth is presented not as replacing one character with another, but as **the same LIFE acquiring a new form**. Its name, elapsed age, and continuity of care are preserved.

```text
EGG → PIX → JUVENILE → ADULT
              │
              └─ BLAZE / FLOW / BLOOM
```

The selection of a growth branch and the PATCH sequence are designed to fit the world of digital lifeforms.

## 3. Main Screens

| Screen | Purpose | Primary Experience |
|---|---|---|
| STARTUP | Establish a connection with LIFE | Connection and data-loading sequences |
| NEW LIFE / EGG | Introduce a new life | Begin raising a LIFE from a common EGG |
| HATCH | Present the birth of LIFE | PIX emerges from the EGG |
| NAMING | Name the individual LIFE | Name the LIFE while seeing the newly born character |
| HOME | Observe LIFE's daily existence | Autonomous behavior, elapsed days, and visual status cues |
| INTERACTION | Interact with LIFE | FOOD, TOUCH, and PLAY (details in development) |
| LIFE DATA | View individual information | Name, age, condition, relationship, and more |
| PATCH | Mark a growth milestone | Branch selection, confirmation, and transformation |
| LIFE END / MEMORY DATA | Handle the end of life and the next generation | End-of-life sequence, records, and transition to a new LIFE |

Some screens and visual sequences remain in design or production. Their inclusion does not mean they are all complete.

## 4. Overall Navigation Flow

The following diagram illustrates a representative user journey. Exceptional recovery flows and detailed conditional branches are omitted.

```text
STARTUP
   │
   ├─ EXISTING LIFE ────────────────────┐
   │                                   │
   └─ NEW LIFE → EGG → HATCH → NAMING  │
                                       ▼
                                      HOME
                                    /      \
                                   ▼        ▼
                             INTERACTION  LIFE DATA
                                   \        /
                                    └── HOME
                                         │
                                  Growth PATCH
                                         │
                              JUVENILE / ADULT
                                         │
                                        HOME
                                         │
                                      LIFE END
                                         │
                                     MEMORY DATA
                                         │
                                      NEW LIFE
```

On startup, the app checks the saved LIFE state and directs the user to the appropriate experience, such as HOME, a pending growth event, or end-of-life processing.

## 5. Core Screen UX

### 5.1 HOME — A Space for Observation

On HOME, LIFE may walk, pause, or sleep. Rather than constantly demanding input, the screen is designed to let users observe its daily existence.

- **DAY**: Briefly indicates the number of days elapsed since birth.
- **DATA**: Opens LIFE DATA.
- **Tap LIFE**: Initiates the transition to the interaction screen.
- **JUNK DATA**: Visually communicates the condition of the digital environment.

Detailed care parameters and remaining lifetime are not persistently displayed on HOME.

### 5.2 INTERACTION — Getting Closer to LIFE

Tapping LIFE on HOME initiates a transition in which LIFE appears to approach the user before entering the interaction screen. The goal is not merely to change screens, but to create **the feeling that LIFE has responded to the user**.

Core interactions are FOOD, TOUCH, and PLAY. LIFE responds to these actions, which can affect its relationship and care state. The specific PLAY experience is still under development.

### 5.3 LIFE DATA — Information When Needed

LIFE DATA brings together the individual's profile and current condition.

| Field | Description |
|---|---|
| NAME / SPECIES | Name and current form |
| AGE | Time elapsed since birth |
| LIFE RHYTHM / PERSONALITY | Daily rhythm and personality |
| CONDITION | Current condition |
| BOND / SATIETY | Relationship and satiety |
| MEMO | A short description of LIFE's characteristics |

Rather than exposing every internal parameter numerically, the interface translates them into meaningful information for the user. Remaining lifetime is not shown directly.

## 6. Lifecycle Experience Design

### 6.1 Birth

PIX is born from a common EGG, and the user gives it a name. The newly born LIFE remains visible on the naming screen to emphasize **the experience of naming the individual the user has just met**.

### 6.2 Growth and PATCH

LIFE grows in response to elapsed time and the care it receives. A PATCH represents the application of new data to a digital lifeform, allowing it to acquire a new appearance.

The growth branches are **BLAZE / FLOW / BLOOM**. LIFE retains its identity and continuity after transformation.

### 6.3 End of Life and the Next Generation

LIFE has a finite lifespan, and the end of its life is part of the raising experience. Data decomposition and MEMORY DATA sequences represent the individual's history and its connection to a future generation.

Inheritance is not intended to recreate the previous LIFE exactly. Instead, the goal is to **carry traces of its individuality into the next LIFE**.

## 7. Visual Communication of State

ViBilet does not rely on numbers alone to explain LIFE's condition.

- **Behavior and expressions**: Communicate hunger, drowsiness, affection, and other states through responses.
- **JUNK DATA**: Visualize unwanted data as objects within the digital environment.
- **Visual corruption**: Gradually introduce noise, garbled text, and distorted character imagery as contamination accumulates.
- **PATCH sequences**: Present growth as data transformation and reconstruction.

Visual corruption is, in principle, **a presentation effect** and does not damage the individual's saved information. Essential navigation and controls remain readable and usable.

## 8. Separation of UI and Game Logic Responsibilities

Temporary on-screen presentation is distinguished from LIFE's persistent state.

| UI and Presentation | Game State |
|---|---|
| On-screen position, direction, animation frames | Hunger, Bond, Care |
| Temporary responses to taps | Lifetime, Age, Life Status |
| Noise, particles, screen transition effects | JUNK DATA, DATA ERROR |
| Scaling and display controls | Growth Stage, PATCH State, MEMORY DATA |

Displayed information such as DAY is generally derived from game state. The architecture aims to allow changes to visual presentation without unnecessarily affecting LIFE's internal state or game rules.

---

## 9. About This Public Edition

This public edition introduces the fundamental UI/UX design. Detailed navigation conditions, action-specific thresholds, animation frame specifications, implementation priorities, and unresolved decisions are maintained in internal development documents.

Some elements described here are planned or still in production and are not guaranteed to appear unchanged in the released version.
