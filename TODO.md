# TODO

Work through with `/next` (first unchecked item) or `/next <item>`. Every item needs a "Done when" line that can be checked on the board.

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
