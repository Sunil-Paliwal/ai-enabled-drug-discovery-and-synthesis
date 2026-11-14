# 04 — Retrosynthesis with AiZynthFinder

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis/blob/main/04_aizynthfinder_retrosynthesis/aizynthfinder_retrosynthesis.ipynb)

**Author:** Dr. Sunil Paliwal  
Department of Chemistry and Chemical Biology  
Stevens Institute of Technology

**Can this molecule actually be made, by what synthetic route, and through which reactions?**

## What this module teaches

A structure that passes every validity and liability check in module 02 can still
be unmakeable. This module works backwards from a target to compounds that can be
bought, using [AiZynthFinder](https://github.com/MolecularAI/aizynthfinder): a
neural-network expansion policy proposes disconnections and Monte Carlo tree
search assembles them into routes.

The harder lesson is reading the output sceptically. The models are trained on
reactions extracted from US patents, which are neither a complete nor an unbiased
record of organic chemistry. A route can score 0.99 and rest on a step the model
barely recognises. Deciding which of those is a bad prediction and which is sound
chemistry that patents simply under-record is the judgement the notebook cannot
make for you, and the thing worth arguing about in class.

## What is in it

- **Routes drawn as schemes**, read right to left, with a **green** frame on
  anything found in a catalogue and **orange** on anything that must be made.
- **Reaction names on every step**, from
  [Rxn-INSIGHT](https://doi.org/10.1186/s13321-024-00834-z). The public expansion
  policy ships with its classification column empty, so stock AiZynthFinder can
  name nothing; these names are generated from each reaction's atom-mapped centre.
- **Confidence shown next to score.** Each step carries the policy probability
  `p`, and anything below 0.05 is printed in amber — because the score measures
  how short and how purchasable a route is, and says nothing about whether the
  chemistry works.
- **Which catalogue, not just "in stock".** Every starting material reports where
  it was found, so a current supplier entry is distinguishable from a 2020 one.
- **A PDF report** — scheme, step table and starting materials, one route a page.
- **A ChemDraw bundle** — `route.cdxml` opens as an editable reaction scheme with
  arrows, reaction names and plus signs; plus per-step `.cdxml` and `.rxn` files
  and an SDF of every compound.

## Running it

Run the cells in order. Cells 1 to 3 set the session up once; after that you can
search as often as you like.

| | what it does | how long |
|---|---|---|
| **Cell 1** | installs the search engine and the reaction namer | ~2 min |
| **Cell 2** | downloads the models and the ZINC catalogue (1.3 GB) | ~3–4 min |
| **Cell 3** | writes the support code | instant |
| **Cell 4** | one search | ~2–3 min |

About eight minutes from a cold session to the first set of routes. Nothing is
kept between sessions: if the Colab runtime disconnects, cells 1 to 3 must run
again.

Cell 4 has four settings at the top. `MAX_ROUTES` is how many routes to show;
`MAX_STEPS` is the longest route the search will consider, and AiZynthFinder's
default of 6 is often too short for an elaborate target. Raising it alone will
not help, since a deeper tree is a much bigger tree — raise `TIME_LIMIT` and
`ITERATIONS` with it.

## Catalogues

ZINC ships with AiZynthFinder and is used by default. It tells you a compound was
purchasable *somewhere*, but the bundled snapshot dates from 2020 and carries no
supplier or delivery information, so treat a green frame as "this was buyable five
years ago" rather than "you can order this today". The notebook prints the
catalogue name against every starting material for exactly that reason.

To add your own, convert a supplier catalogue with `build_stock_file.ipynb` and
list it in `EXTRA_STOCKS` at the top of Cell 2. Download any such catalogue under
your own account and on the supplier's own terms; none is distributed here.

Worth knowing when reading the results: catalogues of *novel* building blocks
often omit commodity reagents — piperidine and benzyl bromide may be absent from
one although any lab can buy them in a day, while ZINC has them. Catalogues
complement each other, which is why all of them are reported.

## What this notebook is

AiZynthFinder is used **unmodified**, installed from PyPI at a pinned version.
Every route, score and disconnection comes from it; none of its code is altered or
redistributed here. What this module adds sits downstream of that output:
packaging it to run in Colab, reaction names, per-catalogue availability, the PDF
report and the ChemDraw export.

The one piece of third-party code that *is* modified is the route-drawing layout,
adapted from `rxnutils` (reaction-utils, AstraZeneca, Apache 2.0); the changes are
recorded in the header of the support module, as that licence requires.

Please cite AiZynthFinder as well as this notebook — it does the retrosynthesis.

### Why a separate Python environment?

Colab's default Python is newer than anything AiZynthFinder supports, so a plain
`pip install aizynthfinder` fails with *"No matching distribution found"*. The
notebook installs it into its own Python 3.11 environment (created with
[`uv`](https://github.com/astral-sh/uv)) and runs each search there, which also
makes the notebook resilient to future changes of Colab's default Python.
Everything downstream — naming, drawing, PDF, export — runs in Colab's own Python,
which already has the PyTorch the atom-mapper needs.

## Troubleshooting

**Cell 2 reports that a host was unavailable.** Routine. The models are split
across two public hosts and one of them, figshare, refuses a share of requests;
the cell fetches the missing files from a mirror and carries on.

**Installation or import failures.** Cell 1 resolves dependencies against PyPI so
you keep getting bug and security fixes, but occasionally an unpinned transitive
dependency releases a breaking version — pyarrow 26 dropped NumPy 1.x support
while AiZynthFinder still pins `numpy<2`, producing:

    ImportError: pyarrow requires NumPy 2.0 or newer, found 1.26.4

If that happens, install from the pinned set instead. Fetch it into the session:

    !wget -q https://raw.githubusercontent.com/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis/main/04_aizynthfinder_retrosynthesis/requirements-lock.txt

then replace the `uv pip install` line in Cell 1 with:

    !uv pip install -q --python /content/azf_env/bin/python -r requirements-lock.txt

Those versions were verified on Python 3.11 (linux x86_64) to install, import and
render route images successfully.

**ChemDraw pushes structures off the page.** Set the page to landscape:
File → Page Setup → Landscape, then View → Fit to Window. The schemes are laid out
to fit a landscape page.

## Prerequisites

Assumes a finished design, so realistically modules 01 to 03.

## Setup warning

Unlike the other three modules, AiZynthFinder needs trained policy models and
stock files downloaded separately — a substantial download rather than a
`pip install`. Budget setup time in the session plan and test the download before
teaching from it.

## References

- Genheden, S.; Thakkar, A.; Chadimová, V. *et al.* AiZynthFinder: a fast, robust
  and flexible open-source software for retrosynthetic planning. *J. Cheminform.*
  **2020**, *12*, 70. <https://doi.org/10.1186/s13321-020-00472-1>
- Saigiridharan, L.; Hassen, A. K.; Lai, H. *et al.* AiZynthFinder 4.0:
  developments based on learnings from 3 years of industrial application.
  *J. Cheminform.* **2024**, *16*, 57.
  <https://doi.org/10.1186/s13321-024-00860-x>
- Dobbelaere, M. R.; Lengyel, I.; Stevens, C. V.; Van Geem, K. M. Rxn-INSIGHT:
  fast chemical reaction analysis using bond-electron matrices. *J. Cheminform.*
  **2024**, *16*, 37. <https://doi.org/10.1186/s13321-024-00834-z>

### Licensing

The original AiZynthFinder software is distributed under the MIT License. The
original article is licensed under Creative Commons Attribution 4.0 International,
and its data under the Creative Commons Public Domain Dedication waiver.
