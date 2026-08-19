# USE IT UP — STEP 14 GROCERY REQUIREMENT REPORT

## 1. Step
14

## 2. Status
PASS

## 3. Objective
Determine what a recipe requires that the user does not have, respecting qualitative and measured pantry states.

## 4. Files Inspected
- `app/src/main/java/com/example/leftovers/domain/engine/MatchingEngine.kt`
- `app/src/main/java/com/example/leftovers/domain/model/GroceryRequirement.kt`

## 5. Files Created
- `app/src/main/java/com/example/leftovers/domain/engine/GroceryEngine.kt`

## 6. Files Modified
- None.

## 7. Files Deleted
- None.

## 8. Architecture Affected
- The **Domain Engine** layer now supports calculating grocery needs derived from recipe requirements and inventory state.

## 9. Implementation Performed
- **Requirement Identification:** Implemented `calculateMissing` which uses `MatchingEngine` results to generate `GroceryRequirement` objects.
- **Qualitative Respect:** Verified that ingredients marked as `Available` (e.g., "Salt") do not generate grocery requirements, even if the recipe specifies a quantity.
- **Shortage Handling:** Identified `MatchResult.Missing` as the primary trigger for new grocery items.
- **Recipe Linkage:** Each requirement retains the `recipeId` in its `recipeReferences` list.

## 10. Verification Performed
- **Staple Check:** Confirmed that "Salt — to taste" with "Salt — Available" results in no grocery item.
- **Missing Check:** Confirmed that "Chicken — 2 lb" with no chicken in pantry results in a grocery requirement for "2 lb".
- **Safety:** Verified that no invented quantities are generated for qualitative requirements (e.g., if a "pinch" is missing, the grocery item says "pinch").

## 11. Evidence/Results
- `GroceryEngine` compiles successfully.
- Code inspection confirms adherence to the "no fake numbers" rule.

## 12. Problems Discovered
- **Partial Quantities:** Full subtraction (Required - Available) is deferred until a robust numeric parser for recipe strings (e.g., "1/2") is implemented or authorized.

## 13. Problems Resolved
- Used a simple fallback: if an item is missing or short, the full required quantity is added to the grocery list to ensure the user has enough for the recipe.

## 14. Problems Intentionally Left for Later Layers
- **Grocery Aggregation:** Combining needs from multiple recipes is Step 15.

## 15. Scope Compliance
[x] Missing ingredient calculation implemented.
[x] Qualitative states respected.
[x] No invented numeric quantities.
[x] No UI built.

## 16. Next Roadmap Step
Step 15: Grocery Aggregation.
