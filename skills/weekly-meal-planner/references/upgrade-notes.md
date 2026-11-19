# Upgrade notes

What each version changes in an existing setup, for the "Upgrade an existing setup" path in SKILL.md. Apply every version newer than the one the setup is on, oldest first.

How to tell a setup's version:
- App: the `kitchen-week-version` meta tag in its page, else `appVersion` in `meta`/`current`. Neither present means 1.0.0.
- Weekly task: the line "Kitchen Week prompt version X." at the end of its prompt. None means 1.0.0, or a prompt the household has customized.

## 1.1.0: saved favorites

App: republish `assets/app.html` (adds the star, the Favorites tab, and the remove sheet). The `favorites` collection starts empty; nothing to migrate.

Weekly task: if the prompt is the 1.0.0 template with no hand edits other than filled placeholders and inserted modules, rebuild it from the current `scheduled-task-prompt.md`, keeping its app URL, app name, fallback emails, and any modules already in it. Otherwise insert the block below, between `---` lines, just before the paragraph that starts "The app is the Claude artifact", and change nothing else in the prompt.

---
Saved favorites (firm requirement). The app has a "favorites" collection: recipes the household saved with the star. Keep it for good and never delete a document in it. Each document, id = a slug of the title, holds {title, kind, cuisine, protein, time, serves, intro, ingredients, steps, baby, note, savedAt, savedBy, fromWeek, timesPlanned, lastPlanned}; note is the household's change to make next time.
- When reading the household's input, list "favorites". Posts tagged "feedback" that start "Removed" are favorites someone took off the list, with a reason: "Didn't like it" means never plan that dish or a close version again; "Tired of it for now" means leave it out for at least 6 weeks; "Too much work" means lean toward shorter, simpler recipes.
- When choosing meals, saved favorites count as favorites alongside the Favorites preferences, under the same weekly cap and the same never-back-to-back rule; favor the ones with the oldest lastPlanned. To plan a saved favorite, copy its recipe exactly from its document, apply its note, rescale only if servings changed, and keep its exact title. With 3 or more saved favorites, find what they share (proteins, cooking styles, flavors, cooking time, spice) and lean the new dinners toward it while following every other rule.
- When publishing recipes, give every recipe document a favoriteId field: the favorites document id for a saved favorite, else "". Then, for each saved favorite planned, update its document (read it first for its version): lastPlanned = this week's Monday date, timesPlanned + 1, nothing else.
- When setting "meta"/"current", add favoritesTake: with 3 or more saved favorites, one or two plain sentences on what they have in common and how this week's new picks lean that way, with no percentages or counts; otherwise "".
- In the Discussion update post and in your reply, say which saved favorites came back and which new dishes lean on them. While there are none, end the post with: "Tap the star on any recipe you love to save it."
---

After either path, end the prompt with the line "Kitchen Week prompt version 1.1.0."
