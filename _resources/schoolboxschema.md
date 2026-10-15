# Schoolbox Rubric CSV Schema

**Status:** Production specification (reverse-engineered from controlled exports; partially import-tested)
**Schema version:** 1.1
**Last updated:** 14 October 2026
**Confidence:** High for how Schoolbox *exports* every feature listed in Section 2. Import has been confirmed for one rubric only (see Section 2.2), and Section 10 lists behaviours not yet tested.

## 1. Purpose

This document defines the CSV interchange format for importing and exporting assessment rubrics in Schoolbox. It serves as the authoritative reference for AI agents and developers generating, editing, or validating Schoolbox rubric CSV files.

It synthesises:

1. Schoolbox's public rubric documentation;
2. Four controlled exports from a live Schoolbox instance, generated from one baseline rubric by varying the mark type, mark range setting, and one capability's point values (Section 9); and
3. One successful import of a generated rubric with four capabilities.

## 2. Confirmed Scope

### 2.1 Confirmed by export

Each of the following was set in the Schoolbox UI, saved, and exported, and the export matches this specification:

- comma-separated files encoded without a byte-order mark (BOM), using LF (`\n`) line endings and a trailing LF;
- Row 1 configuration (`capabilityMarkType` and `useMarkRange`) followed by performance level headers;
- `capabilityMarkType` values `raw` (Total Mark), `percent` (Percentage), and `letter:1` (the instance's custom grade scale);
- `useMarkRange` values `On` and `Off`;
- multiple capabilities per rubric, and multiple indicators under one capability;
- automatic capability maxima (sum of indicator maxima) and a custom overridden maximum (scaled total);
- ascending point sequences under `On` and `Off`, and descending sequences under `On` and `Off`;
- `0` as a point value under `On` and `Off`;
- a decimal point value (`1.5`) under `useMarkRange: Off`, and rejection of decimals by the UI under `useMarkRange: On`;
- sparse cells: blank descriptor text with a score, and an unused trailing level (blank text and blank score);
- level header labels, descriptor content, and point direction that disagree with one another (Section 6.7); and
- an embedded comma and line break inside a descriptor (`Test Rubric.csv`).

### 2.2 Confirmed by import

- One generated rubric with four capabilities, one indicator each, five untitled levels, and ascending integer ceilings (`Year_11_English_Language_Analysis_Schoolbox_Rubric.csv`).

Every other feature in Section 2.1 is assumed to import correctly because Schoolbox produces it on export. That assumption is reasonable but untested (Section 10).

### 2.3 Documented by Schoolbox but not tested in CSV files

- Markdown formatting inside descriptors (Section 6.6);
- literal double quotes inside descriptors (standard CSV escaping is assumed); and
- non-ASCII characters such as en dashes, curly quotes, and accented letters. All four test exports contain ASCII characters only, so UTF-8 handling has not been demonstrated.

## 3. Logical Data Model

A Schoolbox rubric maps to four structural objects plus top-level configuration:

```text
Rubric
├── Configuration & Level Headers (Row 1)
│   ├── capabilityMarkType ("raw" | "percent" | "letter:<id>")
│   ├── useMarkRange ("On" | "Off")
│   └── Performance Level Headers [Level 1 .. Level L]   (shared by every indicator)
└── Capability [1..n]
    ├── Capability Name
    ├── Capability Maximum (sum of indicator maxima, or custom scaled total)
    └── Indicator [1..k]
        ├── Indicator Name
        ├── Descriptor Text [Level 1 .. Level L] (optional per cell)
        └── Point Value / Ceiling [Level 1 .. Level L] (optional per cell)
```

Because there is one header row, every indicator in the rubric has the same *L* level columns and shares the same level titles. An indicator with fewer bands leaves its unused levels blank (Section 6.4).

## 4. Physical CSV Structure

For a rubric with *L* performance levels (*L* ≥ 1):

- Every row in the CSV must contain exactly **L + 2** fields (i.e. L + 1 commas per row, not counting commas inside quoted fields).
- **Column 1** belongs to the Capability (storing the Capability Name, the Capability Maximum, or left empty on continuation rows).
- **Column 2** belongs to the Indicator (storing the Indicator Name on descriptor rows, or left empty on numeric rows).
- **Columns 3 to L + 2** correspond to Performance Levels 1 to L.

### 4.1 Row 1: Configuration and Performance Level Headers

```csv
"capabilityMarkType: <type>","useMarkRange: <state>",<Level_1_Name>,<Level_2_Name>,...,<Level_L_Name>
```

**Field 1: `"capabilityMarkType: <type>"`**

| Value | Schoolbox UI option | Notes |
|---|---|---|
| `raw` | Total Mark | Standard default for numerical rubrics. Unlocks the Capability Total field. |
| `percent` | Percentage | |
| `letter:<id>` | Custom grade scale | Observed as `letter:1` in the test instance. The ID refers to a grade scale configured in that instance's administration, so do not assume `1` in other instances. |
| *(not observed)* | None | Selecting None in the UI locks the Capability Total field, so a custom capability total cannot be entered. Its CSV value has not been observed; do not generate it. |

**Field 2: `"useMarkRange: <state>"`**

| Value | Behaviour | Permitted point values |
|---|---|---|
| `On` | Each point value is the upper bound (ceiling) of a mark range for that level | Integers only |
| `Off` | Each point value is a fixed, discrete score for that level | Integers and decimals |

**Fields 3 to L + 2:** Names of the performance levels (e.g. `Low,Med,High` or `"Very Low","Low","Medium","High","Very High"`). If performance levels are untitled in Schoolbox, these fields are left empty (represented by trailing commas).

### 4.2 Capability and Indicator Row Pairs

Every indicator in the rubric is represented by a two-row pair: a **Descriptor Row** immediately followed by a **Numeric Row**.

#### Case A: First indicator of a capability (Indicator 1)

**Descriptor Row**

| Field | Content |
|---|---|
| 1 | Capability Name (non-empty string) |
| 2 | Indicator 1 Name (non-empty string) |
| 3 to L + 2 | Descriptor text for Levels 1 to L (may be empty if a level has no text descriptor) |

**Numeric Row** (immediately following)

| Field | Content |
|---|---|
| 1 | Capability Maximum (numeric). By default, this equals the sum of the highest point values across all indicators belonging to this capability. If a custom value is entered (e.g. `10` when the indicator maximum is `3`), Schoolbox scales the capability to that custom total. |
| 2 | Empty |
| 3 to L + 2 | Point values/ceilings for Indicator 1 across Levels 1 to L (may be empty if a level is not used for this indicator) |

#### Case B: Subsequent indicators under the same capability (Indicators 2..k)

When a capability contains two or more indicators, each additional indicator is written directly below the preceding indicator's numeric row, using empty Field 1 cells on both rows.

**Descriptor Row**

| Field | Content |
|---|---|
| 1 | Empty (signals that this indicator belongs to the current capability) |
| 2 | Indicator Name (non-empty string) |
| 3 to L + 2 | Descriptor text for Levels 1 to L |

**Numeric Row** (immediately following)

| Field | Content |
|---|---|
| 1 | Empty (the Capability Maximum is declared only once, on Indicator 1's numeric row) |
| 2 | Empty |
| 3 to L + 2 | Point values/ceilings for this indicator across Levels 1 to L |

## 5. Formal Row Grammar (EBNF)

```ebnf
file               = header_row, LF, capability_block, { LF, capability_block }, LF ;

header_row         = cap_mark_setting, ",", range_setting, ",", level_header_list ;
cap_mark_setting   = '"capabilityMarkType: ', ( "raw" | "percent" | letter_id ), '"' ;
range_setting      = '"useMarkRange: ', ( "On" | "Off" ), '"' ;
level_header_list  = text_or_empty, { ",", text_or_empty } ;

capability_block   = first_ind_pair, { LF, subseq_ind_pair } ;

first_ind_pair     = first_desc_row, LF, first_num_row ;
first_desc_row     = capability_name, ",", indicator_name, ",", descriptor_list ;
first_num_row      = capability_max, ",", empty, ",", point_list ;

subseq_ind_pair    = subseq_desc_row, LF, subseq_num_row ;
subseq_desc_row    = empty, ",", indicator_name, ",", descriptor_list ;
subseq_num_row     = empty, ",", empty, ",", point_list ;

descriptor_list    = text_or_empty, { ",", text_or_empty } ;
point_list         = number_or_empty, { ",", number_or_empty } ;

(* Terminals *)
capability_name    = csv_text ;                     (* non-empty *)
indicator_name     = csv_text ;                     (* non-empty *)
capability_max     = number ;
letter_id          = "letter:", digit, { digit } ;
text_or_empty      = [ csv_text ] ;
number_or_empty    = [ number ] ;
number             = digit, { digit }, [ ".", digit, { digit } ] ;
empty              = "" ;                           (* zero characters between delimiters *)
csv_text           = (* any CSV field, quoted per Section 7 *) ;
LF                 = "\n" ;
```

**Constraint:** Every `level_header_list`, `descriptor_list`, and `point_list` in the file must contain exactly *L* elements.

## 6. Scoring, Sequencing, and Cell Rules

### 6.1 `useMarkRange: On` vs `useMarkRange: Off`

- **`useMarkRange: On`:** All point values must be integers. Entering `3.5` with mark ranges on blocked saving in the UI with the error: *"You cannot have descriptors with decimal places if using mark range"*. (How the importer responds to a decimal under `On` has not been tested; never generate one.)
- **`useMarkRange: Off`:** Point values may be integers or decimals (e.g. `1.5`, `2.5`).

### 6.2 Sequence direction (ascending and descending)

- Point values across an indicator row may run low-to-high (ascending, e.g. `0, 2, 4, 6, 8, 10`) or high-to-low (descending, e.g. `10, 8, 6, 4, 2, 0`), regardless of whether `useMarkRange` is `On` or `Off`.
- Indicators in the same rubric may run in different directions. In the test exports, Cap B ran `3,2,1` while Caps A and C ran ascending.
- When `useMarkRange: On`, non-empty point values across an indicator should be strictly monotonic (either increasing or decreasing). *(Inferred from how mark ranges work; not yet directly tested. Always generate monotonic sequences.)*

### 6.3 Zero point values

`0` is a valid point value/ceiling in both modes. For example, under `useMarkRange: On`, ceilings of `0, 2, 3` are expected to mean Level 1 awards 0 marks, Level 2 awards 1–2 marks, and Level 3 awards 3 marks (see Section 10, item 1).

### 6.4 Sparse cells and shorter indicators

A level within an indicator may have:

- descriptor text with a point value;
- a point value with blank descriptor text; or
- both descriptor text and point value left blank (when an indicator uses fewer bands than *L*).

Only an unused **trailing** level (the last column) has been tested. Blank leading or middle levels have not.

**Minimum requirement per indicator:** at least one level with a descriptor, a point value, or both. *(Inferred; not yet directly tested.)*

### 6.5 Capability maximum calculation

For a capability with indicators 1…*k*, let *Mᵢ* be the highest numeric point value on indicator *i*'s numeric row.

- **Standard (unscaled) Capability Maximum:** Capability Maximum = *M₁* + *M₂* + … + *Mₖ*
- **Custom (scaled) Capability Maximum:** any positive number placed in Field 1 of Indicator 1's numeric row (e.g. `10` when the sum of *Mᵢ* is `3`). Schoolbox scales the capability to this total.

Schoolbox cannot tell a deliberate custom total from an arithmetic mistake. A wrong sum in Field 1 will be accepted as a scaled total rather than rejected, so generators must calculate this value independently and check it.

### 6.6 Markdown formatting inside descriptors

Schoolbox's documentation states that descriptor fields support Markdown:

| Syntax | Result |
|---|---|
| `# Heading` | Heading |
| `**bold text**` | Bold |
| `*italic text*` | Italics |
| `1. Item` | Ordered list |
| `- Item` | Bullet list |
| `[title](https://www.example.com)` | Hyperlink |

Do **not** prepend apostrophes (`'`) to descriptor lines starting with `-` (bullet points), as this breaks Schoolbox Markdown rendering.

### 6.7 Labels, descriptors, and point direction are not cross-checked

Schoolbox does not check that level header labels, descriptor content, and point values agree. In `Test 2.csv`, `Qs 1-2.csv`, and `Q3.csv`, Cap B's levels are headed `Low,Med,High` and its descriptors read `1,2,3`, but its points run `3,2,1` (or `3,2,1.5`), so the level headed "Low" awards the most marks. Schoolbox saved and exported this without complaint.

A generator must therefore keep headers, descriptors, and points consistent with the source itself. Schoolbox will not catch a reversed or misaligned row.

## 7. CSV Dialect

| Property | Rule |
|---|---|
| Delimiter | Comma (`,`) |
| Quote character | Double quote (`"`) |
| Escape convention | Doubled double quotes (`""`) inside quoted fields |
| Encoding | UTF-8 without BOM |
| Line endings | LF (`\n`), including a trailing LF after the final row |

**Quoting rules:**

- Schoolbox's own exports quote only text containing a space or other special character: `"Cap A"` and `"Ind A1"` are quoted, while `Low`, `Med`, `High`, and single-digit descriptor text are not.
- When generating, quote all non-empty text fields containing spaces, commas, double quotes, or line breaks. Quoting every non-empty text field (including single-word level headers) is also safe and valid.
- Leave empty fields completely empty: write nothing between the delimiters (`,,`), not a pair of quotes (`,"",`).
- Write numeric point values and capability maxima without quotes (e.g. `0`, `9`, `1.5`).

## 8. Worked Examples

### 8.1 Multi-indicator, custom total, and sparse cells (`Test 1(Final).csv`, verbatim)

*The descriptor cells in all test files contain placeholder text that happens to be numeric (e.g. `1,2,3`). In a real rubric these would be quoted descriptor sentences.*

```csv
"capabilityMarkType: raw","useMarkRange: On",Low,Med,High
"Cap A","Ind A1",1,2,3
9,,1,2,3
,"Ind A2",2,4,6
,,2,4,6
"Cap B","Ind B1",1,2,3
10,,1,2,3
"Cap C","Ind C1",1,,3
5,,0,2,3
,"Ind C2",1,2,
,,1,2,
```

- **Row 1:** Sets raw marks, `useMarkRange: On`, and three level headers (`Low`, `Med`, `High`).
- **Cap A:** Contains two indicators (Ind A1 max 3; Ind A2 max 6). The automatic capability total of 9 (3 + 6) appears in Row 3, Field 1. Ind A2 leaves Field 1 blank on both its descriptor row (Row 4) and numeric row (Row 5).
- **Cap B:** Contains one indicator with a maximum of 3, manually overridden to a custom capability total of 10 (Row 7, Field 1).
- **Cap C:** Ind C1 has a `0` point value at Level 1 and blank descriptor text with a score at Level 2. Ind C2 has an unused third level (blank text and blank points). The automatic capability total is 5 (3 + 2).
- **Rubric total:** 9 + 10 + 5 = 24.

### 8.2 Descending decimal scores under mismatched headers (`Q3.csv`, verbatim)

```csv
"capabilityMarkType: letter:1","useMarkRange: Off",Low,Med,High
"Cap A","Ind A1",1,2,3
9,,1,2,3
,"Ind A2",2,4,6
,,2,4,6
"Cap B","Ind B1",1,2,3
10,,3,2,1.5
"Cap C","Ind C1",1,,3
5,,0,2,3
,"Ind C2",1,2,
,,1,2,
```

- **Row 1:** Custom grade scale (`letter:1`) with discrete scores (`useMarkRange: Off`).
- **Cap B (Rows 6–7):** Points run high-to-low and include a decimal (`1.5`), which is permitted only because `useMarkRange` is `Off`. The headers still read `Low,Med,High`, so this row deliberately contradicts its labels (Section 6.7).
- Everything else is identical to Section 8.1.

## 9. Evidence Register

### 9.1 Controlled exports

All four exports come from one baseline rubric (3 capabilities, 5 indicators, 3 levels titled `Low`, `Med`, `High`, 24 marks in total). They are byte-identical apart from Row 1 and Row 7 (Cap B's points). Each is 11 rows of 5 fields, ASCII only, with no BOM, LF line endings, and a trailing LF.

| File | Bytes | Mark type (UI → CSV) | Mark range | Cap B points | What it adds |
|---|---|---|---|---|---|
| `Test 1(Final).csv` | 200 | Total Mark → `raw` | On | `1,2,3` | Baseline: level headers, multi-indicator grouping, automatic and custom capability totals, `0` value, blank descriptor with score, unused trailing level |
| `Test 2.csv` | 205 | Percentage → `percent` | Off | `3,2,1` | `percent`; `Off`; descending sequence |
| `Qs 1-2.csv` | 205 | Custom grade scale → `letter:1` | On | `3,2,1` | `letter:1`; descending sequence under `On` |
| `Q3.csv` | 208 | Custom grade scale → `letter:1` | Off | `3,2,1.5` | Decimal under `Off` |

**UI findings recorded while building `Test 1(Final).csv`:**

- Capability mark type **None** locks the Capability Total field; switching to Total Mark unlocks it.
- A decimal (`3.5` on Ind C1, Level 3) with mark range **On** blocks saving with the error quoted in Section 6.1.

### 9.2 Other files

| File | Type | What it confirmed |
|---|---|---|
| `Test Rubric.csv` | Export | 1 capability, 1 indicator, 3 untitled levels, `raw`, `useMarkRange: On`, embedded comma and line break in a descriptor |
| `Year_11_English_Language_Analysis_Schoolbox_Rubric.csv` | Generated and imported | 4 capabilities, 1 indicator per capability, 5 untitled levels, `2,4,6,8,10` ceilings, 40 marks total |

## 10. Known Gaps (Not Yet Tested)

Treat the following as unverified. Prefer conservative output that avoids depending on them.

1. **Import of exported features.** Multi-indicator capabilities, level headers, custom totals, descending sequences, decimals, sparse cells, `percent`, and `letter:<id>` have been seen on export only. Re-importing the four test files would confirm them.
2. **Non-ASCII characters.** En dashes, curly quotes, accented letters, and emoji have not appeared in any test file, so UTF-8 import has not been demonstrated. Word and PDF rubrics routinely contain these characters.
3. **Literal double quotes and Markdown in imported descriptors.**
4. **Lower bound of the first mark range.** Under `useMarkRange: On`, it is not confirmed whether the lowest band's range starts at 0 or at 1. A source band of `1–2` mapped to a ceiling of `2` may therefore permit a mark of 0 in Schoolbox.
5. **Blank leading or middle levels.** Only a blank trailing level has been tested.
6. **Rejection rules:** non-monotonic sequences under `useMarkRange: On`, indicators with no content, and decimals under `On` when imported rather than typed into the UI.
7. **Decimal or very large custom capability maxima:** only an integer custom total (10) has been tested.
8. **The None mark type:** its CSV value, and whether imported capability totals are ignored under it.
9. **Effect of capability maxima under `percent` and `letter:<id>`:** the custom total of 10 is preserved on export in both modes, but how it affects scoring has not been checked.
10. **Trailing LF and BOM tolerance on import:** files without a trailing LF, or with a BOM, have not been tested.
