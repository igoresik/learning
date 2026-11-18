<sub><a href="../README.md">Back to the notebook</a></sub>

# Running model-written code in a Windows jail instead of trusting a blocklist

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-security-dark.svg"><img src="../assets/tag-security.svg" alt="Security" height="22"></picture></a>&nbsp; **Date:** 2026-11-18

## What I built or changed

- Worked on Schema Engine, my public Python tool where a language model writes a pandas transform that runs in a separate process.
- A review of the project showed that the check before running (a blocklist of names) could be bypassed: code that reached the `os` module through a pandas attribute passed the check and could read the working folder, and the same route reaches deleting files and starting programs.
- Instead of making the blocklist longer, I added a jail underneath it, only on Windows. The child process now runs with a restricted, Low-integrity token, so Windows refuses its writes and deletes outside its own work folder. A Job Object stops it starting other programs, caps its memory and ends it if the app closes.
- Wrote tests that run escape code the check lets through on purpose: a file delete, a file write and a program start. I turned the jail off to confirm each test fails without it and passes with it.
- Updated the README to say exactly what the jail does and what it doesn't (reading files and network connections are still possible), and added a switch to turn it off.
- Also made the CLI refuse an output or transform file that is the input file, and give a plain message when a file can't be opened.

## What I learned

- A blocklist inside the same process is a speed bump. A boundary has to be enforced by the operating system, so code that gets past the check still can't do damage.
- On Windows the pieces are a restricted token with a Low integrity level (writes up the integrity ladder are refused) and a Job Object (limits on processes, memory and the desktop). I called them from Python with `ctypes`.
- Low integrity needs a folder it is allowed to write in. `icacls /setintegritylevel` marks one folder, and files created in it afterwards inherit that.
- A virtual environment's `python.exe` is a launcher that starts the real interpreter, so a "one process only" limit has to be two there, or Python can't start at all.
- After killing a job on a timeout, wait for the process to exit before deleting its folder, or Windows reports the file as still in use.
- A security test only counts if it fails without the fix. Two of my first three tests passed with the jail off, because the check rejected the code for another reason, so I rewrote them.
- Be honest in the docs about what is not covered. Reading files and network access are still open, and other operating systems have no jail.
- A project installed in editable mode points at one checkout. Tests run from a second working copy need `PYTHONPATH=src`, or they silently test the other code.

## Tools used

- Python, `ctypes` (Windows Job Objects, restricted tokens, `CreateProcessAsUser`)
- `icacls`
- pytest
- Git worktrees and GitHub pull requests

## Link to the project repo

- [igoresik/schema-engine](https://github.com/igoresik/schema-engine)
