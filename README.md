# AI-Enabled Drug Discovery and Synthesis

**Author:** Dr. Sunil Paliwal, Department of Chemistry and Chemical Biology,
Stevens Institute of Technology
**Status:** Developed for teaching and research in drug discovery and medicinal
chemistry.

Four Colab notebooks and their worksheets for computer-aided molecular design,
built for a course but usable on their own. Each module answers one question a
designer actually hits, in the order they hit it:

**What makes it bind → is this even a real molecule, and is it safe → how do I fix
what is wrong without losing the binding → can anyone make it?**

Everything runs in Google Colab. There is nothing to install and no account beyond
a Google one. Each notebook installs what it needs in its first cell.

**How much of this is actually AI?** Worth saying plainly, since the title leads
with it. Modules 01 to 03 are rule-based cheminformatics — substructure patterns,
pharmacophore logic and curated replacement tables, all built on RDKit. Module 04
uses a trained policy model. The AI framing describes the workflow these notebooks
support, not the machinery inside each one, and knowing which problems need a model
and which a SMARTS pattern will solve is part of the subject.

---

## The modules

| # | Module | What it teaches | Standalone? | Open |
|---|---|---|---|---|
| 01 | [Molecular Interactions](01_molecular_interactions/) | What makes two molecules stick together, and how strong each kind of contact is | Yes | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis/blob/main/01_molecular_interactions/molecular_interactions.ipynb) |
| 02 | [Structure Validation and Alerts](02_structure_validation_and_alerts/) | Whether a drawn structure is a valid molecule at all, and whether it will survive or react with everything | Yes | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis/blob/main/02_structure_validation_and_alerts/structure_validation_and_alerts.ipynb) |
| 03 | [Bioisostere Explorer](03_bioisostere_explorer/) | Replacing a problem group while keeping the interaction it makes | Assumes 01 and 02 | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis/blob/main/03_bioisostere_explorer/bioisostere_explorer.ipynb) |
| 04 | [Retrosynthesis with AiZynthFinder](04_aizynthfinder_retrosynthesis/) | Whether a designed molecule can actually be made, and by what route | Assumes a finished design | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis/blob/main/04_aizynthfinder_retrosynthesis/aizynthfinder_retrosynthesis.ipynb) |

Modules 01 and 02 can be taught on their own in a single session. Module 03 assumes
both: it asks students to name the pharmacophore role a group plays, which is 01's
content, and the liability that motivates replacing it, which is 02's. Module 02
points students directly at 03 when it flags a group that has known replacements.

---

## For students

Click the Colab badge for the module you are on. Run the cells in order — the green
bars mark the steps you have to follow, and only the cells tagged **you edit this
one** need anything from you.

Each module has a worksheet (`.docx`) to complete alongside the notebook. Most
notebooks generate a PDF report you attach when you submit.

**If your SMILES will not parse**, module 02 explains why and points at the atom
responsible. That is worth running first even if your course starts with module 01,
because every notebook here takes SMILES as input.

## For research use

These are not only coursework. Module 02 has a **batch mode** built for auditing
generated structures: point it at a file of SMILES and it reports the validity
rate *and* which failure modes dominate, rather than the single percentage that
generative-model benchmarks usually quote. That distinction — a model emitting
chemically wrong molecules versus one emitting broken strings — is what you need
to act on a bad validity score.

Module 03's replacement tables and module 04's retrosynthesis search are usable
directly in design work. Modules 01 and 02 are also a reasonable pre-filter in
front of a docking or scoring pipeline, since both run fast and need no setup.

## For instructors

Each module folder holds the notebook, the student worksheet, and a README saying
what the module assumes and what it does not cover. The worksheets are structured
the same way throughout: predict before you run, record what happened, then judge
the tool — the last section is deliberately weighted, because the point is for
students to be able to say what the software cannot tell them.

You are welcome to adapt these. See the licence section below.

---

## Before you trust anything here

These notebooks can be wrong, and the useful thing is knowing *how*.

**The rule-based modules (01–03) match patterns, and a pattern has no judgement.**
It fires on a substructure whether or not the surrounding chemistry makes it a real
problem, and it stays silent on anything nobody thought to write a pattern for. So
the errors run in both directions: groups flagged that are perfectly fine in
context, and genuine problems passed over in silence. **A clean result means
nothing matched — not that nothing is wrong.**

**The model-based module (04) predicts, and predictions fail.** A proposed route
can look reasonable and not work at the bench; a molecule the model cannot route
may be straightforward to make.

**Molecules designed with generative or language models arrive unverified.** That
is much of the reason module 02 exists — but confirming a structure parses is not
the same as confirming it makes sense.

**What none of them do:** predict whether a molecule is active, selective or safe;
predict metabolism; or tell you whether a synthesis is worth attempting. Alerts are
heuristics, and several modules deliberately show approved drugs tripping them.

Treat every output as a reason to think, not an answer to accept. Nothing here
replaces a chemist reading the result.

---

## Licence

- **Code** (notebooks, scripts): MIT — see [LICENSE](LICENSE)
- **Teaching material** (worksheets, written explanations): CC BY 4.0 — see
  [LICENSE-CONTENT](LICENSE-CONTENT)

Adapt the worksheets for your own course; attribution is all that is asked.

## How to cite

If you use these notebooks in teaching or publications, please cite:

> Paliwal, S. *AI-Enabled Drug Discovery and Synthesis.* GitHub, 2026.
> https://github.com/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis

See also [CITATION.cff](CITATION.cff), or the "Cite this repository" button in the
sidebar.

## Built on

[RDKit](https://www.rdkit.org/) for all cheminformatics, and the published
substructure sets it ships: PAINS (Baell & Holloway, *J. Med. Chem.* **2010**, 53,
2719), the Brenk unwanted-substructure list (*ChemMedChem* **2008**, 3, 435), and
the NIH filters. Mutagenicity annotations follow the toxicophores of Kazius,
McGuire & Bursi (*J. Med. Chem.* **2005**, 48, 312) and the Benigni–Bossa rulebase.
[TeachOpenCADD](https://projects.volkamerlab.org/teachopencadd/) is the companion
worth reading for library-scale filtering.
