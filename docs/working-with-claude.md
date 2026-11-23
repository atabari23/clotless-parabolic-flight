# Working with Claude on CLOT-LESS

[← Back to overview](../README.md)

I'm a medical student with a pharmacy background, not a software engineer. CLOT-LESS needed real-time control software, hardware bring-up on a Raspberry Pi, flight-data analysis and a journal-grade write-up, on a student campaign timeline. I used Claude, mainly through Claude Code, as a working member of the team for all four. This page describes how that went and what I learned about doing it well.

## What Claude did

**Flight control software.** The payload runs a Python control panel on a Raspberry Pi 5 that drives two syringe pumps, eight pinch valves, four pressure sensors and two accelerometers. In the last ten days before flight it went through four versions, each one tested on the real hardware, with a test suite that grew to 238 automated tests. Features came straight from bench problems and operator feedback:

- one-click paired runs that sequence valves, pumps and logging automatically
- logging that starts the moment the panel opens, so a run can never go unrecorded
- live pressure charts with event markers, and a pause-and-inspect mode
- a read-only phone dashboard whose only write actions are a hold-to-fire STOP ALL and an observation marker
- charts redesigned after the operator found an earlier layout hard to read at a glance

**Hardware safety.** This was real hardware with no end stops. Two incidents during bring-up needed the 24 V supply pulled by hand. We turned them into standing rules that Claude followed for every motor command afterwards: a timeout on every remote motion (a dropped SSH session can otherwise leave a motor running unattended), no reverse moves with stall detection off, and always ask me where the carriage physically sits before moving it.

**Bench diagnostics.** Flow tests on the pumps turned up a periodic swell in one pump's output. Claude matched the period to exactly one lead-screw revolution at two different speeds, which pointed to mechanical runout in that pump rather than sensor noise.

**Flight-data analysis.** On the day after the flight, Claude processed the 41.5-minute, 20 Hz log: it detected the 11 parabolas, timed every pump start against zero-g onset, audited data quality (zero dropped samples), measured how gravity leaked into each pressure line, and produced figures for the campaign debrief. It also traced the "missing" pressure samples to the sensors' own over-range status flag, which turned a suspected cabling fault into a sensor-range finding.

**Manuscript review.** Claude reviewed the journal draft against the raw flight log and found places where the text didn't match the data, including the number of parabolas flown and the cause of the pressure "gaps". It then implemented the revisions, rebuilt figures from the data and checked citations against their sources. The manuscript's AI-use declaration describes this.

## What I learned about working this way

**Ask for the analysis that could prove you wrong.** The most valuable moments were when Claude came back with "this doesn't hold up". The early pressure result looked like a clean drug effect. When the same comparison was repeated on the perfusion-rate part of each run, it didn't hold in two of the three pairs, and Claude's write-up said so plainly instead of burying it. That is why the results on this page are conservative.

**Check claims against the raw data, every time.** A polished draft can carry a wrong number very confidently. Making "check it against the log" the default caught errors that reading alone would have missed.

**Physical systems need hard rules, not good intentions.** Software mistakes can be undone. A carriage driven into a syringe can't. Writing the safety rules down once, and having them applied to every motor command afterwards, made it safe to move fast.

**Stay the scientist in charge.** Claude was fast and thorough, but the experimental design, the judgement calls in flight and what we claim in the paper stayed mine and my team's. The best results came from treating it like a very capable colleague whose work I still review.
