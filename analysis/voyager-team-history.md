# Voyager Team — A Research-Relevant History

This document captures the engineering-history content distilled from
public interviews, oral histories, and documentary footage of the
Voyager flight team. It complements the spacecraft-side technical
documentation by recording the **human-institutional context** that
explains why certain engineering choices were made, and why so much
of the original tooling has been lost.

Primary sources cited here:

- *It's Quieter in the Twilight* (Miossi, 2022) — `primary-sources/its-quieter-in-the-twilight-2022/`
- David Cummings, *How We Diagnosed and Fixed the 2023 Voyager 1 Anomaly* (2025)
- Bruce Waggoner, *Saving Voyager 1!* (2024)
- James E. Tomayko, *Computers in Spaceflight* (1988)
- Voyager team biographical materials at JPL and elsewhere

---

## The institutional knowledge problem

The Voyagers are operated by two intertwined teams: the **original
engineers** (who designed and built the spacecraft 1972–1977) and the
**second-wave engineers** (who joined for the outer-planet encounters
1979–1989 and stayed through the Voyager Interstellar Mission).

By 2024, the original team is essentially gone — most retired
1990–2010, several have passed away. Larry Zottarelli, the last
original Voyager programmer, retired around 2016. The second-wave
team is now the institutional memory, and they are themselves
mostly past 70.

Engineers interviewed in *It's Quieter in the Twilight* (2022)
characterise the situation in stark terms — paraphrased:

- The team has accumulated knowledge of the spacecraft's
  idiosyncrasies that simply doesn't exist in any document.
- Most members are *single-point failures* — if one of them stops
  contributing, that piece of knowledge is gone.
- It may be a race between how long the engineers live versus how
  long the spacecraft can still communicate.

This is the *human face* of the same problem visible in David
Cummings' 2025 keynote, where JPL had to recover Voyager 1's flight
software with no working assembler and an OCR'd Microsoft Word
listing of unknown accuracy. The tooling was lost because the
people who maintained it retired, and nobody was funded to make it
survive their departure.

---

## The team's demographic and geographic origins

The film makes visible something that engineering literature usually
doesn't: the late-stage Voyager team is **highly multicultural and
multi-generational**. Engineers interviewed describe origins in
Mexico, South Korea, Colombia, Mississippi/Alabama (Tuskegee
Institute graduates), and several other countries. Many were
recruited by JPL on US university campus visits in the mid-1980s and
came to Voyager as their first job, then stayed for 30+ years.

The point isn't that this is unusual for an aerospace team — JPL has
always hired this way — but that the *continuity* of the Voyager
project means a single mission preserved careers and life stories
that crossed national, racial, and generational boundaries for almost
50 years. The mission's success at hiring and retaining is part of
why it survived institutional churn elsewhere.

---

## The Uranus-encounter generation

Several current engineers joined Voyager specifically for the
**Uranus encounter (1986)**. The mission was high-profile, JPL was
recruiting, and assembly-language programmers were in demand to
prepare flight-software changes for the encounter.

> *"I was an assembly language programmer, the lowest level that
> you can program. I was hired with JPL as an independent
> contractor."*  — paraphrased, anonymous engineer in
> *It's Quieter in the Twilight* (2022)

After Uranus and Neptune (1989), as the team contracted into
Voyager Interstellar Mission staffing, these engineers absorbed
multiple subsystem roles. One says, paraphrased: *when the power
subsystem engineer left, I took over that function; when the
propulsion engineer left, I took over that function*. By 2020 a
single engineer might be the sole subject-matter expert for several
spacecraft subsystems.

This is the operational reality of *single-point failures*: not
because anyone designed it that way, but because the team kept
shrinking and the survivors took on what was left.

---

## Voyager 2 — the power-subsystem command anomaly

Distinct from the well-publicised Voyager 1 FDS memory failure
(November 2023), Voyager 2 has its own engineering puzzle that the
team has not root-caused.

From *It's Quieter in the Twilight*, paraphrased:

- The Computer Command Subsystem issues commands to the power
  subsystem as roughly 14-bit command words that select a relay
  to be opened or closed.
- Conceptually, the relay-decode matrix is organised as rows and
  columns; commanding row R / column C should activate exactly
  one specific relay.
- Occasionally on Voyager 2, commanding a specific (R, C) pair
  inadvertently activates another relay in the same column.
- The crosstalk is unique to Voyager 2 — Voyager 1 doesn't show it.
- The team has not reached a satisfying explanation. As one
  engineer puts it: *we have not figured out what failure is
  lurking in the power subsystem.*

Operationally, the workaround is to anticipate which commands risk
crosstalk and avoid issuing them. With ~22.5-hour round-trip light
time, "you can't do things very quickly, so you got to anticipate
what the spacecraft is gonna do."

This anomaly does not currently appear in our `extracted-data/`
corpus because we have no public hex/listing data for V2's CCS
power-subsystem command interface — only the team's narrative
description.

---

## DSS-43 downtime (2020–2021)

Voyager 2's trajectory took it south of the ecliptic. Of the three
DSN 70-meter dishes, **only Canberra DSS-43 (Australia) can point
at Voyager 2** for command uplink. The other two (Goldstone in
California, Madrid in Spain) are at the wrong latitudes.

In **March 2020**, DSS-43 went down for major refurbishment.
Voyager 2 was effectively **uncommandable for ~11 months**, with
Earth still able to receive its downlinked telemetry but unable to
send any new commands. Limited test contact resumed October 2020;
full operations resumed February 2021.

The team prepared for this downtime by uplinking a long,
fault-tolerant command sequence in advance, then crossing fingers
for the better part of a year. Per the documentary, paraphrased: a
*low-level engineer* was during that period accidentally instructed
to take part of JPL's command network down, briefly damaging the
infrastructure that Voyager uses. The team caught it, recovered,
and finished the refurb successfully.

This is operationally similar in spirit to the Voyager 1 FDS
recovery: any one mistake by anybody at the wrong moment can
permanently kill the mission, and the team has to over-engineer
every action.

---

## Power-margin endpoint

Suzanne Dodd, Voyager Project Manager, shows in the film a
power-budget plot tracing RTG output decline. The hard endpoint
where the spacecraft no longer has enough power to operate even
the radio is around **2030**, with possible variation either
direction depending on instrument-shutdown decisions.

This date is consistent with all other public statements — JPL
press releases, NASA fact sheets, and team interviews all converge
on roughly 2030 as the terminus.

---

## Operational philosophy

The film captures, at multiple points, the team's working culture
which has direct bearing on why their engineering succeeds. Three
recurring themes:

1. **Conservatism by necessity.** With round-trip light time of
   22.5+ hours and no testbed for verification, the team's default
   posture is to do nothing unless there's a clear positive case.
   Mistakes are unrecoverable.
2. **Anticipation over reaction.** Every commanding decision is
   made based on *what the spacecraft will do next* rather than
   what it just did. By the time a downlink reports a problem,
   that problem occurred a day ago.
3. **Distributed expertise.** No one engineer knows the whole
   spacecraft. Every change requires the *team* to debate, walk
   through possible failures, build checklists, and iterate. This
   is what Cummings' keynote describes as *"bare-knuckle binary
   manipulation"* — there's no tooling, only collective human
   expertise.

The 2030 endpoint, when it arrives, will end the mission. It will
not, for several years afterward, end the team's professional and
personal connection to it. Many of the engineers in *It's Quieter
in the Twilight* describe the spacecraft as something between a
colleague and a family member. *"You don't wanna let down Voyager."*
