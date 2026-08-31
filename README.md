# Multithreaded Terminal Traffic Dodger

A concurrent, terminal-based arcade traffic dodging game developed in **C/C++** using the **ncurses** library and **POSIX Threads (pthreads)**. The game challenges the player to steer through a multi-lane highway, dodging dynamically generated traffic while collecting points and leveling up.

---

## What It Does

* **Concurrent Game Engine:** Utilizes dedicated threads for player input, dynamic traffic production (`enqueue`), traffic dispatching (`dequeue`), and individual movement lifecycles for every on-screen vehicle (`MoveCar`).
* **Thread-Safe File I/O (Save & Load):** Implements `pthread_mutex_t` synchronization to serialize concurrent write operations, allowing players to save in-flight game sessions and traffic states to binary files (`game.txt`, `cars.txt`) and resume later.
* **Procedural Traffic & Collision Detection:** Generates vehicles of randomized dimensions, speeds, colors, and ASCII shapes (`#`, `*`, `+`) with coordinate-based collision mechanics.
* **Progression System:** Real-time score calculation based on avoided vehicle dimensions, progressive difficulty scaling (speed increases per 300 points across 5 levels), and a persistent high-score history tracker (`points.txt`).
* **Interactive Terminal UI:** Customizable control schemes (Arrow Keys vs. A/D keys), color-pair styling, and non-blocking real-time terminal rendering via `ncurses`.

---

## Tech Stack

* **Language:** C / C++
* **Concurrency:** POSIX Threads (`pthread`), Mutex Synchronization (`pthread_mutex_t`)
* **Terminal UI / Graphics:** `ncurses`
* **Data Structures:** FIFO Queue (`std::queue`)
* **Data Persistence:** Binary & Text File I/O (`fread`/`fwrite`)
