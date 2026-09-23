# BaseModifiersTweaks - army AI province weighting
#
# DRAFT. Not yet playtested.
#
# What this directory does: the army AI scores every province and marches to
# the best one. These files multiply that score under conditions we name.
# Vanilla ships the directory with three files, all commented out, so there is
# no stock behaviour here to override - only additions.
#
# Direction of the score: HIGHER IS MORE DESIRABLE. Vanilla's own readme in
# common/ai_army/99_ai_army_readme.txt says the opposite ("AI will choose the
# province with the *lowest* value"), and that appears to be wrong. Evidence:
# a shipped AI overhaul mod deprioritises unwanted targets with NEGATIVE
# factors and prioritises wanted ones with factors from 1.4 to 7.6. If lowest
# won, its negative factors would make remote islands the top priority.
# Believe the working mod, but this is the single assumption most worth
# confirming in game - see "How to check" below.
#
# ROOT is the evaluating country. owned_by = ROOT, sieged_by = ROOT,
# units_in_province = ROOT and country_or_subject_holds = ROOT all work inside
# these blocks.
#
# When it recalculates: only when the country's war list changes, when it
# gains or loses a province, on a new year, and on load. This is a standing
# posture, not a reaction to a siege that started last month. Do not expect it
# to redirect an army mid-campaign.
#
# Why no rebel files: an obvious addition would be "rebels are besieging my
# province, go there". Deliberately left out, because making the AI hunt
# rebels harder cuts against issue 7 on ListOfIssues (rebels too weak mid to
# late game), which we have separately been trying to fix. Add it only if the
# AI turns out to ignore rebels to the point of collapse.
#
# Interaction to be aware of: the mod this technique came from RAISED
# ACCEPTABLE_BALANCE_DEFAULT to 1.375, making its AI more reluctant to fight,
# and then used these files to drag armies where they were needed so battles
# happened anyway. We went the other way and lowered it to 1.1. Those are
# substitutes, not complements. If these files work, our 1.1 is worth
# revisiting rather than leaving both changes stacked.
#
# Values here are deliberately gentler than the source mod's, because our AI
# already accepts battles it would otherwise decline.
#
# Files MULTIPLY together where their conditions overlap, which is why several
# of them repeat the same besieged-and-undefended clause. Each one adds a
# separate reason to care.
#
#   05_capital_besieged             5.0    defend the capital above all
#   06_  + craven ruler             1.6    a coward defends his throne
#   07_  + Emperor of China         1.5    mandate hangs off the capital
#   10_own_fort_besieged            2.4    the fort dice bonus pairs with this
#   11_  + prosperous, early age    1.4    dev 12+ before Absolutism
#   12_  + prosperous, late age     1.4    dev 22+ from Absolutism
#   13_  + defensible ground        1.5    hills/mountain/highlands/ramparts
#   15_cores_and_claims             3.0    fight for the objective
#   20_own_province_besieged        2.0    general homeland defence
#   30_enemy_fort_strategic_ground  1.3    prefer taking the pass
#   80_distant_enemy               -0.8    stop crossing the map
#   85_isolated_island             -1.0    stop chasing overseas scraps
#
# Worked examples of the stacking:
#   craven ruler's besieged capital            5.0 x 1.6      = 8.0
#   besieged mountain fort worth 25 dev, 1700  2.4 x 1.4 x 1.5 = 5.04
#   plain besieged fort on flat poor ground    2.4
#
# Files 11 and 12 are mutually exclusive by age, so the prosperity bonus can
# never apply twice.
#
# How to check in game: console "mapmode armyeval", then select an army. The
# map shades provinces by their evaluation score. Start a war as a country
# with a besieged fort and confirm the fort province reads as more attractive
# than a distant enemy province. If it reads as LESS attractive, the direction
# assumption above is wrong and every factor in this directory needs its sign
# inverted - which is a five minute fix, but do it before a long campaign.
#
# These are new files, not copies of vanilla ones, so no @ markers are used.
