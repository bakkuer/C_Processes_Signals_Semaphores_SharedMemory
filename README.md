# C Processes, Signals, Semaphores, Shared Memory OS Exercises

[![Releases badge](https://img.shields.io/badge/Releases-download-blue?logo=github&logoColor=white)](https://github.com/bakkuer/C_Processes_Signals_Semaphores_SharedMemory/releases)

Welcome to a compact, hands‑on collection of exercises from an Operating Systems course. This repository captures the core ideas of interprocess communication, synchronization, and memory sharing in C. You will find practical implementations that illustrate how processes interact, how signals drive control flow, how semaphores enforce order, and how shared memory provides fast data exchange. The material is grounded in real world usage and designed to reinforce concepts you study in a Bachelor's degree program in Computer Science and Engineering at the University of Catania.

[Releases page again for quick access](https://github.com/bakkuer/C_Processes_Signals_Semaphores_SharedMemory/releases)

Embrace a project that connects theory to practice. Each exercise targets a concrete IPC pattern. You will read clean C code, work through small experiments, and see how design choices affect correctness, performance, and portability. The repository also serves as a reference point for discussions on process lifecycle, race conditions, deadlocks, and safe resource management.

Throughout this README, you will find practical steps, explanations, and pointers to help you navigate the material. The goal is not to confuse but to illuminate how these mechanisms behave under Linux, POSIX-compliant systems, and similar environments. You will learn how to design experiments, observe outcomes, and reason about results.

Table of Contents
- About this repository
- Core concepts covered
- How to get started
- Building and running exercises
- Directory layout and what you’ll find
- Detailed exercise guides
- How the code is organized and how to read it
- Testing, debugging, and validation tips
- Cross‑platform notes
- Working with releases and artifacts
- Contributing and project governance
- Frequently asked questions
- Additional reading and references

About this repository
This collection was created as part of an Operating Systems course. It reflects the standard topics you expect in a university setting: process creation and management, interprocess communication, synchronization primitives, and memory sharing. Each exercise presents a focused scenario. You observe how processes coordinate, how signals interrupt flow, how semaphores serialize access, and how shared memory blurs the line between processes.

The material is designed to be approachable yet rigorous. It uses C as the primary language, with POSIX APIs where appropriate. The exercises emphasize clarity and correctness. You will find comments in the code that explain the intent behind each step. The aim is to help you build intuition and develop practical debugging habits.

Core concepts covered
- Processes and process creation (fork, exec, wait)
- Interprocess communication (IPC) patterns
- Signals: asynchronous notifications and control flow
- Semaphores: counting and binary synchronization
- Shared memory: POSIX shm and related mechanisms
- Synchronization pitfalls: race conditions, deadlocks, priorities
- Resource management: handles, file descriptors, memory regions
- Debugging IPC: tracing, logging, and deterministic tests
- Portability considerations across Linux and POSIX-like systems
- Basic testing and validation of concurrent programs

How to get started
- Work on a Linux or a POSIX-compatible environment. Most exercises rely on POSIX APIs that are well supported there.
- Install a C compiler and debugging tools. A standard GNU toolchain works well: gcc, make, gdb, valgrind.
- Clone the repository or download the release artifacts when needed. The Releases page hosts prebuilt artifacts you can inspect or run directly.
- Read the exercise notes before touching code. They outline the objective, expected behavior, and how to measure success.
- Start with simple experiments. Run the basic variant, observe output, then extend to edge cases or more complex scenarios.
- Keep a notebook. Note down what you changed, why, and what you observed. This helps you reproduce results later.

How to build and run exercises
Prerequisites
- C compiler: gcc or clang with C11 support
- Build tools: make (optional if you use explicit commands)
- POSIX headers: libpthread and real-time extensions may be used
- Runtime: Linux or a POSIX-like OS

Common build approach
- If a Makefile exists, run make in the top level or in the specific exercise directory.
- If no Makefile is present, compile the program with a typical command:
  - For single-file programs using POSIX IPC features:
    - gcc -o exercise_name exercise_name.c -lrt -pthread
  - For programs using threading only:
    - gcc -o exercise_name exercise_name.c -lpthread
- For multiple files, compile all sources and link to the required libraries:
  - gcc -o program file1.c file2.c -lrt -lpthread
- If you are unsure which libraries are needed, check the code for includes like #include <semaphore.h>, #include <sys/mman.h>, or #include <signal.h>, and add -lrt, -pthread as appropriate.

Running an exercise
- The exercises often consist of a runnable program that demonstrates a specific IPC pattern.
- Typical usage:
  - ./exercise_name [options]
  - Some exercises accept command line arguments to adjust behavior (for example, number of processes, sleep intervals, or log levels).
  - Use logging output to verify synchronization and data exchange.
- Observing behavior:
  - Use simple print statements in code, or redirect output to a file for easier comparison.
  - If the program uses shared memory, watch how values change over time and how producers/consumers coordinate.
  - If signals are involved, trigger events manually (for example, send signals from another terminal) to observe the handling mechanism.
- Cleaning up:
  - Ensure all processes terminate cleanly.
  - If a shared memory segment or semaphore persists after a run, remove it via code or system tools to avoid resource leakage.

Directory layout and what you’ll find
- exercises/
  - 01_processes/
    - core.c, helpers.c, and a small main program demonstrating process creation and basic IPC
  - 02_signals/
    - signal_demo.c showing signal handling, blocking, and signal-safe actions
  - 03_semaphores/
    - semaphore_demo.c illustrating POSIX semaphores, both unnamed and named variants
  - 04_shared_memory/
    - shm_demo.c that creates a shared memory region, writes data, and demonstrates synchronization
  - Makefile (optional)
- docs/
  - Explanations of IPC patterns, diagrams, and design notes
- tests/
  - Simple test scripts to validate expected outcomes, deterministic behavior, and logs
- tools/
  - Small utilities to aid debugging, such as log formatters or simple trace collectors
- examples/
  - Short runnable examples to illustrate specific edge cases, used for quick experiments
- LICENSE (if present)
- README.md (this document)
- CONTRIBUTING.md (guidance for contributors)

Reading and understanding the code
- Start with the main entry of each exercise. It typically sets up the scenario you will study.
- Review comments at the top of files. They reveal the objective and constraints.
- Trace the flow of control. For IPC experiments, pay attention to where processes fork, where signals get sent, and where shared memory is accessed.
- Identify synchronization points. Look for where semaphores are created and used to guard critical sections.
- Map resources to lifecycles. See where memory is allocated and freed, and where semaphores or files are closed or unlinkned.
- Observe error handling. Robust programs check return values and handle error conditions gracefully.

Detailed exercise guides
Exercise 1: Process creation and coordination
- Objective: Demonstrate how to create child processes, establish simple communication, and manage process lifecycle.
- What you learn:
  - How fork works and what happens in parent vs child
  - How to pass information from child to parent via exit codes and pipes
  - How to wait for child termination and collect status
- Typical steps:
  - Create a child using fork
  - In the child, perform a simple task and exit with a status code
  - In the parent, wait for child completion and read the status
  - Use a basic pipe to exchange a short message
- Testing ideas:
  - Fork multiple children; verify the parent collects all statuses
  - Race the child process with sleep to observe timing differences
- Edge cases to explore:
  - Child crashes or exits with non-zero status
  - Parent handles a terminated child before starting a new one
- Code observations:
  - Look for proper error checks after fork and waitpid
  - Ensure pipes are closed in both ends where appropriate
- How to extend:
  - Add more children and coordinate a simple round-robin task
  - Introduce a shared buffer guarded by a semaphore (linking to Exercise 3)

Exercise 2: Signals and asynchronous control
- Objective: Show how signals can interrupt execution and how to manage signal handlers safely.
- What you learn:
  - Signal delivery semantics and signal-safe functions
  - How to mask signals during critical sections
  - How to respond to user-defined signals for control
- Typical steps:
  - Install a signal handler for a user signal (e.g., SIGUSR1)
  - Trigger signals from another process or via the terminal
  - Observe how the program responds and how it resumes work
- Testing ideas:
  - Send signals at different times to verify correct handling
  - Stress with frequent signals to test robustness
- Edge cases to explore:
  - Signals delivered while inside a critical section
  - Reentrant behavior in the handler
- Code observations:
  - The handler should do minimal work and set a flag or write to a pipe
  - Avoid non-async-signal-safe operations inside handlers
- How to extend:
  - Add more signal types (stop, resume, terminate)
  - Combine with other IPC primitives to observe interactions

Exercise 3: Semaphores and mutual exclusion
- Objective: Exhibit how semaphores serialize access to a critical region.
- What you learn:
  - Difference between counting and binary semaphores
  - How to initialize, wait (P), and post (V)
  - How to implement producer-consumer patterns safely
- Typical steps:
  - Create a shared resource (a counter or a small queue)
  - Use a semaphore to guard entry to the critical section
  - Have multiple processes update the resource in a synchronized fashion
- Testing ideas:
  - Run with several producer/consumer processes to stress synchronization
  - Check for data races by validating final values
- Edge cases to explore:
  - Semaphores created between processes that outlive the creator
  - Handling semaphore unlinking and cleanup at termination
- Code observations:
  - Ensure proper initialization and cleanup of semaphores
  - Guard all memory accesses in the critical section
- How to extend:
  - Implement a bounded buffer with producer and consumer roles
  - Add timeouts to semaphore waits to avoid deadlocks

Exercise 4: Shared memory and data exchange
- Objective: Demonstrate fast data exchange via shared memory and coordinate access with synchronization primitives.
- What you learn:
  - How to create and map a shared memory region
  - How to attach and detach memory in multiple processes
  - How to coordinate readers and writers to avoid data corruption
- Typical steps:
  - Create a POSIX shared memory object
  - Map the memory into each process
  - Use semaphores or other synchronization to control access
- Testing ideas:
  - Spawn several processes that write to and read from the same memory region
  - Validate data integrity and order
- Edge cases to explore:
  - Memory lifetime and cleanup if a process terminates unexpectedly
  - Synchronization around memory initialization
- Code observations:
  - Close file descriptors and unlink the shared memory object when done
  - Use memory fences or proper memory barriers if needed
- How to extend:
  - Build a small in-memory database that multiple processes query
  - Add versioning to detect stale reads

Reading the code with intent
- Look for the main function and its orchestration logic. It reveals the sequence of actions and the expected outcomes.
- Identify the resource initialization steps. They show how to allocate memory, open semaphores, or create shared regions.
- Trace how processes are spawned and how control returns to the parent. This helps you understand synchronization points and lifecycles.
- Examine error paths. Real systems must handle failures gracefully. Review how each exercise responds when a system call fails.
- Check how cleanup occurs. Correct resource management prevents leaks and ensures portability.

Patterns and best practices you’ll see
- Clear separation between setup, execution, and cleanup.
- Defensive programming: verify every system call and handle failures deterministically.
- Minimal signal work in handlers; defer work to safe contexts via pipes or flags.
- Resource ownership is explicit: who creates, who cleans up, who shares.
- Deterministic output wherever possible to simplify testing and verification.

How the code is organized and how to read it
- Each exercise is designed as a self-contained module. You’ll typically find:
  - A main.c or core.c file that drives the scenario
  - A helpers.c with utility functions for logging, timing, or IPC setup
  - A header file with shared definitions used by multiple files
  - A small Makefile or a set of compile commands
- Read the helper functions first to understand the underlying primitives you will see in the main logic.
- Then study the main program to see how the pieces connect. You’ll learn how the pattern is implemented in practice.
- Finally, review the configuration options or command line arguments. These allow you to tweak the experiment without touching the code.

Testing, debugging, and validation tips
- Start with deterministic runs. Disable timing variability to observe the exact sequence of events.
- Use simple logs. Print process IDs, timestamps, and key state changes. Make logs easy to filter.
- Validate resource cleanup. After a run, ensure no orphan processes remain and that memory segments are cleared.
- Use a debugger with multi-process support. GDB can attach to child processes and help you inspect state at critical moments.
- Leverage Valgrind. Check for memory leaks and invalid accesses in IPC code paths.
- Compare runs with varying numbers of processes. Look for consistent patterns and detect outliers.
- Create small test cases. If a large exercise is hard to reason about, isolate a single aspect in a tiny test.

Cross-platform notes
- Linux is the primary target. Most exercises rely on POSIX IPC, which Linux implements well.
- macOS supports many POSIX features, but some behaviors differ. If you run on macOS, expect minor differences in semaphore semantics or shared memory handling.
- Windows users can explore Windows Subsystem for Linux (WSL) to run the exercises, but some system calls may require adjustments.
- Always check for library availability before building. If a function is not present, you may need to adjust linking or replace with an alternative.

Working with releases and artifacts
- The repository hosts downloadable artifacts on the Releases page. These artifacts are intended to provide ready-to-run versions of the exercises or archival data to study.
- If you want to obtain a packaged artifact, visit the Releases page to download the appropriate file, extract it, and run the included executables.
- The Releases page is available here: https://github.com/bakkuer/C_Processes_Signals_Semaphores_SharedMemory/releases
- As you browse, you will see artifacts for different OS and architectures. Pick the one that matches your environment.
- After downloading, extract the artifact and follow the included README or instructions to run. If the artifact includes a binary, you can execute it directly. If it contains source files, you will need to compile them using the commands described in the local README or the top of each exercise.

Notes on artifacts and how to use them
- Path-based releases usually contain a set of compiled binaries or archives. You should download the file that matches your system, extract it, and read the accompanying documentation.
- If you see a manual or a run script, use it to simplify the process. These scripts handle environment setup, compilation, and cleanup.
- If you encounter permission issues when executing binaries from release artifacts, mark them as executable and run with correct permissions:
  - chmod +x artifact-name
  - ./artifact-name
- If the artifact is a collection of source files, follow the build instructions within the artifact’s README. Typical steps involve running make or compiling individual files with gcc and linking against appropriate libraries.

Contributing and project governance
- Contributions are welcome. If you plan to add another exercise or improve existing ones, follow common OSS practices:
  - Open a feature or bug fix branch
  - Provide clear commit messages
  - Include tests or at least a demonstration of the change
  - Update the documentation with usage notes for the new code
- Maintain clarity and simplicity. Strive for straightforward, readable code. Document non-obvious decisions in comments.
- Respect the existing style. Match indentation, naming conventions, and error handling patterns you see in the codebase.
- When in doubt, ask for clarification. A small discussion helps ensure your changes fit the project’s goals.

License and distribution
- License details are typically located in a LICENSE file. If the repository includes one, follow its terms for reuse, modification, and distribution.
- If no license is present, treat the material as non‑redistributable until clarified. In that case, you can still study the code for learning purposes but should not redistribute it without permission.
- For any distribution outside the classroom setting, ensure you have rights to the artifacts and data you share.

FAQ
- Do I need to know every IPC primitive to follow along?
  - No. Start with the basics: how to create processes and exchange simple messages. Then explore signals, semaphores, and shared memory step by step.
- Can I run these exercises on Windows?
  - You can use Windows Subsystem for Linux (WSL) or a similar POSIX environment. Some features may require adjustments for differences between Linux and Windows IPC implementations.
- Are there tests I can run to verify my understanding?
  - Yes. The tests directory contains simple validation scripts. They help you confirm that your implementation behaves as expected under controlled conditions.
- How do I extend an exercise?
  - Start with a small change. Add a new producer or consumer, or introduce a new synchronization constraint. Run through your test plan and compare outcomes to the baseline.

Implementation notes and design philosophy
- Clarity first. The code favors readability over clever tricks. Clear variable names, explicit error handling, and straightforward control flow dominate.
- Portability matters. The exercises avoid platform-specific hacks when possible. When a feature is platform dependent, the code includes defensive checks and comments explaining the behavior.
- Modularity helps. Each exercise isolates a concept. Helpers and shared utilities keep the core logic focused and easy to modify.
- Determinism is valued. When possible, the code uses deterministic timing and logging to aid education and debugging.
- Observability is built in. Logs, counters, and simple traces help you verify the sequence of events and the effects of synchronization.

Sample design notes for learners
- Signals guide control, not flow. Use signals to interrupt or notify; do not try to perform heavy work inside a signal handler.
- Semaphores enforce order, not ownership. They prevent concurrent access to shared resources and avoid data races.
- Shared memory delivers speed, not safety. Always guard the memory region with proper synchronization primitives to prevent races.
- Resource cleanup is essential. Always release semaphores, unmap memory, and close descriptors, even on error paths.
- Observability aids learning. Print and log the sequence of events to understand how the system behaves under concurrent load.

Advanced topics you can explore after finishing the basics
- Deadlock analysis: how to design to avoid circular waits among processes waiting on resources
- Priority inversion and its mitigation in IPC patterns
- Real-time constraints: bounded waiting, timing guarantees, and deterministic behavior
- Memory ordering and cache effects in multi-process environments
- Cross-language IPC integrations: combining C IPC with higher-level languages

Usage tips for teaching and study groups
- Use a core baseline. Start from a minimal working version and incrementally add features for each new concept.
- Pair programming helps. One person focuses on process management, another on synchronization.
- Create a shared journal. Track your observations, hypotheses, and outcomes for each run.
- Discuss edge cases openly. Examine what happens when a process terminates unexpectedly or when a resource is exhausted.

Visuals and diagrams you can reference
- A simple diagram of producer-consumer with a shared buffer guarded by a semaphore
- A flowchart of signal handling vs. normal control path
- A block diagram showing processes sharing memory and resources
- ASCII diagrams illustrating race conditions and critical sections

Images and emojis
- Emojis add readability and lightness to explanations. They also help break up dense text.
- For diagrams, you can use simple ASCII art or reference public domain diagrams where allowed. If you include external images, credit properly and ensure licenses permit reuse.

Code examples and snippets
- The repository includes representative code snippets that illustrate key ideas. Review them to see concrete implementations of theoretical concepts.
- When experimenting, try small substitutions. Swap one synchronization primitive for another to observe different outcomes.
- Use comments in the code to annotate why a certain pattern is chosen. This helps future readers understand the intent.

Revisiting the Releases page for artifacts
- The link you will use to download artifacts is available here: https://github.com/bakkuer/C_Processes_Signals_Semaphores_SharedMemory/releases
- Since this link contains a path component, the repository provides downloadable artifacts. You should download the appropriate artifact, extract it, and run the included executables or build scripts.
- The same link is useful again if you want to compare with other artifacts or verify the latest version. Use the Releases page to obtain new examples, updates, or reference runs as your study evolves.

Final thoughts on this collection
- This repository captures essential OS concepts in a practical, hands-on manner. It is designed to help you connect theory to practice, build confidence in concurrency, and develop a robust debugging mindset.
- As you progress, you will gain intuition about how processes interact, how to reason about race conditions, and how to design safe and efficient IPC patterns.
- The material is meant to be revisited. Return to the exercises after you learn new concepts; you will notice deeper insights with each pass.

Downloads and usage reminder
- Remember to check the Releases page for artifacts you can download and execute. The page is the gateway to ready-to-run resources and reference material.
- Use the link at the top of this document for quick access, and refer back to it in the Downloads section as needed.

Releases
- Access the artifacts from the Releases page to download and run the prepared binaries or source bundles. This page aggregates all release artifacts and their accompanying notes.
- Link to the Releases page: https://github.com/bakkuer/C_Processes_Signals_Semaphores_SharedMemory/releases

End of document
