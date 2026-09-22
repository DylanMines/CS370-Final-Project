# Intruder-Be-Known: team rules

## Commands
- Build: ‘make‘ Cross/deploy: ‘make deploy PI=pi@<host>‘
- Tests: ‘make test‘ Sanitizers: ‘make asan‘ Valgrind: ‘make memcheck‘
- A change is DONE only when build, test, and asan pass. Show output.

## Hard constraints
- The PRODUCT makes no network calls except serving its own LAN interface. No LLM APIs, no cloud inference, no pretrained models. The intelligence in analysis/ is ours. This is a graded boundary.
- Every daemon must be supervisable: clean exit codes, no orphaned fds across restart, heartbeat within 60s of start.
- Every allocation checked; every syscall’s error path handled and logged. The 48-hour soak is the test suite of last resort.
- NEVER weaken, skip, or delete a test to make the suite pass.

## Ownership
- Shared: supervisor, interface. The agent edits outside the current session owner’s area only when told explicitly whose session this is.

## Style
- Systems core: C17, -Wall -Wextra -Werror, no VLAs. goto-cleanup for multi-resource functions.
- Smallest diff that passes. Do not refactor unrelated code.

## Workflow
- Multi-file or algorithmic change: plan first, wait for approval.
- Hardware bugs: paste real evidence (dmesg, timing capture, /proc/interrupts). No fixes proposed from a verbal description.
- Commit only from a green state; message format "M<n>: <what>".
