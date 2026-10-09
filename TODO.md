# TODO

Work through with `/next` (first unchecked item) or `/next <item>`. Every item needs a "Done when" line that can be checked on the board.

## CI

- [ ] **Fix the Node.js 20 deprecation warning in GitHub Actions** (all 3 jobs): bumped `actions/checkout` and `actions/upload-artifact` from v4 to v7 (Node 24). The firmware build itself had no compiler warnings.
      Done when: the next push shows no warning annotations (`gh api repos/{owner}/{repo}/check-runs/<job-id>/annotations`). The "ubuntu-latest will migrate to Ubuntu 26 on 2026-10-19" notice is informational; just check the Firmware job still passes after that date.
- [ ] **KiBot warnings in the PCB job** (they don't fail CI, but they're noise). These need PCB edits, so discuss with Ben first:
  - W135: "part field mismatch" for most parts. The schematic `Value` (e.g. `0402,4.7kΩ`) differs from the PCB/LCSC part number field. Possibly fix with a KiBot field mapping in `config.kibot.yaml` rather than by editing parts.
  - W147: 6 footprints with reference `REF**` (likely mounting holes or the gauge outline at (77,77), (123,123), etc.). Give them real references (H1…) or mark them as board-only.
  - DRC: 11 warnings (6 "extra footprint", 5 "duplicate footprints"), probably the same `REF**` footprints.
      Done when: the KiBot log (`gh run view <run-id> --log | grep WARNING`) has no W135/W145/W147 lines, and the board is electrically unchanged.

## Known bugs (found in existing code)

- [ ] **Motor timing wraps wrongly.** TIM1's counter is 16-bit, but `motor.c` compares it to a `uint32_t lastStepTime`. Each time the counter wraps, the difference is huge, so the motor takes an early step.
      Done when: step timing uses 16-bit unsigned subtraction, and the needle sweeps smoothly without jitter.
- [ ] **Negative positions aren't clamped.** In `setMotorTargetPosition`, `targetSteps < 0` is always false (`targetSteps` is unsigned), and converting a negative float to `uint16_t` is undefined behaviour.
      Done when: the input is clamped as a float before conversion.
- [ ] **The needle never moves after homing.** Nothing calls `updateMotor()` from `loop()`, and `updateMotor` isn't declared in `motor.h`.
      Done when: `loop()` services the motor, and setting a target position moves the needle without blocking the display.

## Features (DRAFT: written by the agent from pins/code; Ben to edit)

- [ ] Decide what the needle shows (speed? volts? time?) and its input source. <!-- Ben: fill in -->
- [ ] **MODE button (PA6)**: read with debouncing, short and long press.
      Done when: <!-- Ben: what should MODE do? -->
- [ ] **Set the clock**: use the MODE button to set hours and minutes on the OLED, replacing the commented-out `HAL_RTC_SetTime` in `setup()`.
      Done when: the time can be set from the button and survives a reset.
- [ ] **Voltage reading (PA7 / ADC1 ch7)**: read and scale it to volts using the PCB divider (values from `speedo-pcb` schematic).
      Done when: <!-- Ben: show on OLED? needle? low-battery warning? -->
- [ ] Stop redrawing the OLED on every loop iteration; only redraw when the minute changes.

## PCB revision notes

Record board problems found during hardware testing here. Only edit the PCB after discussion.
