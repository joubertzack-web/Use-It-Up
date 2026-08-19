# USE IT UP — CHANGE LOG

## [Phase 0] — Ground Zero

**Date:** 2026-08-19
**Status:** COMPLETE

Established foundational project requirements, data model definitions, and source strategies.

---

## [Historical Note] — Rejected Initial Implementation

**Date:** 2026-08-19
**Status:** REJECTED & PURGED

An initial implementation relied on synthetic data, placeholder logic, and UI-first development. Audit determined the implementation was approximately 1% MVP-ready. The work was purged and the project was rebuilt from the authoritative requirements.

---

## [v0.0.1] — Step 1: Unauthorized Implementation Purge

**Date:** 2026-08-19
**Status:** PASS

* Removed synthetic 300-recipe corpus (`recipes_seed.json`).
* Removed placeholder `RecommendationEngine`.
* Removed `DatabaseSeeder`.
* Cleaned DAOs and repositories of mock logic.
* Left the project in a clean, incomplete state for correct rebuilding.

---

## [v0.0.2] — Step 2: Specification Audit

**Date:** 2026-08-19
**Status:** PASS

* Audited `01_Data_Model.txt`, `MVP_finalDesign.txt`, `Recipes.txt`, and `Sources.txt`.
* Established authoritative baseline requirements.
* Locked core requirements: Native Android, Compose, Room, and local-first architecture.

---

## [v0.0.3] — Step 3: Canonical Domain Model

**Date:** 2026-08-19
**Status:** PASS

Established domain models in `com.example.leftovers.domain.model`.

**Entities:**

* Recipe
* Ingredient
* RecipeIngredient
* PantryItem
* PantryQuantity
* Leftover
* GroceryRequirement

**Constraint:** Recipe quantities remain Strings; pantry quantities are tracked separately.

---

## [v0.0.4] — Step 4: Room Persistence

**Date:** 2026-08-19
**Status:** PASS

* Rebuilt Room entities and DAOs from the canonical model.
* Established `RecipeIngredientEntity` junction table.
* Implemented `PantryQuantity` serialization for state preservation.
* Incremented database version.

---

## [v0.0.5] — Step 5: Domain / Persistence Mapping

**Date:** 2026-08-19
**Status:** PASS

* Implemented `Mappers.kt` in the data layer.
* Established bidirectional mapping between Room entities/relations and domain models.
* Verified fidelity of qualitative measurements and pantry states.

---

## [v0.0.6] — Step 6: Repository Layer

**Date:** 2026-08-19
**Status:** PASS

* Refactored `RecipeRepository`, `PantryRepository`, and `GroceryListRepository`.
* Created `IngredientRepository` and `LeftoverRepository`.
* Repositories now expose domain models and use atomic Room transactions.

---

## [v0.0.7] — Step 7: Search Foundation

**Date:** 2026-08-19
**Status:** PASS

* Implemented `searchRecipes` in `RecipeDao` and `RecipeRepository`.
* Added broad matching across recipe names, tags, and ingredient names.
* Uses relational joins for ingredient-aware discovery.

---

## [v0.0.8] — Step 8: Sort / Filter Engine

**Date:** 2026-08-19
**Status:** PASS

* Created domain-level `RecipeQueryEngine`.
* Implemented `RecipeSortMode` and `RecipeFilter`.
* Added multi-term search requiring all terms to match.

---

## [v0.0.9] — Step 9: Favorites

**Date:** 2026-08-19
**Status:** PASS

* Implemented atomic favorite updates in `RecipeDao` and `RecipeRepository`.
* Verified persistence and relational stability.

---

## [v0.0.10] — Step 10: Pantry Domain Logic

**Date:** 2026-08-19
**Status:** PASS

* Created `PantryEngine` for inventory logic.
* Implemented smart combination of measured and available pantry quantities.
* Integrated pantry logic into `PantryRepository`.

---

## [v0.0.11] — Step 11: Leftover Domain Logic

**Date:** 2026-08-19
**Status:** PASS

* Implemented full CRUD for leftovers.
* Added optional ingredient linking for discovery matching.

---

## [v0.0.12] — Step 12: Matching Primitives

**Date:** 2026-08-19
**Status:** PASS

* Created `MatchingEngine` producing `MatchResult` facts.
* Established deterministic matching between recipes and current inventory/leftovers.

---

## [v0.0.13] — Step 13: Recommendation Engine

**Date:** 2026-08-19
**Status:** PASS

* Created `RecommendationEngine` with explicit 10/50/30 scoring weights.
* Implemented Top 5 recommendation logic.
* Created `RecommendedRecipe` domain wrapper.

---

## [v0.0.14] — Step 14: Grocery Requirement Engine

**Date:** 2026-08-19
**Status:** PASS

* Created `GroceryEngine` for identifying missing ingredients.
* Preserved qualitative pantry states; available staples do not trigger shopping requirements.

---

## [v0.0.15] — Step 15: Grocery Aggregation

**Date:** 2026-08-19
**Status:** PASS

* Implemented multi-recipe grocery aggregation.
* Added unit-aware summing and qualitative quantity preservation.

---

## [v0.0.16] — Step 16: Personal Recipe Capability

**Date:** 2026-08-19
**Status:** PASS

* Added `isUserCreated` to `Recipe` and `RecipeEntity`.
* Verified CRUD support for user-created recipes using the canonical relational infrastructure.

---

## [v0.0.17] — Step 17: Application Orchestration

**Date:** 2026-08-19
**Status:** PASS

* Created `RecipeService`, `InventoryService`, and `GroceryService`.
* Orchestrated reactive data streams using Kotlin `Flow.combine`.
* Isolated business logic from UI concerns.

---

## [v0.0.18] — Step 18: Pre-Data Integration Audit

**Date:** 2026-08-19
**Status:** PASS

* Conducted full source-level audit of the rebuilt logic stack.
* Confirmed zero-placeholder policy compliance.
* Declared system ready for real data ingestion.

---

## [v0.0.19] — Step 19: Recipe Corpus Acquisition & Selection

**Date:** 2026-08-19
**Status:** PASS

* Downloaded verified Wikibooks Cookbook dataset containing 3,895 records.
* Conducted usability audit, identifying 3,717 usable candidates.
* Selected the 300-recipe MVP corpus using established targets and quality metrics.
* Generated `wikibooks_selection_manifest.json`.

---

## [v0.0.20] — Step 20: Recipe Corpus Normalization & Source Documentation

**Date:** 2026-08-19
**Status:** IMPLEMENTATION AUTHORIZED / VALIDATION REQUIRED

### Scope

Normalize and validate the selected 300-recipe Wikibooks MVP corpus against the established Use It Up domain model.

### Inputs

* `recipes_parsed.json`
* `wikibooks_selection_manifest.json`
* `Use It Up_300_Recipe_Selection_Report.md`
* `Documents/01/Recipes.txt`
* `Documents/01/Sources.txt`
* `Documents/01/MVP_finalDesign.txt`
* `Documents/01/01_Data_Model.txt`

### Requirements

* Preserve original recipe meaning.
* Preserve provenance.
* Preserve source quantities.
* Preserve qualitative quantities such as "to taste," "pinch," and "as needed."
* Do not fabricate measurements, ingredients, or instructions.
* Canonicalize ingredient identities only where semantically safe.
* Keep recipe quantities separate from pantry quantities.
* Flag ambiguous or unusable records rather than guessing.

### Required Artifacts

* `wikibooks_300_normalized.json`
* `Documents/01/Use It Up_300_Normalization_Report.md`
* `Documents/01/Sources_Updated.txt`

### Source Documentation

`Sources_Updated.txt` documents the actual sources used by the current MVP while preserving the distinction between:

* Sources actually used
* Sources planned for future use
* Historical source strategy

The existing `Sources.txt` is preserved as a historical reference.

### Acceptance

This step is not fully verified until the normalized dataset and validation results have been inspected. The recipe corpus is not yet declared production ready.

### Next Step

Step 21: First-Launch Room Import.

---

## [v0.0.21] — Step 21: First-Launch Room Import

**Date:** 2026-08-19
**Status:** PASS — TECHNICAL READINESS COMPLETE

* Packaged the 300-recipe corpus into `app/src/main/assets/recipes_300.json`.
* Implemented `SeedStatusEntity` with deterministic `NOT_STARTED`, `IN_PROGRESS`, and `COMPLETE` states.
* Created `DataImportService` for atomic, transactional Room ingestion.
* Integrated import into `Use It UpApplication.onCreate()`.
* Verified deduplication for canonical ingredient identities.
* Resolved UI build errors resulting from the domain model transition.

---

## [v0.0.22] — Steps 22–41: UI Phase Implementation

**Date:** 2026-08-19
**Status:** PENDING VALIDATION

### Implementation

* Conducted UI Step 1 audit.
* Acquired CC0/Public Domain assets and generated the UI Step 2 Asset Manifest.
* Established UI Step 3 architecture.
* Implemented Navigation 3 shell with adaptive Bar/Rail support and the Kitchen Hub.
* Built Home, Discover, Kitchen, Recipe Detail, and Saved screens.
* Integrated the real `RecommendationEngine`, `MatchingEngine`, and `GroceryEngine`.
* Applied the Earthy Kitchen design system with Sage/Terracotta palette.
* Added spring-based favorite and checkbox animations.
* Optimized touch targets, semantics, and contrast.

### Reporting

Generated the UI Verification Report.

### Technical Verification

* Build: SUCCESSFUL
* Navigation: FUNCTIONAL
* Data Integration: REAL — Wikibooks 300
* Placeholders: REMOVED

### Remaining Validation

* Visual identity and overall coziness.
* Favorite and checkbox animation behavior.
* Adaptive layout behavior on tablet/foldable configurations.
* Final representation of the Wikibooks corpus.

---

## [v0.1.0] — UI Phase Final Completion

**Date:** 2026-08-19
**Phase:** UI Implementation & Polish
**Status:** TECHNICAL IMPLEMENTATION COMPLETE

### Major Capabilities

* Kitchen Hub integrating Pantry, Use It Up, and Grocery requirements.
* Real recommendation synthesis using deterministic `RecommendationEngine` output.
* Earthy Kitchen design system with Sage/Terracotta palette and spring-based interactions.
* Adaptive Navigation 3 shell for phone and tablet configurations.
* 300-recipe corpus rendering with provenance and qualitative quantities.

### Files Changed

**Created:**

* `KitchenScreen.kt`
* `Use It UpScreen.kt`
* `SavedScreen.kt`
* `SavedViewModel.kt`
* `RecipeJson.kt`
* `DataImportService.kt`

**Modified:**

* `Destinations.kt`
* `Use It UpApp.kt`
* `HomeScreen.kt`
* `RecipeDetailScreen.kt`
* `PantryScreen.kt`
* `GroceryListScreen.kt`
* `Theme.kt`
* `Color.kt`

### Technical Verification

* Build: SUCCESS — `:app:assembleDebug`
* Zero-placeholder audit: PASS
* Synthetic data: PURGED
* Fallbacks: DOCUMENTED

### Known Limitations

* Numerical quantity math such as `1/2 + 1/4` is not yet implemented; string aggregation is currently used.
* Binary image assets for the 300-recipe corpus currently use framework fallbacks.

---

## [v0.1.1] — Brand Identity Update

**Date:** 2026-08-19
**Phase:** UI / Branding
**Status:** PASS

* Integrated the stylized logo as the authoritative brand identity.
* Added `app_logo.png` to project drawables.
* Updated the UI Step 2 Asset Manifest.

---

## [v0.1.2] — Monetization Audit

**Date:** 2026-08-19
**Phase:** Monetization
**Status:** AUDIT COMPLETE / FURTHER IMPLEMENTATION NOT AUTHORIZED

* Conducted forensic audit of current AdMob integration.
* Confirmed development builds use official Google Test IDs.
* Verified functional "Disable Advertisements" setting in DataStore.
* Inventoried ad placements in Home and Search.
* Confirmed advertising logic remains isolated from recipe and pantry domain engines.
* Produced Monetization Forensic Audit.
* Produced Ad Placement Inventory.
* Produced Monetization Settings Audit.
* Produced AdMob Production Readiness Report.
* Produced Monetization Architecture Report.

---

## [v0.1.3] — Recipe Classification Rebuild

**Date:** 2026-08-19
**Status:** PASS

* Rebuilt classification metadata for the 300-recipe MVP corpus.
* Applied multi-label classification based on actual recipe contents, including Chicken, Beef, Soup, Vegetarian, Gluten-Free, and other applicable categories.
* Generated Classification Manifest.
* Generated Corpus Integrity Report.
* Verified 100% integrity of the 300-recipe corpus identities.
* Updated `wikibooks_300_normalized.json` and `app/src/main/assets/recipes_300.json`.
* Increased classification coverage from approximately 1 tag per recipe to more than 5 tags per recipe.

---

## [v0.1.4] — Recipe UI Forensic Audit & Redesign Specification

**Date:** 2026-08-19
**Phase:** UI
**Status:** AUDIT COMPLETE / IMPLEMENTATION NOT AUTHORIZED

* Conducted forensic audit of `RecipeDetailScreen.kt`.
* Identified P0 layout failure causing vertical/clipped text with real recipes such as Chicken Cacciatore.
* Identified visual hierarchy and legibility defects in long-form instructions.
* Produced Recipe UI Redesign Specification based on the Kitchen Journal concept.
* Defined asset requirements for parchment textures and organic icons.
* Created validation matrix covering 300-recipe corpus edge cases.
* Result: Recipe UI baseline moved to FAIL; further prototype validation is blocked pending implementation of the new specification.

---

## [v0.1.5] — Recipe UI Redesign Implementation

**Date:** 2026-08-19
**Phase:** UI
**Status:** TECHNICALLY IMPLEMENTED / VALIDATION REQUIRED

* Implemented the Kitchen Journal Recipe UI redesign.
* Fixed the P0 ingredient-row layout failure using `Modifier.weight(1f)` for name wrapping.
* Implemented parsed instruction steps with 1.5× line spacing for cooking legibility.
* Created `RecipeSharedComponents` and `RecipeDetailComponents` to modularize the screen.
* Added adaptive Split Journal layout for tablet and expanded viewports.
* Added spring-animated Pop effect to the Favorite toggle.
* Checked rendering against complex recipes including Chicken Cacciatore and Roast Beef using the 300-recipe corpus.
* Produced Recipe UI Implementation Report.

### Current Status

Recipe UI implementation is complete for this phase, with validation remaining on the identified rendering and layout issues.

---

## [v0.1.7] — Global Rename & Integrated Repairs
**Date**: 2026-08-19
**Phase**: UX / Product
**Status**: IMPLEMENTATION COMPLETE — VALIDATION REQUIRED
**Action**:
- **Global Rename**: Rebranded application from "Use It Up" to "**Use It Up**" across all layers (App ID, strings, class names, navigation).
- **AdMob Production Readiness**: Updated AdMob IDs to authorized production values provided by the user.
- **Grocery Deletion Fix**: Resolved the functional bug where bought items remained on the list. `GroceryRequirement` now correctly carries database IDs through the aggregation layer.
- **Onboarding Implementation**: Created and integrated a 5-step first-use orientation flow using the new brand identity.
- **Grocery UI Redesign**: Replaced ambiguous icons with explicit "Stock" (Buy/Add to Pantry) and "Remove" actions for user clarity.
- **Recipe UI repairs**: Finalized the `LocalJournalPalette` ownership model and fixed window insets for the Kitchen Journal.
- **Technical Verification**: Build successful. Persistence and reactive updates verified for the Grocery -> Pantry flow.

---

## [v0.1.8] — Recipe UI Structural Rebuild
**Date**: 2026-08-19
**Phase**: UI / Architecture
**Status**: IMPLEMENTATION COMPLETE — VALIDATION REQUIRED
**Action**:
- **Structural Rebuild**: Refactored the Recipe Detail screen from a monolithic composition into a **Strict Card Shell** architecture.
- **Eliminated Dead Space**: Removed `fillMaxSize()` and `weight()` constraints from scrollable cards, ensuring they are content-sized.
- **Spacing Ownership**: Established a hierarchy where the Page Shell owns gaps between cards (16dp) and Cards own internal padding.
- **Contrast & Hierarchy**: Implemented theme-independent Journal contrast and visual anchors for Method steps.
- **Inset Fix**: Corrected content underlap by applying Scaffold top padding inside the scroll container.
- **Verification**: Verified stable rendering across long ingredient lists and instructions (e.g. *Arroz con Pollo*, *Roast Beef*).
