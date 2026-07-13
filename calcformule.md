POKÉMON VGC DAMAGE CALCULATION FORMULA & RULES
==================================================
This document defines the strict mathematical sequence for calculating battle damage in Double Battles (VGC). NotebookLM must follow this step-by-step logic linearly without skipping any modifiers.

--------------------------------------------------
1. THE CORE DAMAGE FORMULA
--------------------------------------------------
When a Pokémon uses an attacking move against a target, the base damage is calculated exactly as follows:

Damage = ((((2 * Level / 5) + 2) * Power * (A / D) / 50) + 2) * Targets * Weather * Critical * Random * STAB * Type1 * Type2 * Other

Where:
- Level = The attacking Pokémon's current level (Standard VGC is always Level 50).
- Power = The Base Power of the move being used (from 'pokemon_moves_vgc.json').
- A (Attacker's Stat): 
  * Uses the 'Attack' stat if the move category is 'Physical'.
  * Uses the 'Special Attack' (SpA) stat if the move category is 'Special'.
- D (Defender's Stat):
  * Uses the 'Defense' (Def) stat if the attack is 'Physical'.
  * Uses the 'Special Defense' (SpD) stat if the attack is 'Special'.

--------------------------------------------------
2. SEQUENTIAL MODIFIERS (MULTIPLIERS)
--------------------------------------------------
Apply the following multipliers in exact order from left to right:

* A. TARGETS MODIFIER (Spread Damage Reduction)
  - If the move has 'is_spread_damage': true in 'pokemon_moves_vgc.json' AND there is more than one active target on the opponent's field, multiply the damage by 0.75.
  - If it is a single-target move, multiply by 1.0.

* B. WEATHER MODIFIER
  - Multiply by 1.5 if a Fire-type move is used in Harsh Sunlight, or a Water-type move is used in Rain.
  - Multiply by 0.5 if a Water-type move is used in Harsh Sunlight, or a Fire-type move is used in Rain.
  - Otherwise, multiply by 1.0.

* C. CRITICAL HIT MODIFIER
  - If the hit is a Critical Hit, multiply by 1.5. (Critical hits also ignore any defensive stat boosts from the target).
  - Otherwise, multiply by 1.0. 

* D. RANDOM FACTOR
  - To show a damage range, calculate the minimum and maximum possible rolls.
  - Random is a value between 0.85 (minimum roll) and 1.00 (maximum roll), stepping by decimals (0.85, 0.86, ..., 1.00).

* E. STAB (Same-Type Attack Bonus)
  - If the Type of the move matches one of the Types of the attacking Pokémon, multiply by 1.5.
  - If the attacker has the ability Adaptability and types match, multiply by 2.0.
  - Otherwise, multiply by 1.0.

* F. TYPE EFFECTIVENESS (From 'type_chart_vgc.txt')
  - Type1: Match the move's type against the defender