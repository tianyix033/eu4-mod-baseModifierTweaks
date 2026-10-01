# BaseModifiersTweaks

A Europa Universalis IV mod that changes the game's global rules rather than any
particular country. Target version 1.37.5.0.

The aim throughout is historical plausibility: wars should be slow and costly,
rebellions should be dangerous, technology should spread unevenly, and no idea
group should be an obvious auto-pick.

## What it changes

**Attrition and war.** Monthly attrition caps are raised and terrain carries real
hostile attrition, so campaigning in the steppe, desert or mountains costs an army
something. Several unit and price values move with it.

**Rebels.** All 40 rebel types are flagged `resilient`, `reinforcing` and `smart`,
so a revolt that is ignored becomes a war rather than a nuisance. Every country
gets a flat rebel support efficiency bonus, and `REVOLT_TECH_IMPACT` is raised so
rebel stacks scale with the era.

**Idea groups.** `00_basic_ideas.txt` is rebalanced so the weak groups are worth
taking — influence, espionage, maritime, aristocracy and plutocracy all gain —
and innovativeness is given military boost to firepower. `FREE_IDEA_GROUP_COST` rises from 3 to 4.

**Technology and institutions.** Institution spread is slowed to a third of
vanilla, and the link between uneven tech and corruption is removed outright
(`LAGGINGTECH_CORRUPTION = 0`) — it never made sense that being behind in one
field bred graft.

**Horde government reforms.** `steppe_horde` and `great_mongol_state_reform` trade
cheap cavalry and low attrition for poor tax, poor governance and no colonial
range.

**AI behaviour.** The province-defence weights are retuned so an AI under threat
defends its homeland instead of marching off, and it will develop provinces past
vanilla's low cap.

## Install

Copy this folder into

    Documents/Paradox Interactive/Europa Universalis IV/mod/

and create a sibling `BaseModifiersTweaks.mod` next to it containing the same
lines as `descriptor.mod` plus an absolute path, forward slashes:

    path="C:/Users/<you>/Documents/Paradox Interactive/Europa Universalis IV/mod/BaseModifiersTweaks"

EU4 ignores the launcher's load order and resolves conflicts by mod *name*, with
the earlier-sorting name winning.

## Conventions

Every line changed in a copied vanilla file carries an `@` in its trailing
comment, with the previous value: `#@ was 5` in script files, `--@ was 0.1` in
`defines.lua`. A plain text search for `@` therefore lists every line this mod
touches, and deleted lines are commented out rather than removed so the search
still finds them. `git diff` against each file's first, unmodified commit is the
authoritative record.

Script files are CP-1252 with LF endings and no BOM; localisation is UTF-8 with
BOM. Both matter — EU4 fails quietly on the wrong one.

## Note

Most EU4 directories override by filename rather than merging, so changing a few
values means copying the whole vanilla file and editing it in place. A number of
files here are therefore Paradox Interactive's, with edits marked as above. This
is an unofficial personal mod, not affiliated with or endorsed by Paradox
Interactive.
