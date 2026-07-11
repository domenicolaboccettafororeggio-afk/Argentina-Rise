# Post-Mexico Global Crisis Crash Fix Report v1.0

## Scope

This report documents the crash affecting `ARG 7.hoi4` after selecting the multipolar posture in `ARG_rise.93`. The save was not modified.

## Evidence

At `01:00, 5 August 2025`, `game.log` recorded the message `post-Mexico world crisis announced` nine times. The message came from the option of `ARG_rise.94`, a global `news_event`.

At `01:00, 6 August 2025`, the crash report recorded these script failures:

```text
events/ARG_rise_global_crisis_events.txt:83: invalid event target: event_target:ARG_rise_crisis_argentina
events/ARG_rise_global_crisis_events.txt:89: invalid event target: event_target:ARG_rise_crisis_argentina
events/ARG_rise_global_crisis_events.txt:90: invalid event target: event_target:ARG_rise_crisis_argentina
```

The application then terminated with `EXCEPTION_ACCESS_VIOLATION`.

## Root Causes

1. `ARG_rise.94` scheduled `ARG_rise.95` from an option in a global news event. HOI4 evaluates that option for each news recipient, multiplying the dispatch.
2. `ARG_rise.93` used `save_event_target_as`. That target exists only in the originating event scope and is invalid when delayed events run in USA, SOV, or CHI.

## Applied Correction

- Replaced `save_event_target_as` with vanilla's persistent `save_global_event_target_as` pattern.
- Moved the one-time global crisis flag and the USA/SOV/CHI dispatch into the selected option of `ARG_rise.93`.
- Preserved the intended timing: `.94` appears after one day and `.95` begins after two days.
- Made `.94` a pure major news event with no state-changing effects or event dispatch.
- Preserved all existing response events, flags, opinion modifiers, and fallbacks.

## Test Procedure

1. Load `ARG 7.hoi4`, the save from before the posture choice.
2. Choose the multipolar option in `ARG_rise.93`.
3. Advance two in-game days.
4. Confirm that `.94` appears once and that Washington receives one `.95` event.
5. Check `game.log`: the former repeated `post-Mexico world crisis announced` entries must be absent.
6. Check `error.log`: no `invalid event target: event_target:ARG_rise_crisis_argentina` entries may appear.

## Compatibility

The correction changes only the Argentina Rise event chain. Millennium Dawn, the base game, and the save file remain untouched.
