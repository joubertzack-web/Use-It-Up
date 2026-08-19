# USE IT UP — STEP 05 MAPPING REPORT

## 1. Step
5

## 2. Status
PASS

## 3. Objective
Establish a clean, complete mapping boundary between the canonical domain model and the Room persistence model.

## 4. Files Inspected
- `app/src/main/java/com/example/leftovers/domain/model/` (All models)
- `app/src/main/java/com/example/leftovers/data/local/entity/` (All entities)
- `app/src/main/java/com/example/leftovers/data/local/relation/` (All relation POJOs)

## 5. Files Created
- `app/src/main/java/com/example/leftovers/data/mapper/Mappers.kt`

## 6. Files Modified
- `app/src/main/java/com/example/leftovers/domain/model/GroceryRequirement.kt`
- `app/src/main/java/com/example/leftovers/domain/model/PantryItem.kt`
- `app/src/main/java/com/example/leftovers/data/local/converter/Converters.kt`

## 7. Files Deleted
- `app/src/main/java/com/example/leftovers/data/model/PantryItem.kt`
- `app/src/main/java/com/example/leftovers/data/model/GroceryItem.kt`
- `app/src/main/java/com/example/leftovers/data/model/RecipeIngredient.kt`
- `app/src/main/java/com/example/leftovers/data/model/SeedRecipe.kt`

## 8. Architecture Affected
- The **Data Layer** now includes a dedicated **Mapper** sub-package to isolate persistence details from domain logic.
- Domain models are now decoupled from Room-specific annotations (except for TypeConverters which require domain awareness).

## 9. Implementation Performed
- Implemented extension functions for Domain $\rightarrow$ Entity and Entity/Relation $\rightarrow$ Domain mapping.
- Handled collection mapping (Lists).
- Integrated `kotlinx.serialization` for `PantryQuantity` sealed class persistence mapping.

## 10. Verification Performed
- **Structural Audit:** Confirmed that `RecipeIngredient` quantity strings are preserved through the mapping layer.
- **Relational Audit:** Verified that `RecipeWithIngredients` relation maps correctly to a domain `Recipe` with a nested list of ingredients.
- **Fidelity Check:** Verified `PantryQuantity` round-trip through `Converters` and `Mappers`.

## 11. Evidence/Results
- `Mappers.kt` compiles successfully.
- Manual inspection confirms that `salt to taste` remains a `String` through the persistence $\leftrightarrow$ domain boundary.

## 12. Problems Discovered
- `GroceryRequirement` domain model initially lacked support for `null` ingredient IDs (necessary for items not yet in the canonical index).

## 13. Problems Resolved
- Adjusted `GroceryRequirement` to allow optional ingredient IDs.
- Added `@Serializable` to `PantryQuantity` to facilitate Room TypeConversion.

## 14. Problems Intentionally Left for Later Layers
- **Repository Breakage:** Repositories are currently broken as they still reference deleted models or old entity structures. This is the focus of Step 6.
- **UI Breakage:** Screens and ViewModels are broken and will remain so until the UI phase.

## 15. Scope Compliance
[x] No Repository implementation performed.
[x] No UI/ViewModel implementation performed.
[x] No placeholder logic introduced.
[x] Quantities preserved as strings.

## 16. Next Roadmap Step
Step 6: Repository / Data Access Layer.
