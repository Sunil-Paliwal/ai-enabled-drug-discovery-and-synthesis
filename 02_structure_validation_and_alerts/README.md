# 02 — Structure Validation and Alerts

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis/blob/main/02_structure_validation_and_alerts/structure_validation_and_alerts.ipynb)

**Author:** Dr. Sunil Paliwal  
Department of Chemistry and Chemical Biology  
Stevens Institute of Technology

**Before you ask whether a molecule is active, ask whether it exists.**

## What this module teaches

Two different things can be wrong with a designed molecule, and they fail in two
different ways.

The first is that **it is not a molecule at all** — a CH<sub>2</sub> sitting in a
ring drawn as aromatic, a carbon with five bonds, an unclosed ring. Software does
not flag these politely: RDKit refuses to build the molecule and throws an error
written for programmers. If a pipeline does not catch that, the structure silently
vanishes from the dataset and the student learns nothing about why.

The second is that it **is** a valid molecule but **will not survive**, or reacts
with everything. An aldehyde, an acyl chloride, a gem-diol that is just the hydrate
of a carbonyl, an enol drawn where the ketone is the form that actually exists.
These parse perfectly and are chemically doomed.

The notebook checks both, in that order, because the second cannot run until the
first passes.

## What is in it

- **Stage 1 — structural validity.** Parses without sanitising, asks RDKit what is
  wrong, maps each problem to a specific atom, and translates the error into a
  sentence a chemist can act on. The offending atom is drawn, marked, and labelled
  with what it actually is.
- **Stage 1.5 — is the drawing complete?** Unspecified stereocentres and undefined
  double-bond geometry. A valid structure that still does not describe one compound.
- **Stage 2 — stability and reactivity.** 37 curated patterns plus the PAINS, Brenk
  and NIH catalogues, merged so one group is reported once. Every hit carries the
  chemistry behind it and a suggested replacement.
- **Mutagenicity with mechanisms**, capped at medium severity on purpose.
- **A reality check** — eleven approved drugs through the same filters. Most trip
  something, and in four cases the flagged group *is* the mechanism.
- **Batch mode** for auditing generative-model output: not just how many were
  valid, but which failure modes dominated.
- **PDF and HTML reports** behind download buttons.

## Prerequisites

None. This module stands alone, and it is worth running before the others even if
your course starts elsewhere — every notebook here takes SMILES as input, and
Stage 1 is where students learn why theirs will not parse.

## What it does not do

- It does not predict metabolism.
- It does not predict whether the molecule binds anything.
- It knows nothing about whether the molecule can be synthesised.
- Stage 2 is pattern matching. SMARTS have no concept of context, and every hit
  needs a chemist to read it.
- A mutagenicity flag is a prediction about an assay outcome, not a property you
  can read off a structure. Real assessment runs through Toxtree, VEGA or a
  commercial system under ICH M7.

## Files

| File | What it is |
|---|---|
| [`structure_validation_and_alerts.ipynb`](structure_validation_and_alerts.ipynb) | The notebook. Open it in Colab with the badge above. |
| [`structure_validation_and_alerts_worksheet.docx`](structure_validation_and_alerts_worksheet.docx) | Student worksheet, five parts. Predict, run, judge the tool, redesign. |

## Teaching notes

The worksheet asks students to commit to a prediction **before** opening the
notebook, and weights the "judging the tool" section at 20%. The closing line says
credit is for the quality of reasoning, not for submitting a molecule with no
alerts — without that, students optimise for a green banner.

Part B gives four deliberately broken structures. Two of them fail for genuinely
different reasons that look identical at a glance, which is the point: `c1ccCcc1`
is a kekulisation failure, `C1=C[CH2]=CC=C1` is a valence failure caused by square
brackets pinning the hydrogen count.

Part C runs the same carbonyl in three environments — plain ketone, aldehyde,
α,β-unsaturated ketone — and asks why only two are flagged. That is the most
useful single exercise in the sheet.
