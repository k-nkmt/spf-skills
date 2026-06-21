# PharmaForest Package Information Policy

## Purpose
This document defines where package information must be fetched from.
Do not maintain a fixed package list in this file.

## Primary Sources
1. Package list and basic metadata (title, category, short description):
   https://github.com/PharmaForest/PharmaForest.github.io/blob/main/repositories.js
2. Package-level details (usage, macros, installation, examples):
   https://github.com/PharmaForest/{PackageName}/blob/main/README.md

Example:
- OncoPlotter details:
  https://github.com/PharmaForest/OncoPlotter/blob/main/README.md

## Retrieval Rules
1. For package list or package count questions:
   read from repositories.js.
2. For package details:
   open that package repository's README.md.
3. If information is not in repositories.js or the package README:
   clearly say the information is not available and do not guess.

## Fallback Rule
If README.md is missing in main branch:
1. check default branch (main/master)
2. check alternative files
3. if still missing, report that detail document is unavailable
