# BaseModifiersTweaks - army AI province weighting
#
# The army AI scores every province and marches to one of them. These files
# adjust that score under conditions we name. Vanilla ships the directory with
# three files, all commented out, so there is no stock behaviour to override.
#
#
# THE SCORE IS A COST. LOWEST WINS. WE SUBTRACT TO ATTRACT.
#
# Confirmed in game. The armyeval tooltip shows a sum of hardcoded terms, then
# any scripted adjustment, then the final score. Read for one Kilwa army in a
# single war:
#
#   Macua, the war target   110000 + 383 + 60000 + 1876 = 172260
#   unrelated neighbour     110000 + 265 + 60000 + 2432 = 172698
#
# Two provinces within 0.3% of each other. The army went to the lower one.
#
#
# WHY THESE FILES USE eval_add AND NOT eval_multiply.
#
# Every hardcoded term in that sum is a PENALTY: "over supply limit",
# "not focal province" (+60000), "wrong region" (+9000 to +20000), distance.
# A multiplier scales the penalties, so the more hostile, distant and
# over-supplied a province is, the more leverage the multiplier gets on it -
# and a NEGATIVE multiplier turns the whole penalty stack into a reward. The
# worse a province was, the more the army wanted it.
#
# That is not a tuning problem, it is the wrong operator. We used eval_multiply
# for two revisions and it produced exactly that inversion: a Jianzhou army
# marched across Asia to the capital of a country it was not at war with,
# because 182094 x -5 = -910469 was the lowest score on the map.
#
# An addition shifts a score by a constant. It cannot invert an ordering, it
# cannot be amplified by someone else's penalty, and two of them cannot
# multiply into a sign flip. Use eval_add. Do not reintroduce eval_multiply.
#
# The shape follows vanilla's own dummy in 00_ai_army_province_war.txt: outer
# factor 0, with the real value on the modifier inside.
#
#
# Scale. Pick values against the engine's own adds, not against nothing:
# "wrong region" is +9000 to +20000 and "not focal province" is +60000. Ours
# sit between -20000 and +20000, large enough to reorder provinces within a
# branch and too small to drag one across branches - the supply-check branch
# starts at +110000 and the fight branch at +200, and nothing here should be
# papering over a gap that size.
#
#   05_capital_besieged            -20000   defend the capital above all
#   06_  + craven ruler            -10000   a coward defends his throne
#   07_  + Emperor of China        -10000   mandate hangs off the capital
#   10_own_fort_besieged           -10000   the fort dice bonus pairs with this
#   11_  + prosperous, early age    -3000   dev 12+ before Absolutism
#   12_  + prosperous, late age     -3000   dev 22+ from Absolutism
#   13_  + defensible ground        -3000   hills/mountain/highlands/ramparts
#   15_cores_and_claims             -8000   fight for the objective
#   20_own_province_besieged        -5000   general homeland defence
#   30_enemy_fort_strategic_ground  -2000   prefer taking the pass
#   40_bordering_my_land            -6000   fight the front, not four regions away
#   80_low_priority_targets        +20000   too far, or an isolated island
#
# They add, so overlapping rules accumulate:
#   craven ruler's besieged capital            -20000 - 10000 = -30000
#   besieged mountain fort worth 25 dev, 1700  -10000 - 3000 - 3000 = -16000
#   plain besieged fort on flat poor ground    -10000
#   enemy province four regions away           +20000
#
# Files 11 and 12 are mutually exclusive by age. Every avoidance rule lives in
# file 80 so the penalties stay in one place.
#
#
# ROOT is the evaluating country. owned_by = ROOT, sieged_by = ROOT,
# units_in_province = ROOT and country_or_subject_holds = ROOT all work here.
# Note "holds" may mean CONTROLS rather than owns - unconfirmed, and it matters
# for occupied provinces.
#
# Recalculates only when the war list changes, a province changes hands, a new
# year ticks, on load, or on console "ai_army_tick". A standing posture, not a
# reaction to this month's siege.
#
#
# OPEN QUESTION, worth settling on the next test. A war objective - the enemy
# war leader's capital, or the wargoal province - gets its score NEGATED by
# something, which makes it the most attractive province on the map and keeps
# armies parked on it long after it is taken. Denmark's entire coalition sat on
# occupied Stockholm bleeding attrition for exactly this reason.
#
# We do not yet know whether that negation is ours or the engine's. With no
# eval_multiply left anywhere in this directory, the "Scripted multiply" line
# should now be gone. If a war objective still shows a negative multiply, it
# was never ours and the behaviour is vanilla's. If it disappears, it was.
# Either answer is worth having - check one objective province and see.
#
#
# How to check in game: console "ai_army_tick", then "mapmode armyeval", then
# select an army and hover. A besieged fort of yours should read LOWER than a
# distant enemy province.
#
# These are new files, not copies of vanilla ones, so no @ markers are used.
