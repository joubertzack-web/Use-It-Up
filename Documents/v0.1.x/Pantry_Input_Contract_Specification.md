# Use It Up — Pantry Input Contract / Final Pre-Implementation Specification

**DATE:** 2026-08-19  
**PHASE:** Final Specification / Contract Lock  
**STATUS:** **AUDIT / SPECIFICATION ONLY** — NO CODE MODIFIED

---

## 1. CANONICAL INGREDIENT SELECTION CONTRACT

### Authoritative Flow
- **User Typing**: Initiates a real-time prefix and substring search against the `ingredients` table.
- **Suggestions**: UI displays matches from the **1,419-item canonical vocabulary** only.
- **Explicit Selection**: The user must tap a suggestion to "lock in" the identity.
- **Enabled State**: The **Add** button in the dialog is **DISABLED** by default. It is only enabled when a valid `ingredientId` is resolved through explicit selection.

### Identity Rules
- **No Creation**: `PantryViewModel` must NOT call `saveIngredient` with user-entered text. The "Create-on-Miss" branch is strictly deprecated.
- **Distinction Preservation**: Variety (e.g., *Russet Potato* vs *Red Potato*) is the primary key. Selecting a parent category (if one exists) must be a deliberate user choice.
- **Fuzzy Suggestions**: Typo-tolerant search (e.g., "Chickenn" $\rightarrow$ "Chicken") is permitted for **Discovery only**, never for automatic resolution.

## 2. QUANTITY / UNIT CONTRACT

### Authoritative Quantity Model
- **Observed Pattern**: The 300-recipe corpus uses three primary measurement families: **Volume**, **Weight**, and **Count**.
- **Supported Units**: `cup`, `g`, `lb`, `ml`, `oz`, `tbsp`, `tsp`, `pcs` (default for count).
- **Pantry Model**: Must support both **Measured** (1.5 lb) and **Qualitative** ("Available", "Missing") states.

### Ingredient-to-Unit Compatibility Model
The system will use **Measurement Groups** assigned to canonical ingredients:
- **Group: DRY_VOLUME** (Flour, Sugar, Spices) $\rightarrow$ `tsp`, `tbsp`, `cup`, `g`.
- **Group: LIQUID_VOLUME** (Oil, Milk, Water) $\rightarrow$ `tsp`, `tbsp`, `cup`, `ml`, `oz`.
- **Group: WEIGHT** (Meat, Vegetables) $\rightarrow$ `g`, `kg`, `oz`, `lb`.
- **Group: COUNT** (Eggs, Whole Produce) $\rightarrow$ `pcs`.

## 3. QUANTITY VALIDATION CONTRACT
- **Integers/Decimals**: Valid (e.g., "2", "2.5").
- **Fractions**: Valid (e.g., "1/2"). Must be parsed to `Double` for storage if measured.
- **Ranges**: Invalid in Pantry (e.g., "3-4"). Users must choose a single value for inventory.
- **Zero/Negative**: Invalid. Quantity must be $> 0$.

## 4. CROSS-SYSTEM IDENTITY GUARANTEE
- **The Guarantee**: The `ingredientId` (Primary Key in `ingredients` table) is the **only** authoritative identity.
- **Consumption Path**:
    - **Recipe**: References `ingredientId` in `recipe_ingredients`.
    - **Pantry**: References `ingredientId` in `pantry`.
    - **Grocery**: References `ingredientId` in `grocery_list`.
    - **Matching**: Compares `RecipeRequirement.ingredientId` $\equiv$ `PantryItem.ingredientId`.
- **Elimination of Strings**: String-based equality checks are strictly limited to the initial "Search & Resolve" interaction.

## 5. EXISTING BAD DATA (MIGRATION)
- **Requirement**: Any `ingredients` record not belonging to the 1,419-item "Seed" set must be flagged.
- **Resolution**: A one-time background migration will attempt to re-map manual strings to the closest canonical ID (e.g., "Chickenn" $\rightarrow$ "Chicken"). If no high-confidence match exists, the record remains isolated for manual deletion by the user.

## 6. IMPLEMENTATION BOUNDARY

### REQUIRED CHANGES
- `PantryViewModel.kt`: Remove create-on-miss logic; implement suggestion state.
- `PantryScreen.kt`: Replace `TextField` with `ExposedDropdownMenuBox` (Autocomplete).
- `IngredientRepository.kt`: Add substring search query.

### MUST NOT BE MODIFIED
- `AppDatabase.kt`: Schema remains version 5 stable.
- `recipes_300.json`: Corpus is already canonicalized.
- `MatchingEngine.kt`: Already operates on IDs.

---

**SPECIFICATION COMPLETE — NO CHANGES APPLIED**  
The contract for a typo-proof, identity-locked pantry is now established. Standing by for authorization to implement.
