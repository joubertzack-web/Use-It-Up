# Use It Up — Polish & Acceptance Checklist

**Date:**  
**Version:**  
**Build:**  
**Device:**  
**Android Version:**  

## Instructions

Check each item against the actual running application.

- `[x]` = PASS
- `[ ]` = FAIL / NOT ACCEPTED
- Record failures in the Defect Log.

---

# 1. First Launch & Onboarding

- [ ] App immediately explains what Use It Up does
- [ ] First action is obvious
- [ ] Onboarding is easy to understand
- [ ] Each onboarding step has one clear purpose
- [ ] Pantry is explained clearly
- [ ] Leftovers are explained clearly
- [ ] Grocery is explained clearly
- [ ] Recipe discovery/recommendations are explained clearly
- [ ] User knows what to do after onboarding
- [ ] Onboarding completion persists after app restart
- [ ] A new user could reasonably use the app without outside explanation

**Result:** PASS / FAIL

**Notes:**


---

# 2. Home

- [ ] Purpose of Home is immediately obvious
- [ ] Recommended recipes are understandable
- [ ] Recommendations feel useful
- [ ] Recipe cards are visually consistent
- [ ] Favorite control is obvious
- [ ] Favorite animation feels intentional
- [ ] Scrolling feels natural
- [ ] No unexplained dead space
- [ ] No obvious visual clutter
- [ ] Ads do not interfere with normal use

**Result:** PASS / FAIL

**Notes:**


---

# 3. Discover

- [ ] Search is immediately understandable
- [ ] Recipe-name search works
- [ ] Ingredient search works
- [ ] Tag search works
- [ ] Multiple search terms work correctly
- [ ] Sort/filter controls are understandable
- [ ] Results are readable
- [ ] Long recipe names render correctly
- [ ] Empty results state is understandable
- [ ] No clipping
- [ ] No excessive dead space

**Result:** PASS / FAIL

**Notes:**


---

# 4. Recipe Detail — Overall Structure

**Recipes Tested:**

1. 
2. 
3. 
4. 
5. 

- [ ] Header is clearly separated from recipe content
- [ ] Hero/identity area is clearly separated
- [ ] Prep List is its own card
- [ ] Method is its own card
- [ ] Action/footer area has its own purpose
- [ ] Recipe reads as a page made of distinct cards
- [ ] Cards do not feel like one giant container
- [ ] Cards do not unexpectedly affect each other's spacing
- [ ] Page-level spacing is consistent
- [ ] No unexplained dead space
- [ ] No sections feel unnecessarily cramped
- [ ] Overall hierarchy is immediately understandable

**Result:** PASS / FAIL

**Notes:**


---

# 5. Recipe Detail — Readability

- [ ] Recipe title is clearly readable
- [ ] Ingredient names are clearly readable
- [ ] Ingredient quantities are clearly readable
- [ ] Method instructions are clearly readable
- [ ] Section headers are clearly readable
- [ ] Tags are clearly readable
- [ ] Body text does not blend into the background
- [ ] No text disappears
- [ ] No text is clipped
- [ ] Long text wraps correctly
- [ ] Text size is comfortable while cooking
- [ ] Line spacing is comfortable while cooking
- [ ] Dark Mode does not destroy Recipe Detail readability

**Result:** PASS / FAIL

**Notes:**


---

# 6. Grocery

- [ ] Grocery screen purpose is immediately obvious
- [ ] Grocery items are readable
- [ ] Quantities are understandable
- [ ] `Stock` clearly communicates what it does
- [ ] `Remove` clearly communicates what it does
- [ ] Stock moves the item to Pantry
- [ ] Stock removes the item from Grocery
- [ ] Stock does not create duplicates
- [ ] Remove does not modify Pantry
- [ ] Correct item is affected
- [ ] Other Grocery items remain unchanged
- [ ] Controls do not require guessing
- [ ] Empty Grocery state is understandable

**Result:** PASS / FAIL

**Notes:**


## Critical Stock Test

**Item Tested:**  

**Expected Result:**  

**Actual Result:**  

- [ ] Item entered Pantry
- [ ] Item disappeared from Grocery
- [ ] No duplicate was created
- [ ] Other Grocery items were unaffected

**Result:** PASS / FAIL

**Notes:**


---

# 7. Pantry

- [ ] Pantry purpose is obvious
- [ ] Add item works
- [ ] Edit item works
- [ ] Remove item works
- [ ] Quantities behave correctly
- [ ] Qualitative pantry states behave correctly
- [ ] Changes persist
- [ ] Pantry affects recipe matching/recommendations
- [ ] Empty Pantry state is understandable

**Result:** PASS / FAIL

**Notes:**


---

# 8. Leftovers

- [ ] Leftovers purpose is obvious
- [ ] Add leftover works
- [ ] Edit leftover works
- [ ] Remove leftover works
- [ ] Leftovers participate in recipe matching
- [ ] Empty Leftovers state is understandable
- [ ] Leftovers are visually distinct from Pantry inventory

**Result:** PASS / FAIL

**Notes:**


---

# 9. Saved

- [ ] Saved screen purpose is obvious
- [ ] Favorited recipe appears
- [ ] Unfavoriting behaves correctly
- [ ] Saved state persists
- [ ] Empty Saved state is understandable

**Result:** PASS / FAIL

**Notes:**


---

# 10. Navigation

- [ ] Home navigation works
- [ ] Discover navigation works
- [ ] Kitchen navigation works
- [ ] Saved navigation works
- [ ] Back navigation behaves naturally
- [ ] No dead-end screens
- [ ] Navigation labels/icons are understandable
- [ ] Navigation does not unexpectedly lose state

**Result:** PASS / FAIL

**Notes:**


---

# 11. Responsive Layout

**Devices Tested:**

- 
- 
- 

- [ ] Phone layout works
- [ ] Large phone layout works
- [ ] Tablet/expanded layout works where applicable
- [ ] No clipping
- [ ] No excessive dead space
- [ ] Cards remain coherent
- [ ] Text wraps correctly
- [ ] Navigation adapts correctly
- [ ] Controls remain usable
- [ ] Recipe Detail remains coherent across layouts

**Result:** PASS / FAIL

**Notes:**


---

# 12. Accessibility

- [ ] Text contrast is sufficient
- [ ] Buttons have understandable labels
- [ ] Icon-only controls have descriptions
- [ ] Touch targets are comfortable
- [ ] Important information is not communicated by color alone
- [ ] Larger text does not destroy the layout
- [ ] Long text remains readable

**Result:** PASS / FAIL

**Notes:**


---

# 13. Defect Log

| ID | Screen | Severity | Defect | Evidence | Required Fix |
|---|---|---|---|---|---|
| 001 | | P0/P1/P2 | | | |
| 002 | | P0/P1/P2 | | | |
| 003 | | P0/P1/P2 | | | |
| 004 | | P0/P1/P2 | | | |
| 005 | | P0/P1/P2 | | | |
| 006 | | P0/P1/P2 | | | |
| 007 | | P0/P1/P2 | | | |
| 008 | | P0/P1/P2 | | | |
| 009 | | P0/P1/P2 | | | |
| 010 | | P0/P1/P2 | | | |

---

# 14. Final Acceptance

## Critical Defects

- [ ] No P0 defects
- [ ] No P1 defects
- [ ] Only minor P2 polish remains
- [ ] No known functional blockers
- [ ] No known onboarding blockers
- [ ] No known Recipe Detail blockers
- [ ] No known Grocery blockers

## Final Decision

- [ ] ACCEPTED
- [ ] ACCEPTED — MINOR POLISH REMAINING
- [ ] TARGETED FIXES REQUIRED
- [ ] REJECTED

**Reviewer:**  

**Date:**  

**Build:**  

## Final Notes

