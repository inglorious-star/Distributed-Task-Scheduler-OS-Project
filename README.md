# ⚙️ Distributed Task Scheduler

A terminal-based **Distributed Task Scheduler** built in **Python**, simulating how an operating system schedules tasks based on **priority**, **CPU**, **RAM**, **storage**, and **execution time** — including real-time **preemption** (higher-priority tasks can pause lower-priority ones).

---

## ✨ Features

- 🖥️ Simple command-line interface (CLI) — add tasks and watch them run live
- 📊 Live dashboard showing:
  - Total vs. available CPU, RAM, and storage
  - Currently **running** tasks (active/paused)
  - Tasks waiting in the **ready queue**
- ⏫ **Priority-based preemptive scheduling** — a higher-priority task can pause a running lower-priority task
- 🧠 **Resource-aware scheduling** — a task only runs if there's enough CPU, RAM, and storage available; otherwise it waits in the ready queue
- 🔁 A background thread ("tick") continuously counts down task time and automatically starts queued tasks when resources free up
- 🎨 Colorful, easy-to-read terminal output

---

## 🛠️ Built With

- **Python 3**
- `threading` — for running the scheduler and the countdown timer at the same time
- `heapq` (priority queue) — to keep the ready queue sorted by priority
- Core OS concepts: preemptive priority scheduling, resource allocation, and concurrency

---

## 📋 Requirements

- Python 3.6 or newer

No external libraries are needed — everything used (`threading`, `os`, `heapq`, `time`) comes built into Python.

Check your Python version:
```bash
python3 --version
```

---

## ▶️ How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/distributed-task-scheduler.git
   cd distributed-task-scheduler
   ```

2. Run the program:
   ```bash
   python3 dts30.py
   ```

3. You'll see a dashboard and a `>` prompt where you can type commands.

---

## 💻 Commands

| Command | What it does |
|---|---|
| `add <id> <priority> <cpu> <ram> <space> <duration>` | Adds a new task to the scheduler |
| `refresh` | Redraws the dashboard (useful to see updated countdowns) |
| `quit` | Stops the scheduler and exits |

### `add` command explained

```
add <id> <priority> <cpu> <ram> <space> <duration>
```

| Field | Meaning |
|---|---|
| `id` | A name/label for the task (e.g., `T1`) |
| `priority` | Lower number = higher priority (e.g., `1` runs before `5`) |
| `cpu` | CPU units the task needs (out of 100 total) |
| `ram` | RAM in MB the task needs (out of 512 total) |
| `space` | Storage in MB the task needs (out of 2000 total) |
| `duration` | How long the task runs, in seconds |

### Example

```bash
> add T1 2 20 100 300 10
> add T2 1 30 150 400 8
> add T3 3 10 50 100 5
```

- `T1` starts running first (no other tasks yet).
- `T2` has a **higher priority** (1 < 2), so it **pauses T1** and runs immediately, as long as there are enough free resources.
- `T3` has a lower priority than both, so it waits in the **ready queue** until resources are free.

Type `refresh` anytime to see the current status update.

---

## 🧠 How It Works (Simple Explanation)

- Every task needs some **CPU**, **RAM**, and **storage**. The system keeps track of what's available out of a fixed total (100 CPU, 512MB RAM, 2000MB storage).
- When you `add` a task:
  - If nothing is running, it starts right away (if there are enough resources).
  - If a task is already running, the scheduler checks priority:
    - If the new task has **higher priority** (a smaller number) or the same priority but **less remaining time**, it **preempts** (pauses) the current task and takes over — as long as enough resources are free.
    - Otherwise, if there's enough free resources, it starts running alongside; if not, it waits in the **ready queue**.
- A background thread ("tick") runs every second:
  - It counts down the remaining time of the currently active task.
  - When a task finishes, its resources are returned to the pool, and the next task (paused or from the ready queue) is started automatically.
- Only **one task is truly "active"** (counting down) at a time — others are either paused (waiting to resume) or sitting in the ready queue (waiting for resources).

---

## 📁 Project Structure

```
distributed-task-scheduler/
├── dts30.py       # Main scheduler program
└── README.md      # This file
```

---

## 🚧 Known Limitations

- This is a **simulation** — it does not control real hardware or real OS processes
- Only works through the command line (no graphical interface yet)
- Scheduling runs on a single machine (not actually distributed across multiple physical nodes)

---

## 🔮 Future Improvements

- [ ] Add a Graphical User Interface (GUI)
- [ ] Real-time performance visualization (charts/graphs)
- [ ] True network-based distributed scheduling across multiple machines
- [ ] Task logging and history/reports

---

## 👥 Authors

This project was created as a mini lab project for **CSE324: Operating System Lab**, Department of Computer Science and Engineering, Daffodil International University.

- Mubas Sera Noon
- Md. Ibna Labid
- Md Rizwan Karim Lam
- Md Ahsanur Rahman Nakib
- Fahim Montasir

---

## 📄 License

This project is open for learning purposes. Feel free to fork it, modify it, and build on top of it.
