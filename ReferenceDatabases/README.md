# How to Create a Reference Database

A reference database is a collection of biological sequences (DNA/RNA) paired with information about each organism. This guide walks you through creating one from scratch, even if you have little technical experience.

---

## What You Need to Create

Every reference database is a folder containing exactly three files:

| File | What it contains |
|------|-----------------|
| `data.tsv` | A spreadsheet-like table with one row per sequence |
| `sequences.fasta` | The actual DNA/RNA sequences in a standard text format |
| `metadata.yaml` | General information describing the whole database |

---

## File 1 — `data.tsv` (the sequence table)

This is a plain text file where columns are separated by **tabs** (not commas). You can create or edit it in a spreadsheet program like Excel or Google Sheets, then export it as a tab-delimited `.tsv` file.

### Required columns

The first row must be a header row with exactly these column names (in any order):

| Column | Description | Example |
|--------|-------------|---------|
| `ID` | A unique identifier for each sequence | `NR_201430` or `11642` |
| `accessionNumber` | The GenBank/database accession number | `NR_201430` |
| `scientificName` | The full scientific name of the organism | `Tricholoma magnivelare` |
| `taxonRank` | The taxonomic rank of the scientific name | `species` |
| `higherClassification` | Full taxonomy from broadest to narrowest, separated by `;` | `Eukaryota;Fungi;Ascomycota;...;Genus;Species_name` |
| `basisOfRecord` | How the specimen was recorded | `materialSample`, `preservedSpecimen`, or `specimen` |
| `decimalLatitude` | Latitude where the organism was collected (can be empty) | `48.8566` |
| `decimalLongitude` | Longitude where the organism was collected (can be empty) | `2.3522` |
| `typeStatus` | Whether this is a type specimen (can be empty) | `HOLOTYPE`, `EPITYPE` |
| `catalogueNumber` | Museum or collection identifier (can be empty) | `NYS:f2421` |
| `identifiedBy` | Who identified the organism (can be empty) | `J. Smith` |
| `country` | Country of collection (can be empty) | `France` |
| `locality` | Specific location of collection (can be empty) | `Lyon` |

> **Tip:** Columns that can be empty should still appear in the header — just leave the cell blank for rows where that information is not available.

### Example rows

```
ID	accessionNumber	scientificName	decimalLatitude	decimalLongitude	typeStatus	catalogueNumber	identifiedBy	taxonRank	country	locality	basisOfRecord	higherClassification
NR_201430	NR_201430	Tricholoma magnivelare			HOLOTYPE	NYS:f2421		species			materialSample	Eukaryota;Fungi;Basidiomycota;Agaricomycetes;Agaricales;Tricholomataceae;Tricholoma;Tricholoma_magnivelare
NR_201429	NR_201429	Tricholoma matsutake			EPITYPE	TNS:F-82226		species			materialSample	Eukaryota;Fungi;Basidiomycota;Agaricomycetes;Agaricales;Tricholomataceae;Tricholoma;Tricholoma_matsutake
```

> **Important:** The `ID` column must match the sequence identifiers in your `sequences.fasta` file (see below).

---

## File 2 — `sequences.fasta` (the sequence file)

A FASTA file is a plain text file that stores biological sequences. Each entry has two parts:

1. A **header line** starting with `>` followed by the sequence ID
2. The **sequence itself** on the next line(s)

### Example

```
>NR_201430
GGTGAACCTGCGGAAGGATCATTATTGAATAAAGCTTGGTTAGGTTGTTGCTGGCTCTCC...
>NR_201429
GGTGAACCTGCGGAAGGATCATTATTGAATAAAGCTTGGTTAGGTTGTTGCTGGCTCTCC...
```

### Rules to follow

- The ID after `>` must **exactly match** the `ID` column in `data.tsv`.
- Write sequences in **uppercase** letters.
- Each record in `data.tsv` must have a matching sequence in `sequences.fasta`, and vice versa.

---

## File 3 — `metadata.yaml` (the database description)

This file describes the entire database: what it covers, who made it, when it was released, and so on. It uses a format called YAML, which is designed to be human-readable.

### Full example

```yaml
title: My Fungal ITS Reference Database
targetGene: ITS
taxonRanks: kingdom;phylum;class;order;family;genus;species
description: |-
  A curated collection of ITS sequences for fungal identification.
  Sequences were obtained from type specimens deposited at international
  herbaria and verified by expert taxonomists.
issued: '2025-06-01'
version: 1.0.0
contact:
  given: Jane
  family: Smith
  email: jane.smith@university.edu
creator:
  - given: Jane
    family: Smith
    orcid: 0000-0000-0000-0001
  - given: John
    family: Doe
    organization: Natural History Museum
keyword:
  - ITS
  - fungi
  - type specimens
geographicScope: global
taxonomicScope:
  - Fungi
licence: CC-BY 4.0
url: https://example.com/my-database
```

### Field reference

| Field | Required | Description |
|-------|----------|-------------|
| `title` | Yes | Full name of the database |
| `targetGene` | Yes | The gene or marker sequenced (e.g., `ITS`, `16S rRNA`, `18S rRNA`) |
| `taxonRanks` | Yes | Taxonomic levels used in `higherClassification`, separated by `;` |
| `description` | Yes | A plain-language description of the database and its contents |
| `issued` | Yes | Release date in `'YYYY-MM-DD'` format |
| `version` | Yes | Version number or identifier |
| `contact` | Yes | The main person to contact about the database |
| `creator` | Yes | List of people or organizations who built the database |
| `keyword` | No | Tags to help others discover the database |
| `geographicScope` | No | Geographic coverage (e.g., `global`, `Europe`) |
| `taxonomicScope` | No | Which groups of organisms are included |
| `licence` | No | The license under which the data is shared (e.g., `CC-BY 4.0`, `MIT`) |
| `url` | No | Website or link to the original data source |
| `logo` | No | URL to an image/logo for the database |

> **YAML formatting tips:**
> - Indentation matters — use spaces (not tabs) for nested items.
> - Dates must be wrapped in single quotes: `'2025-06-01'`.
> - For multi-line descriptions, start with `|-` and indent the text below it.
> - List items start with `- ` (a dash followed by a space).

---

## Final checklist

Before you consider your database complete, verify the following:

- [ ] `data.tsv` has all required column headers in the first row
- [ ] Every row in `data.tsv` has a unique value in the `ID` column
- [ ] Every `ID` in `data.tsv` has a matching `>ID` entry in `sequences.fasta`
- [ ] Every sequence in `sequences.fasta` has a matching row in `data.tsv`
- [ ] Sequences in `sequences.fasta` are in uppercase
- [ ] `higherClassification` values use `;` as separator with no spaces around it
- [ ] `metadata.yaml` includes at least: `title`, `targetGene`, `taxonRanks`, `description`, `issued`, `version`, `contact`, and `creator`
- [ ] All three files are saved in the same folder

---

## Looking at real examples

The `example-datasets/` folder in this repository contains several complete reference databases you can use as a reference:

| Folder | Organism group | Gene |
|--------|---------------|------|
| `pr2` | Protists | 18S rRNA |
| `refSeq-ITS` | Fungi | ITS |
| `refSeq-bac16s` | Bacteria | 16S rRNA |
| `unite` | Fungi | ITS |

Open any of these folders and compare the three files to understand how they fit together.
