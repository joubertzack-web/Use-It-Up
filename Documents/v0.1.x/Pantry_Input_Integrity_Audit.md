# Use It Up — Pantry Ingredient Entry / Input Integrity Audit

**DATE:** 2026-08-19  
**PHASE:** Audit / Investigation  
**STATUS:** **AUDIT ONLY** — NO CODE MODIFIED

---

## 1. CURRENT INPUT FLOW
*   **UI Layer**: `AddPantryItemDialog` in `PantryScreen.kt` uses a standard `OutlinedTextField` for the `name` parameter.
*   **Logic Layer**: When the user clicks "Add", the `PantryViewModel#addPantryItem(name: String, ...)` function is invoked with the raw string from the text field.
*   **Data Flow**: 
    1.  `PantryViewModel` checks the `IngredientRepository` for the name.
    2.  If `null`, it calls `ingredientRepository.saveIngredient(Ingredient(name = name, ...))`.
    3.  `IngredientRepository` inserts a new row into the `ingredients` table.
    4.  The new `ingredientId` is then used to create a `PantryEntity` record.

## 2. CURRENT INGREDIENT IDENTITY FLOW
*   **Identity Source**: The identity is derived **exclusively** from the user's keystrokes.
*   **Persistence**: Typos (e.g., "Chickenn") are permanently stored in the `ingredients` table as first-class culinary identities.
*   **ID Resolution**: The system performs a case-insensitive name lookup, but lacks any fuzzy matching or restricted selection during entry.

## 3. CURRENT QUANTITY/UNIT FLOW
*   **Model**: `PantryQuantity` (Sealed class: `Measured`, `Available`, `NotAvailable`).
*   **Input**: `AddPantryItemDialog` uses a free-text `OutlinedTextField` for the `unit` field.
*   **Consistency**: There is no validation to ensure the unit (e.g., "pcs") is appropriate for the selected ingredient.

## 4. EXACT SOURCE OF THE "Chickenn" FAILURE
*   **File**: `app/src/main/java/com/joubertzack/useitup/ui/screens/pantry/PantryViewModel.kt`
*   **Lines 29-33**: The implementation explicitly allows "Create-on-Miss" behavior:
    ```kotlin
    if (ingredient == null) {
        val id = ingredientRepository.saveIngredient(Ingredient(name = name, category = category))
        ingredient = ingredientRepository.getIngredientById(id)
    }
    ```
*   **Consequence**: This bypasses the **Canonical Vocabulary** entirely, polluting the relational database with garbage data that will never match a recipe.

## 5. CANONICAL VOCABULARY INTEGRATION STATUS
*   **Status**: **PARTIAL / DISCONNECTED**.
*   **Observation**: While the 300-recipe corpus was audited to create a canonical JSON and the `ingredients` table was seeded, the **Manual Entry Path** was never updated to respect this authority. It still operates as if the database is an empty slate to be filled by the user.

## 6. WHERE VALIDATION IS CURRENTLY MISSING
*   **UI**: No autocomplete or restricted selection list.
*   **ViewModel**: No check to ensure the ingredient name belongs to the "Seed" set.
*   **Repository**: Blind insertion of new ingredient strings.

## 7. PROPOSED CONTROLLED ENTRY FLOW (RECOMMENDATION)
1.  **Search-First Input**: Replace the `name` text field with an `ExposedDropdownMenuBox` or a "Search & Select" interaction.
2.  **Restricted Confirmation**: The "Add" action should only be enabled once a **Canonical Ingredient ID** is resolved from the vocabulary.
3.  **No-Match Handling**: If no match is found, show "No ingredient found" and prevent addition.

## 8. PROPOSED SUGGESTION FLOW
*   **Repository**: Add `searchCanonicalIngredients(query: String)` to `IngredientRepository`.
*   **ViewModel**: Expose a `filteredSuggestions: StateFlow<List<Ingredient>>` that updates in real-time as the user types.
*   **UI**: Display matches (e.g., "Chicken Breast", "Chicken Thigh") as the user types "chick".

## 9. PROPOSED FUZZY-MATCH / CLOSEST-MATCH FLOW
*   Implement a **Levenshtein Distance** or **Double Metaphone** check at the ViewModel/Repository layer.
*   If the user types "Chickenn", the system should suggest: *"Did you mean: Chicken?"*

## 10. PROPOSED QUANTITY/UNIT CONTROL MODEL
*   **Vocabulary Metadata**: The `Ingredient` domain model should eventually include a `defaultUnits` list.
*   **Pantry Entry**: Constrain the unit selection to relevant units based on the selected ingredient (e.g., "lb", "oz", "pcs" for Meat; "tsp", "cup" for Oil).

## 11. CROSS-SYSTEM IDENTITY CONSISTENCY FINDINGS
*   **Observed Fact**: Recipe matching (`MatchingEngine.kt`) relies on `ingredientId`.
*   **Observed Fact**: If "Chickenn" is in the pantry, it will **NEVER** match a recipe for "Chicken" because the IDs are different.
*   **Impact**: Manual entry typos break the core value proposition of the app (recommendations).

## 12. DATA MIGRATION / EXISTING BAD-ENTRY CONCERNS
*   **Issue**: Users may already have "Chickenn" or "russet potato" (fragmented) in their DB.
*   **Recommendation**: Implement a one-time "Identity Cleanup" service that merges existing manual entries into the nearest canonical ID if the match confidence is > 95%.

## 13. RISKS AND EDGE CASES
*   **Staple Variations**: A user might want "Organic Whole Milk" specifically. The system must decide if the vocabulary is broad enough or if "Organic" is an **Attribute**, not an **Identity**.

## 14. RECOMMENDED IMPLEMENTATION OWNERSHIP BY LAYER
*   **UI Layer**: Searchable selection UI.
*   **Logic Layer (ViewModel)**: Filtering and suggestion generation.
*   **Domain Layer**: Defining the "Variety vs Attribute" rules.

## 15. MANUAL VALIDATION PLAN
1.  Open Add Ingredient.
2.  Type "chick". Verify suggestions appear from the 300-recipe corpus.
3.  Type "Chickenn". Verify the "Add" button is disabled and a "No match found" message appears.
4.  Select "Chicken Breast". Verify the unit dropdown restricts choices to weight/pieces.

---

**AUDIT COMPLETE — NO CHANGES APPLIED**  
Standing by for authorization to implement the restricted, suggestion-based entry system.
