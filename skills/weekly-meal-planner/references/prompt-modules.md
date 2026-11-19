# Optional prompt modules

Each module adds one behavior to the weekly plan. They are offered in the "Tune your plan" round after the first plan. To turn modules on, paste the text of each chosen module (between its `---` lines) in place of `{{EXTRA_RULES}}` in `scheduled-task-prompt.md`, separated by blank lines, with a blank line before and after the whole block. For a task that already exists, read its prompt with `list_triggers`, add the module text in the same place (just before the paragraph that starts "The app is the Claude artifact"), and save it with `update_trigger`. Never add a module twice.

Modules are written so that they work with any household: they read the location and other details from the preferences.

## Weather

---
Match the week's weather (firm requirement). In step 1, look up the forecast for the household's Location for the coming Monday through Sunday with WebSearch (prefer the national weather service or another forecast page updated this week; fetch the page if the results lack daily detail). For each day note the rain chance and the high. A warm day is dry with a high above the warm-day threshold in the preferences (default 73F or 23C). Every other day is a cool day. If no forecast can be found, plan for the usual weather in that place that month and say so in the summary. In step 4, split the dinners between warming dishes (soups, stews, braises, curries, roasts, hot noodles, warm grain bowls) and cooling dishes (hearty main-dish salads with a protein and a grain or beans, room-temperature bowls, tacos or wraps, cold noodles, no-cook plates) in about the same proportion as cool days to warm days; with no warm days, every dinner is warming. Lean breakfasts and quick lunches the same way. Every variety rule still applies. Because meals have no assigned days, start the summary in step 7c with a short weather line and say which dinners suit the cool stretch and which suit the warm days, and give the weather in a phrase in the email.
---

## Use up leftovers

---
Use up what is left over (firm requirement). The household wants as little food waste as possible.
- In step 1, read the most recent "history" document's leftovers field. Count each leftover as on hand unless a post since notesReadThrough says it was used, eaten, or tossed, or its keeps window has passed by shopping day. Also carry forward older leftovers that keep for months (grains, nuts, seeds, jars, frozen items). These plus food the posts mention are this week's inventory.
- In step 4, build meals around the inventory first, most perishable first, so every still-good leftover lands in at least one recipe. When a recipe needs part of a unit the store sells (half a cabbage, part of a bunch of herbs, part of a carton of broth or tub of yogurt, part of a can of coconut milk), use the rest in another recipe this week or scale the recipe to use the whole unit. Share perishables across recipes rather than buying many different bunches and tubs.
- In step 5, mark an ingredient that comes from last week "(from last week)".
- In step 6, after the list is built, compare what is on hand plus bought with what all recipes use. For each item that will not be fully used, first change a recipe or quantity to use it up; record it only if that is not possible, as {item, qty, keeps}, where keeps is how long it stays good from shopping day ("about 1 week once opened", "3 weeks", "months", "freeze now"). Skip spices, oils, vinegars, and big condiment jars unless perishable once opened.
- In step 7c, start onHand with "Using up from last week:" and each carried-over item with the recipe that uses it. In step 7d, add the leftovers list from step 6 to the history document as leftovers. In step 7e, ask the household to post a note before the next plan if any expected leftover gets eaten or tossed.
---
