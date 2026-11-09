# 03 — Bioisostere Explorer

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Sunil-Paliwal/ai-enabled-drug-discovery-and-synthesis/blob/main/03_bioisostere_explorer/bioisostere_explorer.ipynb)

**Author:** Dr. Sunil Paliwal, Department of Chemistry and Chemical Biology,
Stevens Institute of Technology

**Replace a functional group without losing the interaction it makes.**

## What this module teaches

Almost every group that binds well also brings a liability. A carboxylic acid makes
an excellent salt bridge and is ionised at every physiological pH, which costs
permeability and usually rules out the brain. A bioisostere lets you keep the
interaction and drop the liability, or trade it for one you can live with — the
classic swap being acid for tetrazole: different atoms, same salt bridge, better
behaviour.

Students pick the pharmacophore role the group plays — anion, cation,
donor/acceptor, acceptor, aryl or lipophilic — and the notebook returns candidate
replacements with the properties that matter for choosing between them.

## Prerequisites

Assumes **01** and **02**.

From 01: the notebook asks which pharmacophore role a group plays, which is 01's
content.

From 02: what motivates a replacement is a liability, and module 02 is where
students find theirs. Module 02 points directly here when it flags a group that
has known replacements — an aldehyde, for instance.

## What it does not do

The replacement tables are curated chemistry, not a model. They will not suggest
anything nobody has written down, and a replacement that is right in one binding
site can be wrong in another. A bioisostere is a hypothesis to test, not a fix.

## Files

| File | What it is |
|---|---|
| [`bioisostere_explorer.ipynb`](bioisostere_explorer.ipynb) | The notebook. Open it in Colab with the badge above. |
| [`bioisostere_explorer_worksheet.docx`](bioisostere_explorer_worksheet.docx) | Student worksheet. |
