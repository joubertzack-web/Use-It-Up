# Use It Up — Targeted Polish Audit / Investigation Findings

**DATE:** 2026-08-19  
**PHASE:** Audit / Investigation  
**STATUS:** **AUDIT ONLY** — NO CODE MODIFIED

---

## TARGET 1: FIRST-USE / HOME EXPERIENCE

### 1. OBSERVED BEHAVIOR
A new user completes a 5-step onboarding flow explaining the value of inventory-driven recommendations. Upon clicking "Get Started," they are dropped onto a "Home" screen that is visually sparse, containing only a "Ready to Cook?" header and a text label: *"Add items to your pantry to see recommendations!"*

### 2. INVESTIGATION
- **Flow Trace**: `OnboardingScreen` → `onboardingViewModel.completeOnboarding()` → `UseItUpApp` → `MainAppShell`.
- **Default Route**: `rememberNavBackStack(UseItUpRoute.Home)`.
- **Home Composition**: `HomeScreen.kt` uses a `LazyVerticalGrid` that checks `topRecipes.isEmpty()`. If true, it renders a single `Text` item.

### 3. EVIDENCE
The transition from a high-energy, image-heavy onboarding flow to an empty text-based Home screen violates the **"First Fifty Feet"** principle. The user has been "sold" on the concept but is not given the tool to realize it.

### 4. ROOT-CAUSE HYPOTHESIS
The navigation logic treats "Home" as the technical root, but "Pantry" is the **product root** for a new user. The empty state lacks an "Action Bridge."

### 5. CONFIDENCE LEVEL: 5/5

### 6. ALTERNATIVE HYPOTHESES
- The transition is too abrupt (Animation problem).
- The Home screen should show "featured" recipes regardless of inventory (rejected by MVP privacy/local-first specs).

### 7. RECOMMENDED FIX
Modify the post-onboarding completion callback to route the user specifically to `UseItUpRoute.Kitchen` (Pantry tab) for their **first-ever session**. Alternatively, add a high-prominence "Stock My Pantry" button to the Home empty state.

### 8. CORRECT OWNERSHIP LAYER
**Navigation Shell** (`UseItUpApp.kt`) and **UI Logic** (`HomeScreen.kt`).

### 9. WHAT SHOULD NOT BE CHANGED
Do not change the recommendation engine scoring; the "emptiness" is technically correct data-wise.

### 10. MANUAL VALIDATION PLAN
1. Clear App Data.
2. Complete onboarding.
3. Verify arrival at an action-oriented screen (Pantry) instead of an empty Home feed.

---

## TARGET 2: RECIPE DETAIL — INGREDIENT RENDERING

### 1. OBSERVED BEHAVIOR
Structural cards are stable, but internal row rendering is erratic. *Fresh Egg Pasta* renders perfectly; *Baby Back Ribs* and *Chicken Rice Salad* exhibit vertical squashing, overlapping text, or orphaned numbers.

### 2. INVESTIGATION
Trace the data for "Fresh Egg Pasta" (PASS) vs "Chicken Rice Salad" (FAIL):
- **Good Data**: `quantity: "300", unit: "g"`. (Numeric/Simple).
- **Bad Data**: `quantity: "½ cup (120 ml) mayonnaise"`, `name: "½ cup (120 ml) mayonnaise"`.
- **Structure**: The `quantity` field in the "Bad" data is contaminated with the full source string.

### 3. EVIDENCE
In `WeightedIngredientRow`, the UI assumes the right-side text is a short quantity.
```kotlin
// UI Assumption
modifier = Modifier.widthIn(min = 48.dp) // Aligned right
```
When `quantity` contains "½ cup (120 ml) mayonnaise", it fights with the `name` column for horizontal space, causing a catastrophic layout collapse.

### 4. ROOT-CAUSE HYPOTHESIS
**Data Normalization failure.** The importer is failing to bifurcate "qualitative descriptors" from "canonical names" for certain Wikibooks string patterns, causing the `quantity` field to act as a second `name` field.

### 5. CONFIDENCE LEVEL: 5/5

### 6. ALTERNATIVE HYPOTHESES
- Compose `Text` rendering bug (Unlikely).
- ConstraintLayout circular dependency (Not used here).

### 7. RECOMMENDED FIX
1. **Ingestion Layer**: Update `DataImportService` to detect when `quantity` is a semantic duplicate of `name` and nullify it.
2. **UI Layer**: Update `WeightedIngredientRow` to detect "Long Quantities" and render them as a subtitle under the name rather than a right-aligned column.

### 8. CORRECT OWNERSHIP LAYER
**Data Service** (`DataImportService.kt`) and **Shared UI Components**.

### 9. WHAT SHOULD NOT BE CHANGED
Do not change the `JournalSurface` or color palette; they are functioning correctly.

---

## TARGET 3: CANONICAL INGREDIENT IDENTITY

### 1. OBSERVED BEHAVIOR
The Grocery list generates multiple entries for semantically identical items (e.g., "Medium Russet Potatoes" and "Small Russet Potatoes"). Matching fails if a recipe asks for "Onion" but the pantry has "yellow onion".

### 2. INVESTIGATION
- **Persistence**: `IngredientEntity` is currently keyed by whatever string the source provided.
- **Matching**: `MatchingEngine.kt` uses literal string containment.
- **Mapping**: `Mappers.kt` passes source-contaminated names directly into the domain `Ingredient` object.

### 3. EVIDENCE
`classification_manifest.json` shows entries like:
- `canonical_name: "3-4 each medium russet potatoes"`
This proves the "Canonical" field is currently a copy of the unparsed ingredient string.

### 4. ROOT-CAUSE HYPOTHESIS
The system lacks an **Authority List**. It is attempting to derive identity from highly variable recipe prose rather than resolving prose to a fixed ID.

### 5. CONFIDENCE LEVEL: 5/5

### 6. PROPOSED CANONICAL MODEL & RULES
- **The Model**: `IngredientVocabulary` (Fixed Catalog)
    - `id: Long`
    - `canonicalName: "Potato"`
    - `group: "Russet"`
    - `synonyms: ["spud", "earth apple"]`
- **Identity Rules**:
    - `Source String`: "2 each medium russet potatoes, peeled"
    - → `Quantity`: "2"
    - → `Unit`: "each"
    - → `Identity`: "Potato (Russet)"
    - → `Prep`: "peeled, medium"

### 7. RECOMMENDED FIX
Implement an **Ingredient Benchmark Dataset** (approx. 500 common kitchen items). Update `DataImportService` to resolve source strings against this benchmark using a multi-pass match (Exact → Synonym → Word-stem).

### 8. CORRECT OWNERSHIP LAYER
**Domain Model** and **Data Ingestion**.

### 9. WHAT SHOULD NOT BE CHANGED
Original source strings. These MUST be preserved in the `RecipeIngredient` entity to maintain provenance and the user's "Journal" experience.

---

## TARGET 4: INGREDIENT AVAILABILITY UX

### 1. OBSERVED BEHAVIOR
Availability is communicated by a 8dp colored dot. User testing shows users do not associate these colors with "In Pantry" vs "Missing" without being told.

### 2. INVESTIGATION
- **Component**: `WeightedIngredientRow` in `RecipeSharedComponents.kt`.
- **Accessibility**: Color is the **only** signal. No semantic text or iconography is present.

### 3. EVIDENCE
```kotlin
val statusColor = if (isAvailable) palette.statusHave else palette.statusNeed
// ...
Surface(color = statusColor, ...) // Tiny bullet
```

### 4. ROOT-CAUSE HYPOTHESIS
**Semantic Minimalism.** The design prioritized "Earthy Coziness" over clear functional feedback.

### 5. CONFIDENCE LEVEL: 5/5

### 6. ALTERNATIVE HYPOTHESES
- The dots are too small (Size problem).
- The colors are wrong (Palette problem).

### 7. RECOMMENDED FIX
Add a secondary, non-color signal.
1. **Iconography**: Use a tiny checkmark for `Available` and a plus-sign for `Missing`.
2. **Typography**: Use an "Action Label" (e.g., "HAVE" vs "NEED") or apply a `LineThrough` decoration to available items to simulate a physical checklist.

### 8. CORRECT OWNERSHIP LAYER
**Shared UI Components**.

### 9. WHAT SHOULD NOT BE CHANGED
Do not replace the "Journal" font or paper background.

### 10. MANUAL VALIDATION PLAN
1. Open a recipe.
2. Ensure you can identify which items are missing **with the screen in grayscale** (simulating color-blindness or low discoverability).
