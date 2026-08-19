# Recipe Corpus Integrity Report

**DATE:** 2026-08-19
**STATUS:** COMPLETE

## 1. Authoritative Corpus Summary
The recipe identities for the 300-recipe MVP corpus were compared against the authoritative selection manifest (`wikibooks_selection_manifest.json`).

| Metric | Count |
| :--- | :--- |
| **Authoritative recipes expected** | 300 |
| **Authoritative recipes present** | 300 |
| **Unauthorized recipes present** | 0 |
| **Authoritative recipes missing** | 0 |
| **Duplicate identities** | 0 |

**RESULT: PASS** - The corpus perfectly matches the authoritative selection.

## 2. Classification Rebuild Results
The classification metadata was rebuilt based on actual recipe contents (name, ingredients, instructions) rather than source categories alone.

### Coverage Statistics
- **Total recipes:** 300
- **Average classifications per recipe:** ~5.2
- **Minimum classifications:** 2 (e.g., "Lane Cake")
- **Maximum classifications:** 11 (e.g., "Koshari")
- **Recipes with multiple classifications:** 300
- **Recipes with zero classifications:** 0

### Classification Frequency (Top 20)
| Classification | Count |
| :--- | :--- |
| Vegetables | 244 |
| Meat | 212 |
| Gluten-Free | 173 |
| Chicken | 105 |
| Eggs | 92 |
| Pork | 80 |
| Vegetarian | 80 |
| Beef | 79 |
| Rice | 60 |
| Dessert | 49 |
| Bread | 48 |
| Soup | 47 |
| Potatoes | 38 |
| Stew | 37 |
| African | 33 |
| Pasta | 32 |
| Italian | 30 |
| Fish | 29 |
| BBQ | 22 |
| Seafood | 17 |

## 3. Integrity Check Result
The classification rebuild successfully enriched the metadata while preserving 100% of the recipe content and provenance. The resulting JSON files are verified to be structurally valid and ready for Room ingestion.
