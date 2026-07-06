# Notes on Bug Fixes

## Explanation of Fix: Double-Booking Bug (Task 1)

The `dates_overlap()` function was checking only if the new booking's start date fell within an existing booking's range: `start_b <= start_a <= end_b`. This missed overlaps where the new booking started before the existing one but ended during it, or started during and ended after.

The fix changes the logic to: `start_a <= end_b and end_a >= start_b`. This correctly detects any overlap between two date ranges by checking that the new booking's start doesn't come after the existing end AND the new booking's end doesn't come before the existing start.

## Double-Booking Example

**Existing booking:** Canon DSLR Camera from 2026-01-10 to 2026-01-15

**Wrongly allowed by original code:** Booking 2026-01-08 to 2026-01-12
- Original check: `10 <= 8 <= 15` → False (no conflict detected, booking allowed)
- Fixed check: `8 <= 15 and 12 >= 10` → True (conflict correctly detected, booking blocked)

## AI Use

Used Copilot to help understand the codebase structure and identify which functions handled date overlap checking. Verified the fix by:
1. Tracing through the logic manually with the Jan 8-12 vs Jan 10-15 example
2. Confirming the fixed overlap logic with the inclusive billing rule
3. Testing the other bug fixes by reviewing the code flow and boundary conditions (e.g., same-day rental math)
4. Checking that the maintenance equipment filter was applied consistently in both the availability endpoint and booking creation endpoint
