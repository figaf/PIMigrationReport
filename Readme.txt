The migration-assessment app is a single-file web application (migration-assessment.html) for assessing SAP PI/PO integration landscapes and planning migrations to SAP Integration Suite (Cloud Integration / CPI).

What it does
1. Interface Assessment

Upload an export from your SAP PI/PO system. The app reads each Integration Configuration Object (ICO) and runs it through configurable rules to produce a verdict for each interface:

Auto-Migrate — can be migrated automatically using SAP's migration tool
Adjust — needs manual adjustment after migration
Manual — must be rebuilt from scratch
Excluded — out of scope
2. Rule Engine

Two JSON rule files drive the assessment:

assessment-rules.json — maps interface characteristics (mapping type, adapter, etc.) to verdicts
tagging-rules-export.json — assigns tags to interfaces for grouping in Figaf
FIGAF Pricing.txt - Pricing details of Figaf software
Rules.txt File has SAP Migration assessment rules
These files are loaded fresh from disk every time the app opens (via fetch() when served over HTTP).

3. Project View / Wave Planner 

Once interfaces are assessed, the Project View lets you plan the migration project:

Set team size, capacity (interface-pairs per week), hourly rate, and project start date
Interfaces are grouped into waves based on effort capacity
A wave calendar shows real working-day dates for each wave (skipping weekends)
KPI tiles show total effort, consultancy cost, Figaf licence cost, and total project cost
4. Pricing Engine

Figaf licence costs are calculated progressively (like tax bands) from FIGAF Pricing.txt, loaded at startup:

Migration Edition: €100/obj (0–200), €80 (201–500), €70 (501–1,000), €60 (1,000+)
DevOps Suite: €2,000–4,700/yr by iFlow count
5. Exports
Excel export — two-sheet workbook: Project Summary + Interface Register (mirrors exactly what the Project View shows)
Tags CSV — semicolon-separated file (PI object;Tag1) for upload to Figaf, assigning each interface to its wave
Key design decisions
Single HTML file — no build step; publish by copying the .html + three sibling data files to a web server
pvComputeState() — single source of truth; both the UI render and Excel export call it, so they always match
All data files (assessment-rules.json, tagging-rules-export.json, FIGAF Pricing.txt) are loaded fresh on every page open so updates to rules or pricing take effect immediately without touching the HTML