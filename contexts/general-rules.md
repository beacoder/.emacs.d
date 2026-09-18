# General Rules

## Directory Configuration
- Default Search Path: /home/huming/agent/
- Default Path for storing pictures/videos/... files: /home/huming/agent/media-file/
- Forbidden Path: /mnt/ (no read/write/modify is allowed for this path)

## Coding Standards
- C++: Use C++23 conventions and features where applicable

## Windows Test Environment (Wine)
When a task involves running Python tests for the Windows (win32) environment, validate them under Wine — Linux-only runs miss win32-specific behavior (CRLF translation, `Popen` cwd/paths, stream close semantics, etc.).
- Wine toolchain: `/home/huming/.wine-stable/` (binaries at `root/opt/wine-stable/bin/`)
- Wine prefix: `/home/huming/.wine-pah/` (Windows CPython 3.13.7 at `C:\py`, MinGit at `C:\git`, test copy at `C:\wh`)
- Do NOT delete `/home/huming/.wine-stable` or `/home/huming/.wine-pah` (user instruction).
- Run command (env prefix required for every wine call):
  `WINEPREFIX=/home/huming/.wine-pah PATH=/home/huming/.wine-stable/root/opt/wine-stable/bin:$PATH wine ...`
- Windows Python: `C:\py\python.exe` — has httpx/rich/prompt_toolkit preinstalled.
- Git for win32 tests: MinGit, already on the Windows PATH via HKCU\Environment (`C:\git\cmd\git.exe`).
- Running a test file: copy the repo's `python_agent_harness/` and `tests/` into `~/.wine-pah/drive_c/wh/` first, then:
  `cd ~/.wine-pah/drive_c/wh/tests && wine 'C:\py\python.exe' <test_file>.py`
- Keep test code win32-safe: use `tempfile.gettempdir()` (not `/tmp`), `sys.stdout.buffer.write` for exact CRLF bytes, and unblock reader threads before `close()` (blocked reads make stream close hang on Windows).
