# USE IT UP — STEP 3 DATA MODEL REPORT

## 1. Files Created

- [Ingredient.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/domain/model/Ingredient.kt)
- [Recipe.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/domain/model/Recipe.kt)
- [PantryItem.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/domain/model/PantryItem.kt)
- [Leftover.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/domain/model/Leftover.kt)
- [GroceryRequirement.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/domain/model/GroceryRequirement.kt)

## 2. Files Modified

- None. (Purged placeholder models were left intact to avoid breaking current compilation before the persistence layer is rebuilt, but are documented as conflicting below).

## 3. Files Deleted

- None.

## 4. Models Established

- **Ingredient:** Canonical identity (name, category).
- **Recipe:** Complete structure including metadata (source, tags, servings) and a list of `RecipeIngredient`.
- **RecipeIngredient:** Data structure representing the usage of an ingredient in a recipe, preserving the original quantity string and unit.
- **PantryItem:** Tracks user inventory, linking to an `Ingredient` ID and using the `PantryQuantity` sealed class.
- **PantryQuantity:** Sealed class supporting `Measured` (Double + Unit), `Available` (presence only), and `NotAvailable`.
- **Leftover:** Prepared food items with qualitative/quantitative descriptions and ingredient links.
- **GroceryRequirement:** Aggregated needs for shopping, referencing multiple recipes.

## 5. Quantity Representation

- **Recipe Quantity:** Represented as a `String` (e.g., "1/2", "2") to avoid precision loss or fabrication of numbers from the source.
- **Pantry Quantity:** Represented via `PantryQuantity` sealed class. This allows for specific measurements when known (e.g., "500g") while supporting the "available/not available" requirement for staples.

## 6. Qualitative Measurement Representation

- Supported natively in `RecipeIngredient.quantity` as a `String`. This allows "to taste", "pinch", "as needed", etc., to be stored without conversion to arbitrary numeric values.

## 7. Recipe vs Pantry Quantity Separation

- **RecipeIngredient** uses a `quantity: String` and `unit: String?` model to reflect the source text.
- **PantryItem** uses the `PantryQuantity` sealed class to reflect user inventory state (Measured vs. Status).
- These concepts are strictly decoupled in the domain model.

## 8. Relationship Model

- **Recipes** contain a list of **RecipeIngredients**.
- **RecipeIngredients** optionally link to a canonical **Ingredient** ID.
- **PantryItems** and **Use It Up** link to a canonical **Ingredient** ID.
- **GroceryRequirements** link to a canonical **Ingredient** ID and a list of **Recipe** IDs.

## 9. Decisions Taken From Existing Specification

- Adhered to the "strict uniform JSON structure" requirement by ensuring the domain model can map cleanly to/from the intended JSON.
- Implemented "Local-first" principle by designing models that contain all necessary metadata for offline operation.
- Preserved "Provenance" requirement in the `Recipe` model.

## 10. Conflicts Discovered

> [!WARNING]
> **Existing Persistence Layer Conflicts**
> The following existing files in `com.example.leftovers.data` conflict with the canonical model and MUST be replaced in the next step:
> 
> 1. `IngredientEntity.kt`: Currently forces `quantity: Double` and `unit: String` into the ingredient identity.
> 2. `PantryEntity.kt`: Currently forces `quantity: Double`, failing to support the "Available" status.
> 3. `RecipeIngredient.kt` (model): Currently forces `quantity: Double`, failing to support "to taste" or "pinch".
> 4. `SeedRecipe.kt`: Currently forces `quantity: Double` for imported data.

## 11. Validation Performed

Manual verification of the model's ability to represent the required test cases:

1. **2 chicken breasts:** `name="chicken breasts"`, `quantity="2"`, `unit=null` (OK)
2. **1 cup rice:** `name="rice"`, `quantity="1"`, `unit="cup"` (OK)
3. **1/2 onion:** `name="onion"`, `quantity="1/2"`, `unit=null` (OK)
4. **2 eggs:** `name="eggs"`, `quantity="2"`, `unit=null` (OK)
5. **1 tbsp olive oil:** `name="olive oil"`, `quantity="1"`, `unit="tbsp"` (OK)
6. **1 tsp salt:** `name="salt"`, `quantity="1"`, `unit="tsp"` (OK)
7. **salt to taste:** `name="salt"`, `quantity="to taste"`, `unit=null` (OK)
8. **pinch of salt:** `name="salt"`, `quantity="pinch"`, `unit=null` (OK)
9. **pepper as needed:** `name="pepper"`, `quantity="as needed"`, `unit=null` (OK)

**Pantry State Validation:**
- **Chicken:** `PantryQuantity.Measured(2.0, "lbs")` (OK)
- **Rice:** `PantryQuantity.Measured(1.0, "cup")` (OK)
- **Salt:** `PantryQuantity.Available` (OK)
- **Pepper:** `PantryQuantity.Available` (OK)

## 12. Scope Compliance

[x] Canonical Kotlin data structures implemented.
[x] No persistence (Room) implemented.
[x] No UI modified/implemented.
[x] No recipe seed data created.
[x] No search/recommendation logic implemented.
[x] No Step 4 work performed.

## 13. Remaining Work

- Rebuilding the Room Persistence layer (Entities, DAOs, Database) to match this canonical model.
- Implementing TypeConverters for the `PantryQuantity` sealed class.
- Updating JSON serialization/deserialization for seed data import.
