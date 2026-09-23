# BaseModifiersTweaks - army AI province weighting
#
# What this directory does: the army AI scores every province and marches to
# one of them. These files multiply that score under conditions we name.
# Vanilla ships the directory with three files, all commented out, so there is
# no stock behaviour here to override - only additions.
#
#
# THE SCORE IS A COST. LOWER IS BETTER. LOWEST WINS.
#
# This is confirmed in game, not inferred. The armyeval tooltip shows a sum of
# hardcoded terms, then "Scripted multiply", then the final score. Four
# provinces read for a Jianzhou army at war:
#
#   own capital            200 + 853 + 50 - 62 - 100  =    942  x5  =   4708
#   enemy capital          200 + 1030 + 9000 - 340    =   9890  x5  =  49451
#   enemy leader capital   200 + 1292 + 500 + 9000 - 284 = 10708 x5 =  53539
#   Oirat capital (far)    110000 + 60000 + 12094     = 182094  x-5 = -910469
#
# The army went to Oirat's capital - the lowest score, and a country it was not
# even at war with. So a LOW score is what the AI picks.
#
# Consequences, both of which cost us a playtest to learn:
#
#   To make the AI PREFER a province, use a factor BELOW 1. It shrinks the cost.
#   To make the AI AVOID a province, use a factor ABOVE 1. It inflates the cost.
#   NEVER use a negative factor. It flips a large positive cost into a large
#   negative one, which is the best possible score, and sends armies straight
#   to the provinces you meant to forbid. That is exactly what -0.8 did above:
#   182094 x -5 = -910469, the lowest score on the map by a factor of two
#   hundred, on a neutral country's capital on the far side of Asia.
#
# Vanilla's own readme in common/ai_army/99_ai_army_readme.txt states this
# correctly ("AI will choose the province with the *lowest* value"). An earlier
# version of this directory assumed the opposite, on the strength of a shipped
# AI overhaul mod that uses positive factors for targets it wants and negative
# ones for targets it does not. That mod appears to have the same inversion;
# its filenames describe the opposite of what its numbers do. Trust the
# tooltip, not another mod.
#
#
# ROOT is the evaluating country. owned_by = ROOT, sieged_by = ROOT,
# units_in_province = ROOT and country_or_subject_holds = ROOT all work inside
# these blocks.
#
# When it recalculates: only when the country's war list changes, when it
# gains or loses a province, on a new year, and on load - or on the console
# command "ai_army_tick". This is a standing posture, not a reaction to a siege
# that started last month.
#
# What this CANNOT do: the hardcoded terms dwarf everything. "Not focal
# province" is +60000 and "wrong region" is +9000 against a fight-branch base
# of 200. A multiplier is proportional, so it can move a province within its
# branch but never between branches. Do not expect miracles from these files.
#
# The multiply displayed exactly +-5.000 on all five provinces read, which is
# too uniform to be a product of the varied factors then in use. It looks like
# the engine clamps scripted multiply to +-5. If so, the values below sit
# inside the usable range instead of saturating at the limit - worth
# re-reading one province's tooltip to confirm.
#
#
# Files MULTIPLY together where their conditions overlap, which is why several
# repeat the same besieged-and-undefended clause. Each adds a separate reason
# to care, and each drives the cost further down.
#
#   05_capital_besieged             0.2    defend the capital above all
#   06_  + craven ruler             0.625  a coward defends his throne
#   07_  + Emperor of China         0.67   mandate hangs off the capital
#   10_own_fort_besieged            0.42   the fort dice bonus pairs with this
#   11_  + prosperous, early age    0.71   dev 12+ before Absolutism
#   12_  + prosperous, late age     0.71   dev 22+ from Absolutism
#   13_  + defensible ground        0.67   hills/mountain/highlands/ramparts
#   15_cores_and_claims             0.33   fight for the objective
#   20_own_province_besieged        0.5    general homeland defence
#   30_enemy_fort_strategic_ground  0.77   prefer taking the pass
#   40_bordering_my_land            0.55   fight on the front, not four regions away
#   80_low_priority_targets         2.5    too far, or an isolated island
#
# Worked examples, remembering that lower is better:
#   craven ruler's besieged capital            0.2 x 0.625        = 0.125
#   besieged mountain fort worth 25 dev, 1700  0.42 x 0.71 x 0.67 = 0.20
#   plain besieged fort on flat poor ground    0.42
#   enemy province four regions away           2.5
#
# Files 11 and 12 are mutually exclusive by age, so the prosperity bonus can
# never apply twice. Every avoidance rule lives in file 80 behind one OR, so
# penalties cannot compound unexpectedly - add new ones to that OR rather than
# as a new file.
#
#
# How to check in game: console "ai_army_tick", then "mapmode armyeval", then
# select an army and hover provinces. The tooltip shows the branch sum, the
# scripted multiply and the final score. A besieged fort of yours should read
# LOWER than a distant enemy province. If any province shows a negative score,
# something here has a negative factor and needs finding.
#
# These are new files, not copies of vanilla ones, so no @ markers are used.
