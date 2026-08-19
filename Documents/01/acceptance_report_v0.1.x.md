# Use It Up — Polish & Acceptance Checklist

**Date:** 2026-08-19  
**Version:** v0.1.x  

## Instructions
Check each item against the actual running application.
- `[x]` = PASS
- `[ ]` = FAIL / NOT ACCEPTED

---

# 1. First Launch & Onboarding
- [x] App immediately explains what Use It Up does
- [x] First action is obvious
- [x] Onboarding is easy to understand
- [x] Each onboarding step has one clear purpose
- [x] Pantry is explained clearly
- [x] Use It Up / Leftovers are explained clearly
- [x] Grocery is explained clearly
- [x] Recipe discovery/recommendations are explained clearly
- [x] User knows what to do after onboarding
- [x] Onboarding completion persists after app restart
- [x] A new user could reasonably use the app without outside explanation

**Result:** PASS

---

# 2. Home
- [ ] Purpose of Home is immediately obvious
- [ ] Recommended recipes are understandable
- [ ] Recommendations feel useful
- [x] Recipe cards are visually consistent
- [ ] Favorite control is obvious
- [x] Favorite animation feels intentional
- [x] Scrolling feels natural
- [ ] No unexplained dead space
- [x] No obvious visual clutter
- [x] Ads do not interfere with normal use

**Result:** FAIL (Targeted fixes required for empty state and "First Fifty Feet" principle)

---

# 3. Discover
- [x] Search is immediately understandable
- [x] Recipe-name search works
- [x] Ingredient search works
- [x] Tag search works
- [x] Multiple search terms work correctly
- [x] Sort/filter controls are understandable
- [x] Results are readable
- [x] Long recipe names render correctly
- [x] Empty results state is understandable
- [x] No clipping
- [x] No excessive dead space

**Result:** PASS

---

# 4. Recipe Detail — Overall Structure
- [x] Header is clearly separated from recipe content
- [x] Hero/identity area is clearly separated
- [x] Prep List is its own card
- [x] Method is its own card
- [x] Action/footer area has its own purpose
- [x] Recipe reads as a page made of distinct cards
- [x] Cards do not feel like one giant container
- [x] Overall hierarchy is immediately understandable

**Result:** PASS

---

# 5. Recipe Detail — Readability
- [x] Recipe title is clearly readable
- [ ] Ingredient names are clearly readable (Inconsistent between recipes)
- [ ] Ingredient quantities are clearly readable
- [x] Method instructions are clearly readable
- [x] Body text does not blend into the background
- [ ] Dark Mode does not destroy Recipe Detail readability

**Result:** FAIL (Data/rendering boundary investigation required for ingredients)

---

# 6. Grocery
- [ ] Grocery screen purpose is immediately obvious
- [x] Stock moves the item to Pantry
- [x] Stock removes the item from Grocery
- [ ] Quantities are understandable (Blocked by ingredient identity)

**Result:** FAIL (Canonical ingredient identity system required)

---

# 7. Pantry
- [x] Pantry purpose is obvious
- [ ] Pantry affects recipe matching/recommendations (Unreliable due to ingredient strings)

**Result:** FAIL (Canonical ingredient identity system required)

---

# 8. Use It Up / Leftovers
- [x] Use It Up purpose is obvious
- [ ] Leftover matching is unreliable without canonical ingredient identities

**Result:** FAIL (Canonical ingredient identity system required)

---

# 9. Saved
- [x] Saved screen purpose is obvious
- [x] Favorited recipe appears
- [x] Saved state persists

**Result:** PASS

---

# 10. Navigation
- [x] Home navigation works
- [x] Discover navigation works
- [x] Kitchen navigation works
- [x] Saved navigation works
- [x] Back navigation behaves naturally

**Result:** PASS

---

# 11. Responsive Layout
- [x] Phone layout works
- [x] Tablet/expanded layout works
- [x] Cards remain coherent

**Result:** PASS

---

# 12. Accessibility
- [x] Text contrast is sufficient
- [ ] Important information is not communicated by color alone (Ingredient dots)

**Result:** FAIL

---

# 13. Final Decision
- [x] TARGETED FIXES REQUIRED

**Reviewer:** Zack
**Date:** 2026-08-19

## Summary of Required Next Steps
1. **Canonical Ingredient Identity**: Separate identity from quantity/source wording to fix matching across Pantry, Grocery, and Leftovers.
2. **First-use Product Flow**: Route user to Pantry after onboarding to drive immediate value.
3. **Data Normalization**: Clean up ingredient string formatting at the data ingestion layer.
