# USE IT UP — RECIPE DATASET INSPECTION REPORT

**Date:** 2026-08-19
**Dataset Source:** [Wikibooks Cookbook Dataset (Hugging Face)](https://huggingface.co/datasets/gossminn/wikibooks-cookbook)
**File Path:** [recipes_parsed.json](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/Documents/01/raw/recipes_parsed.json)
**Status:** DOWNLOADED & INSPECTED

## 1. Dataset Overview

The dataset is a machine-readable JSON representation of the Wikibooks Cookbook. It was downloaded successfully and placed in the working directory for further processing.

- **Total Records:** 3,895
- **File Size:** ~16.5 MB

## 2. Structure Inspection

Each record in the JSON file follows a consistent nested structure:

```json
{
    "filename": "recipes/recipe_id.html",
    "recipe_data": {
        "url": "https://en.wikibooks.org/wiki/Cookbook:Recipe_Name",
        "title": "Recipe Name",
        "infobox": {
            "category": "Category path",
            "servings": "string",
            "time": "string",
            "difficulty": integer/null
        },
        "text_lines": [
            {
                "text": "line content",
                "line_type": "html_tag",
                "section": "Section Name (e.g., Ingredients, Procedure)"
            }
        ]
    }
}
```

### Key Findings:
- **Ingredients:** Located in `text_lines` where `section == "Ingredients"`.
- **Instructions:** Located in `text_lines` where `section == "Procedure"`, `"Instructions"`, or `"Steps"`.
- **Metadata:** URL, Title, and Category are readily available in the `recipe_data` root.
- **Quantities:** Preserved exactly as found on Wikibooks (e.g., "1 Â½ lemons", "â…“ cup").

## 3. Usability Audit

A brief automated audit was performed to determine the number of records containing both ingredients and step-by-step instructions.

- **Usable Recipes:** 3,717 (95.4% of the dataset)
- **Non-Usable Records:** 178 (Likely category stubs, indices, or incomplete pages)

## 4. Next Steps (Proposed)

1. **Filtering:** Select the top 100-300 recipes matching the MVP priorities (American, Southern, BBQ, etc.).
2. **Cleaning:** Handle encoding issues (e.g., `Â½` for `½`) and normalize the structure into the Use It Up canonical JSON format.
3. **Attribution:** Ensure the Wikibooks URL and CC BY-SA attribution requirements are met in the final seed data.

## 5. Scope Compliance

[x] Downloaded `recipes_parsed.json`.
[x] Placed in non-production working location (`Documents/01/raw/`).
[x] Inspected JSON structure.
[x] Determined number of usable records (3,717).
[x] Original source data preserved.
[x] Reported findings.

**FINAL STATUS:** DATA ACQUISITION COMPLETE. DATASET READY FOR PROCESSING.
