# 🎮 Dodge the Fall  [![Download](https://img.shields.io/badge/Download-Play%20Now-brightgreen)](https://github.com/TUSHARKADAM2005 /dodge-the-fall/releases/tag/v1.0)  > Windows | No install needed | Unzip and play


# 🎮 Dodge the Fall — 2D Unity Game

A fast-paced 2D arcade survival game built in Unity where you dodge falling enemies for as long as possible and chase a high score.

---

## 📋 Table of Contents

- [Game Overview](#game-overview)
- [Gameplay](#gameplay)
- [Controls](#controls)
- [Game Features](#game-features)
- [Project Structure](#project-structure)
- [Scripts Reference](#scripts-reference)
- [Scene Setup Guide](#scene-setup-guide)
- [Inspector Configuration](#inspector-configuration)
- [How to Run](#how-to-run)
- [Known Limitations](#known-limitations)

---

## 🕹️ Game Overview

Dodge the Fall is a minimalist 2D survival game. Enemies spawn from the top of the screen and fall downward. Your goal is to survive as long as possible by moving your player left and right to avoid them. Every second you stay alive, your score increases. When an enemy hits you, a sound plays, the screen shows Game Over, and you can restart instantly.

---

## 🎯 Gameplay

- Enemies continuously spawn from random horizontal positions at the top of the screen
- All enemies fall downward at a constant speed
- The player earns **+1 score every second** they are alive
- A collision with any enemy triggers **Game Over**
- On Game Over, all enemies freeze in place and scoring stops
- Press **R** to restart the scene instantly

---

## 🎮 Controls

| Key | Action |
|-----|--------|
| ← Left Arrow | Move player left |
| → Right Arrow | Move player right |
| R | Restart game (after Game Over only) |

### Screen Wrapping

If the player moves off the left edge of the screen, they reappear on the right edge — and vice versa. This allows continuous movement in either direction without getting stuck at the edges.

---

## ✨ Game Features

- **Survival scoring** — score increments by 1 every second the player is alive
- **Infinite enemy spawning** — enemies spawn at regular intervals from random X positions
- **Screen wrapping** — player wraps around screen edges seamlessly
- **Hit sound effect** — an audio clip plays on collision with an enemy
- **Game Over UI** — a text overlay appears on death with restart instructions
- **Auto enemy cleanup** — enemies that fall below the screen are automatically destroyed to save memory

---

## 📁 Project Structure

```
Assets/
├── Scripts/
│   ├── Player.cs          # Player movement, enemy spawning, collision, game over logic
│   └── ScoreScript.cs     # Score tracking and UI update every second
├── Prefabs/
│   └── Enemy              # Enemy prefab (requires Collider2D set as Trigger)
├── Audio/
│   └── HitSound           # Audio clip played on enemy collision
└── Scenes/
    └── SampleScene        # Main game scene
```

---

## 📜 Scripts Reference

### `Player.cs`

Attached to the **Player** GameObject. Handles everything about the player and game state.

| Field | Type | Description |
|-------|------|-------------|
| `speed` | float | Player movement speed (default: 5) |
| `spawnInterval` | float | Seconds between each enemy spawn (default: 2) |
| `enemyFallSpeed` | float | Speed at which enemies fall (default: 2) |
| `enemyPrefab` | GameObject | The enemy prefab to spawn — **must be assigned in Inspector** |
| `hitSound` | AudioClip | Sound played when player is hit — **must be assigned in Inspector** |
| `audioSource` | AudioSource | AudioSource component on Player — **must be assigned in Inspector** |
| `gameOverText` | TextMeshProUGUI | The Game Over UI text — **must be assigned in Inspector** |

**Key methods:**

- `MovePlayer()` — reads arrow key input and moves the player, handles screen wrap
- `SpawnEnemy()` — called repeatedly via `InvokeRepeating`, instantiates enemies at random top positions
- `OnTriggerEnter2D()` — detects enemy collision, plays hit sound, triggers game over state

---

### `ScoreScript.cs`

Attached to a **Score** GameObject in the scene. Handles score display.

| Field | Type | Description |
|-------|------|-------------|
| `score` | int | Current score value |
| `scoreText` | TextMeshProUGUI | The score UI text element — **must be assigned in Inspector** |

**Key methods:**

- `AddScore()` — called every second via `InvokeRepeating`, increments score and updates UI
- `stopScoring()` — called by `Player.cs` on Game Over to cancel the repeating invoke

---

### `EnemyMover` (inside `Player.cs`)

A component dynamically added to each spawned enemy. No Inspector setup needed.

- Moves the enemy downward every frame using `Vector2.down`
- Destroys the GameObject when it falls below the visible screen area

---

## 🏗️ Scene Setup Guide

Follow these steps to set up the scene from scratch:

### 1. Player Object
- Create a Circle 2D sprite and name it `Player`
- Add the `Player.cs` script
- Add a `CircleCollider2D` → check **Is Trigger**
- Add an `AudioSource` component (uncheck Play On Awake)
- Tag it `Player` if needed

### 2. Enemy Prefab
- Create a sprite (any shape) and name it `Enemy`
- Add a `Collider2D` → check **Is Trigger**
- Tag it `Enemy`
- Save it as a Prefab in the Prefabs folder

### 3. Canvas UI
- Create a UI Canvas (`GameObject → UI → Canvas`)
- Add a **TextMeshPro** text for the score display (top of screen)
- Add a **TextMeshPro** text for Game Over (center of screen, larger font)

### 4. Score Object
- Create an empty GameObject named `ScoreManager`
- Attach `ScoreScript.cs` to it
- Assign the score TextMeshPro element in the Inspector

### 5. Camera
- Ensure the Main Camera is set to **Orthographic** (required for screen width calculations)

---

## 🔧 Inspector Configuration

After scene setup, select the **Player** object and assign the following in the Inspector:

| Slot | Assign |
|------|--------|
| Enemy Prefab | Your enemy prefab from the Prefabs folder |
| Hit Sound | Your audio clip from the Audio folder |
| Audio Source | The AudioSource component on the Player object |
| Game Over Text | The Game Over TextMeshPro UI element |

Select the **ScoreManager** object and assign:

| Slot | Assign |
|------|--------|
| Score Text | The score TextMeshPro UI element |

---

## ▶️ How to Run

1. Open the project in **Unity 2021.3 or later**
2. Open `Assets/Scenes/SampleScene`
3. Make sure **Active Input Handling** is set to **Input Manager (Old)**
   - Go to: Edit → Project Settings → Player → Other Settings → Active Input Handling → `Input Manager (Old)` or `Both`
4. Press **Play** in the Unity Editor
5. Use **Left / Right Arrow Keys** to move
6. Survive as long as possible and beat your score!

---

## ⚠️ Known Limitations

- Enemy fall speed does not increase over time — difficulty stays constant
- No persistent high score between sessions
- Only keyboard input is supported (no mobile/touch controls)
- All enemies share the same fall speed set at spawn time

---

## 🛠️ Built With

- **Unity** (2D, Orthographic Camera)
- **C#**
- **TextMeshPro** for UI text
- **Unity Audio System** for hit sound
