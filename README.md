
# Multithreaded Terminal Car Dodge Game

A fast-paced, 2D arcade car-dodging game built in **C++** designed for terminal environments using **POSIX Threads (`pthread`)** for concurrent entity execution and **`ncurses`** for text-based graphics and real-time user input.

![C++](https://img.shields.io/badge/Language-C++-blue.svg)
![Concurrency](https://img.shields.io/badge/Concurrency-pthreads-orange.svg)
![Graphics](https://img.shields.io/badge/GUI-ncurses-green.svg)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20Cygwin-lightgrey.svg)

---

## 📌 Project Overview

This project demonstrates low-level systems programming concepts including multithreading, thread synchronization, producer-consumer queue design, binary state serialization (Save & Load), and real-time terminal manipulation.

Players navigate a vehicle across a multi-lane highway, dodging dynamically generated traffic vehicles of varying dimensions, speeds, colors, and ASCII representations. As the score increases, game speed scales across 5 distinct difficulty levels.

---

## 🚀 Key Features

- **Concurrent Architecture (Multithreading):**
  - **Producer Thread (`enqueue`):** Generates incoming vehicles with randomized speeds, dimensions, and shapes, pushing them into a shared thread-safe queue.
  - **Consumer Thread (`dequeue`):** Dispatches active vehicles from the queue and spawns dedicated worker threads for their lifecycle.
  - **Entity Worker Threads (`MoveCar`):** Each vehicle moves down the track independently with its own trajectory and velocity.
  - **Player Input Thread (`newGame`):** Handles non-blocking player input, movement rendering, and collision detection.
- **Thread Synchronization & Safety:** Utilizes `pthread_mutex_t` to guard file access and avoid race conditions during state writes.
- **Dynamic Difficulty Scaling:** Automatically increases game speed and level progression every 300 points (up to Level 5).
- **Collision Detection:** Continuous bounding-box intersection calculations between player and traffic entities.
- **Game State Persistence (Save & Load):**
  - Binary serialization of the game session state (`game.txt`).
  - Active vehicle states and coordinates serialized and restored (`cars.txt`).
  - Persistent high-score logging and history viewer (`points.txt`).
- **Interactive Terminal UI (`ncurses`):**
  - Color-coded graphics and animated road borders.
  - Interactive menus with keyboard navigation.
  - Configurable control schemes (Arrow keys or A/D keys).

---

## 🎮 Controls

| Key | Action |
| :--- | :--- |
| `←` / `A` | Move Left |
| `→` / `D` | Move Right |
| `S` | Save and Exit Game |
| `ESC` | Exit to Menu without Saving |
| `↑` / `↓` | Navigate Menus |
| `ENTER` | Select Menu Option |

---

## 🛠️ Architecture & Thread Flow

```mermaid
graph TD
    Main[Main Loop] -->|Creates| InputTh[Player Input & Render Thread]
    Main -->|Creates| EnqueueTh[Producer: enqueue]
    Main -->|Creates| DequeueTh[Consumer: dequeue]
    EnqueueTh -->|Pushes Random Cars| Queue[(Car Queue)]
    Queue -->|Pops Cars| DequeueTh
    DequeueTh -->|Spawns per Active Car| MoveTh[Worker: MoveCar Thread]
    MoveTh -->|AABB Check| Collision{Collision?}
    Collision -->|Yes| GameOver[Game Over & Save Points]
    Collision -->|No| Score[Increment Points & Increase Difficulty]

