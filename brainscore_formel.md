BRAIN SCORE – FINAL FORMULA (LOCKED)
==================================

Design assumptions
------------------
• Two observation phases → 2 × 15 s free observation
• Total free observation time: 30 s
• Maximum observation time: 120 s
• Hits dominate over time
• Each additional hit must stay meaningful up to 6 hits
• Random luck is capped and separated from skill

--------------------------------------------------

Definitions
-----------
T = total observation time in seconds (sum of all observation phases, including jokers)
H = number of hits (integer, 0…6)

Constants
---------
FREE_TIME = 30        // seconds (2 phases × 15 s)
MAX_TIME  = 120       // seconds
EXTRA_TIME_RANGE = 90 // MAX_TIME - FREE_TIME

--------------------------------------------------

Clamp observation time
----------------------
if T < FREE_TIME → T = FREE_TIME
if T > MAX_TIME  → T = MAX_TIME

--------------------------------------------------

Time Factor
-----------
Extra observation time:
E = clamp(T - FREE_TIME, 0…EXTRA_TIME_RANGE)

Time score factor:
S_time = 1 - 0.31 * (E / EXTRA_TIME_RANGE) ^ 0.855

Properties:
• S_time = 1.00 at T = 30 s
• S_time = 0.69 at T = 120 s

--------------------------------------------------

Hit Factor (logistic, fairness-balanced)
----------------------------------------
k = 1.0              // fairness slope (lower = more even hit weighting)
c = 2.5976583        // shift (calibrated to anchor points)

Raw logistic:
L(H) = 1 / (1 + exp(-k * (H - c)))

Normalized hit factor:
S_hits = L(H) / L(6)

Special rule:
• If H = 0 → no percentage score

--------------------------------------------------

Final Brain Score
-----------------
If H = 0:
  Display: "Brain Score: No Hit, no Score"

Else:
  BrainScore (%) = 100 * S_time * S_hits

--------------------------------------------------

Anchor validation
-----------------
• T = 30 s, H = 6 → 100 %
• T = 120 s, H = 6 → 69 %
• T = 120 s, H = 1 → 12 %
• H = 0 → No Hit, no Score

--------------------------------------------------

Modification rule
-----------------
This formula is locked.
Any change to constants (k, c, exponents, coefficients)
requires re-validation of all anchor points.
