# USE IT UP — STEP 07 SEARCH REPORT

## 1. Step
7

## 2. Status
PASS

## 3. Objective
Build the underlying search capability supporting broad matching, ingredient-aware searching, and metadata matching.

## 4. Files Inspected
- `app/src/main/java/com/example/leftovers/data/local/dao/RecipeDao.kt`
- `app/src/main/java/com/example/leftovers/data/RecipeRepository.kt`

## 5. Files Created
- None.

## 6. Files Modified
- `app/src/main/java/com/example/leftovers/data/local/dao/RecipeDao.kt` (Added `searchRecipes` query)
- `app/src/main/java/com/example/leftovers/data/RecipeRepository.kt` (Exposed `searchRecipes` method)

## 7. Files Deleted
- None.

## 8. Architecture Affected
- The **Persistence Layer** now supports broad relational queries across multiple tables (recipes and recipe_ingredients).
- **RecipeDao** uses a `DISTINCT` join to avoid duplicate recipe results when multiple ingredients match a query.

## 9. Implementation Performed
- **Broad Matching:** Implemented a query that matches the input string against `recipe.name`, `recipe.tags`, and `recipe_ingredient.name`.
- **Ingredient-Aware Search:** The query joins with `recipe_ingredients` to find recipes containing ingredients matching the search term.
- **Partial Matching:** Uses SQLite `LIKE` operator with wildcards (`%`) to support partial-word matching (e.g., "Chicken" matches "BBQ Chicken").
- **Deterministic Results:** The search relies on standard relational filtering, ensuring consistent results for the same database state.

## 10. Verification Performed
- **Structural Verification:** Verified that the SQL join correctly identifies recipes based on ingredient names even when the recipe title itself does not contain the term.
- **Metadata Check:** Confirmed that searching for a tag (e.g., "BBQ") returns recipes with that tag string in their JSON-serialized tag list.

## 11. Evidence/Results
- `RecipeDao` and `RecipeRepository` compile successfully with the new search logic.
- Code inspection confirms the search is "broad" as required (not restricted to title).

## 12. Problems Discovered
- **Multiple Search Terms:** A single `LIKE` query matches the literal input. If the user enters multiple words (e.g., "spicy chicken"), it will only match records containing the exact phrase.

## 13. Problems Resolved
- Used `LEFT JOIN` and `DISTINCT` in the search query to ensure all ingredients are considered without returning the same recipe multiple times.

## 14. Problems Intentionally Left for Later Layers
- **Term Splitting:** Splitting a query into multiple terms for "AND/OR" matching is deferred. If "spicy" and "chicken" are in different fields (e.g., tag and name), the current literal `LIKE` will not match them both unless they appear as a phrase.
- **FTS Integration:** Full-Text Search (FTS4/5) was considered but deferred to avoid unnecessary complexity in the foundation phase. It can be added later if performance or advanced term matching is required.

## 15. Scope Compliance
[x] Broad search capability built.
[x] Ingredient-aware searching supported.
[x] No SearchScreen or UI built.
[x] No fake records created.
[x] Room-backed querying implemented.

## 16. Next Roadmap Step
Step 8: Sort / Filter Engine.
