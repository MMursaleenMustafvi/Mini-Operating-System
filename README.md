# Mini-Operating-System
This Mini OS simulates multitasking with RAM/HDD management, task scheduling, core handling via semaphores, and kernel/user modes. It supports launching, minimizing, switching, and closing tasks, with real-time memory and CPU core tracking.

Here's a **comprehensive yet concise explanation** of your **Mini Operating System Simulator in C++**, explaining its core components, logic, and each key function and class. I've broken this down into logical sections:

---

## 🧠 **Overview**

This project simulates a **basic multitasking operating system** in C++ using:

* **Process control** (`fork`, `execl`, `kill`, `wait`)
* **Memory management**
* **Multithreading and CPU core simulation**
* **Task scheduling** with **ready queues**
* **Semaphore-based concurrency control**
* **Kernel Mode** interaction
* **Simulated GUI Applications** like Notepad, Calculator, etc.

---

## 📦 **Core Components**

### 1. **MemoryManager Class**

Handles allocation and deallocation of RAM and HDD.

* `allocateMemory(int ramNeeded, int hddNeeded)`
  Allocates memory for a task if sufficient resources are available.

* `freeMemory(int ramFreed, int hddFreed)`
  Frees up memory used by a task when it exits.

* `getFreeRAM()` / `getFreeHDD()`
  Returns available RAM and HDD.

* `displayMemoryStatus()`
  Prints the current memory usage (RAM + HDD).

---

### 2. **Task Class**

Represents a process (task) running in the OS.

* Stores **task name**, **RAM & HDD usage**, **PID**, and **state** (`Running`, `Minimized`).

* `getTaskName()`, `getRamNeeded()`, `getHddNeeded()`, `getState()`, `getPid()`
  Accessors for task info.

* `setState(string newState)`
  Used to switch task between "Running" and "Minimized".

---

### 3. **OperatingSystem Class**

Simulates the OS managing CPU cores, memory, tasks, and the scheduling logic.

#### 🔧 Constructor

* Takes OS name, RAM, HDD, and core count.
* Initializes memory and semaphores.

#### 🔁 `boot()`

* Displays system info and starts **core worker threads**.

#### 🧵 `coreWorker(int coreId)`

* Simulates CPU cores.
* Waits for tasks from the `taskQueue` and executes them (simulated by sleep).

---

### 🚀 Task Management Functions

#### ✅ `startTask(...)`

* Launches a task if:

  * Enough **RAM & HDD** available.
  * A **CPU core** is free.

* If no core is available → task goes to the **ready queue**.

* Uses `fork()` and `execl()` to run executable tasks via WSL (`wsl.exe`).

* In **parent process**, presents:

  * **Minimize task** (SIGSTOP)
  * **Exit task** (SIGKILL + free memory)
  * **Switch task** (minimizes current and resumes selected task)

---

### 🔄 `switchTask()`

* Allows switching to another task manually.
* Pauses the current running task, resumes the selected one.

---

### ❌ `closeTask(string taskName)`

* Closes a task by name.
* Frees up resources and CPU core.
* Starts next task from the **ready queue** if available.

---

### 📋 `showRunningTasks()`

* Displays all currently running tasks.

---

### 💾 `showMemoryStatus()`

* Calls `MemoryManager::displayMemoryStatus()`.

---

### 🛠 `userToKernelMode()`

* Switches to **Kernel Mode**:

  * Displays **running and ready tasks**
  * Allows deleting tasks from the ready queue
  * Shows **processor usage stats**

---

### 🧩 `main()`

* Initializes OS with user input: **RAM, HDD, cores**
* Displays a **menu-based UI** for:

  * Starting apps (Notepad, Calculator, etc.)
  * Viewing system info
  * Managing tasks
  * Switching to kernel mode
  * Exiting the simulator

---

## 🧰 Technologies Used

| Feature          | Tool / API                                  |
| ---------------- | ------------------------------------------- |
| Process Handling | `fork`, `execl`, `kill`, `waitpid`          |
| Threads          | `std::thread`                               |
| Concurrency      | `semaphores`, `mutex`, `condition_variable` |
| CLI UI           | ANSI Escape Sequences                       |
| Task I/O         | Executables via WSL (`wsl.exe`)             |

---

## 🧠 Example Usage

```bash
./MiniOS 2 10 4
# 2 GB RAM, 10 GB HDD, 4 CPU cores
```

---

## 📦 Applications Simulated

You simulate 14+ tasks that act like real apps:

* Notepad
* Calculator
* GPA Calculator
* File Manager
* Minesweeper
* Voting System
* Tower of Hanoi
* Expense Tracker
* Calendar, Watch, etc.



