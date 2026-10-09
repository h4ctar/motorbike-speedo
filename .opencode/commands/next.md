---
description: Take the next task from TODO.md, implement, verify, review, and commit
---

Work on the next task in `TODO.md`: $ARGUMENTS
If no task was named above, take the first unchecked item.

1. **Plan**: read the relevant code and `AGENTS.md`. If the task needs a CubeMX change (new pin, peripheral, timer, interrupt, or clock config), stop and tell me exactly what to set in STM32CubeMX. Don't hand-edit generated config. If the task is ambiguous, ask before writing code.
2. **Implement**: keep changes small. In generated files, edit only inside `USER CODE` blocks.
3. **Verify**: run `speedo-code/check.sh` until it passes with no warnings.
4. **Review**: launch the `reviewer` subagent and fix every real issue it reports. Re-run `check.sh` afterwards.
5. **Hardware test plan**: write a short checklist of what I should see on the board (needle, OLED, button behaviour). Include `speedo-code/flash.sh` as the flash step. Don't flash it yourself unless I ask.
6. **Commit**: tick the item in `TODO.md` only after I confirm the hardware test passes. Until then, commit the code locally with a message describing the change, and leave the item unchecked with a note "(awaiting hardware test)". Never push.
