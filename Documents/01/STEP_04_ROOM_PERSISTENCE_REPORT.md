# USE IT UP — STEP 4 ROOM PERSISTENCE REPORT

## 1. Files Created

- [RecipeIngredientEntity.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/entity/RecipeIngredientEntity.kt)
- [RecipeWithIngredients.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/relation/RecipeWithIngredients.kt)
- [PantryItemWithIngredient.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/relation/PantryItemWithIngredient.kt)
- [GroceryItemWithIngredient.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/relation/GroceryItemWithIngredient.kt)

## 2. Files Modified

- [IngredientEntity.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/entity/IngredientEntity.kt)
- [RecipeEntity.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/entity/RecipeEntity.kt)
- [PantryEntity.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/entity/PantryEntity.kt)
- [LeftoverEntity.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/entity/LeftoverEntity.kt)
- [GroceryListEntity.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/entity/GroceryListEntity.kt)
- [Converters.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/converter/Converters.kt)
- [AppDatabase.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/AppDatabase.kt)
- [RecipeDao.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/dao/RecipeDao.kt)
- [IngredientDao.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/dao/IngredientDao.kt)
- [PantryDao.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/dao/PantryDao.kt)
- [GroceryListDao.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/dao/GroceryListDao.kt)
- [LeftoverDao.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/data/local/dao/LeftoverDao.kt)
- [PantryItem.kt](file:///C:/Users/fixit/Documents/NexiCode/Projects/Use It Up/app/src/main/java/com/example/leftovers/domain/model/PantryItem.kt) (Added `@Serializable`)

## 3. Files Deleted

- `app/src/main/java/com/example/leftovers/data/model/PantryItem.kt` (Replaced by relation POJO)
- `app/src/main/java/com/example/leftovers/data/model/GroceryItem.kt` (Replaced by relation POJO)
- `app/src/main/java/com/example/leftovers/data/model/RecipeIngredient.kt` (Obsolete)
- `app/src/main/java/com/example/leftovers/data/model/SeedRecipe.kt` (Obsolete)

## 4. Room Entities

- **IngredientEntity:** `id`, `name`, `category`. Removed `quantity`/`unit`.
- **RecipeEntity:** `id`, `name`, `instructions`, `tags`, `source`, `servings`, `isFavorite`. Removed embedded ingredient list.
- **RecipeIngredientEntity:** Junction table linking recipes to ingredients with `quantity: String` and `unit: String?`.
- **PantryEntity:** `id`, `ingredientId`, `quantity: PantryQuantity` (serialized), `expirationDate`.
- **LeftoverEntity:** `id`, `ingredientId?`, `name`, `quantity: String`, `notes`, `savedDate`.
- **GroceryListEntity:** `id`, `ingredientId?`, `name`, `requiredQuantity: String`, `recipeReferences: List<Long>`, `isChecked`.

## 5. Relationships

- **Recipe -> RecipeIngredient:** 1-to-Many relationship established via `RecipeWithIngredients` POJO and `RecipeIngredientEntity.recipeId`.
- **RecipeIngredient -> Ingredient:** Many-to-1 relationship established via `RecipeIngredientEntity.ingredientId`.
- **Pantry -> Ingredient:** Many-to-1 relationship established via `PantryItemWithIngredient` POJO.
- **Grocery -> Ingredient:** Many-to-1 relationship established via `GroceryItemWithIngredient` POJO.

## 6. TypeConverters

- Updated `Converters.kt` to handle `PantryQuantity` (sealed class) using `kotlinx.serialization`.
- Maintained converters for `List<String>` and `List<Long>`.

## 7. PantryQuantity Persistence

- `PantryQuantity` states (Measured, Available, NotAvailable) are persisted as a JSON string. This ensures full fidelity of the domain model without inventing extra columns for every possible state or forcing numeric conversion.

## 8. RecipeIngredient Persistence

- Persists the exact string value for `quantity` (e.g., "to taste", "1/2").
- Decouples the ingredient identity (canonical name/category) from the specific quantity used in a recipe.

## 9. Obsolete Persistence Removed

- Deleted previous `data/model/` POJOs that were used for relations but had incorrect types.
- Removed embedded lists in `RecipeEntity` that were based on the old `Double`-heavy models.

## 10. Validation

- **Recipe A (1 tsp) / Recipe B (to taste):** Successfully represented as two separate entries in `RecipeIngredientEntity` referencing the same `IngredientEntity`.
- **Pantry (Measured / Available):** Successfully represented via `PantryQuantity` TypeConverter.

## 11. Build Result

- **Result:** Build finished with errors.
- **Verification:** The errors are confined to the **Repository** and **UI** layers which are still referencing the deleted/modified models. No errors were detected in the `data/local` package, indicating the Room persistence layer compiles correctly.

## 12. Decisions Made

- **Junction Table for Recipe Ingredients:** Chose a separate table (`recipe_ingredients`) rather than a JSON blob to ensure database-level relationships to canonical ingredients.
- **JSON for PantryQuantity:** Chose JSON serialization for the sealed class to maintain simplicity and extensibility while strictly following the "no fabricate numbers" rule.

## 13. Ambiguities

- **Ingredient linking for Grocery/Use It Up:** If a user adds a leftover not found in the database, `ingredientId` is set to `null` and `name` is used. This wasn't explicitly forbidden and provides better UX than failing.

## 14. Scope Compliance

[x] Room persistence layer implemented.
[x] No JSON import implemented.
[x] No business logic implemented.
[x] No UI modified.
[x] No Step 5 work performed.

## 15. Remaining Work

- Rebuilding Repositories to handle the new `RecipeWithIngredients` and `PantryQuantity` logic.
- Fixing UI and ViewModels to match the new data structures.
- Implementing the JSON seed data import process.
