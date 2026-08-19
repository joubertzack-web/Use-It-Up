# USE IT UP — STEP 13 RECOMMENDATION REPORT

## 1. Step
13

## 2. Status
PASS

## 3. Objective
Build a deterministic recommendation engine that ranks recipes based on pantry inventory, leftovers, and user favorites.

## 4. Files Inspected
- `app/src/main/java/com/example/leftovers/domain/engine/MatchingEngine.kt`
- `app/src/main/java/com/example/leftovers/domain/model/Recipe.kt`

## 5. Files Created
- `app/src/main/java/com/example/leftovers/domain/model/RecommendedRecipe.kt`
- `app/src/main/java/com/example/leftovers/domain/engine/RecommendationEngine.kt`

## 6. Files Modified
- None.

## 7. Files Deleted
- None.

## 8. Architecture Affected
- The **Domain Engine** layer now supports a multi-input scoring system for recipe ranking.
- Introduced `RecommendedRecipe` to encapsulate both the recipe data and its ranking metadata (score, match details).

## 9. Implementation Performed
- **Scoring System:** Established explicit, deterministic weights:
    - Ingredient Match: +10 pts
    - Leftover Match: +50 pts
    - Favorite Affinity: +30 pts
- **Top 5 Logic:** The engine identifies the Top 5 results with a score > 0 and marks them as `isTopRecommendation`.
- **Match Visibility:** Each recommended recipe includes a `matchDetails` map, allowing the UI to explain *why* it was recommended (e.g., "Contains leftover chicken").
- **Deterministic Ranking:** Sorting is based strictly on the calculated score, ensuring stable results for the same input state.

## 10. Verification Performed
- **Logic Validation:** Verified that a recipe with a leftover match scores higher than a simple pantry match, adhering to the "use it up" priority.
- **Top 5 Check:** Confirmed that the engine correctly identifies the top 5 records in a ranked list.
- **Independence:** Verified that search relevance is not factored into this recommendation score, maintaining the separation required by the audit.

## 11. Evidence/Results
- `RecommendationEngine` compiles successfully.
- Code inspection confirms no hidden weights or hardcoded recipe lists.

## 12. Problems Discovered
- None.

## 13. Problems Resolved
- Used the `MatchingEngine` (Step 12) as the foundation for scoring, ensuring that the recommendation engine remains focused on ranking rather than low-level ingredient comparison.

## 14. Problems Intentionally Left for Later Layers
- **UI Presentation:** The "★ indicator" for Top 5 is deferred to the UI implementation phase.

## 15. Scope Compliance
[x] Deterministic recommendation engine implemented.
[x] Explicit scoring system established.
[x] No hidden weights.
[x] Top 5 logic implemented.
[x] No UI built.

## 16. Next Roadmap Step
Step 14: Grocery Requirement Engine.
