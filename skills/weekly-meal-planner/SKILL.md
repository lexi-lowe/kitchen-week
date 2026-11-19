---
name: weekly-meal-planner
description: Sets up a household's own weekly meal planner in Claude - a shared app with a grocery checklist, recipes, saved favorites, a discussion board where Claude answers food questions, and editable preferences, plus a weekly scheduled task that plans varied meals, fills the app, drafts a delivery cart where a connector allows it, and emails a link. Use when someone asks to set up the weekly meal planner, a meal plan app, or a shared grocery list with weekly recipes.
---

# Weekly meal planner setup

Build a private copy of the "kitchen week" app for this person and schedule a weekly planning run. Every preference comes from their answers; nothing from anyone else's household is included.

Files:
- `assets/app.html`: the app page (tabs: Groceries, Recipes, Discussion, Preferences, Favorites).
- `references/scheduled-task-prompt.md`: the weekly task prompt, with placeholders to fill.
- `references/prompt-modules.md`: optional add-ons (weather, use up leftovers) offered in the "Tune your plan" round.
- `references/upgrade-notes.md`: what each version changes in an existing app and task.

If the person asks to upgrade, update, or fix an existing meal planner, or the pre-checks find a setup and they choose to update it, follow "Upgrade an existing setup" at the end instead of the setup touchpoints.

**Keep setup to three touchpoints for the person:** (1) one typed message with the essentials, (2) one round of multiple-choice questions, (3) one confirmation. After the build, offer one optional "Tune your plan" round (touchpoint 4); never make it a requirement. Do everything else yourself, quietly. Only add a follow-up message when an answer is missing, ambiguous, or contradictory, and batch all follow-ups into one message.

## Before asking anything (no questions to the person)

1. Load tools with ToolSearch and check you have: the **Artifact** and **ArtifactData** tools, **scheduled tasks** (`create_trigger`, `list_triggers`, `fire_trigger`), and **Gmail** `send_message`. If Artifact or scheduled tasks are missing, explain that this needs Claude's hosted apps and scheduled tasks (for example in Cowork) and stop. If Gmail is missing, note it for touchpoint 3.
2. Call `list_triggers` and the Artifact tool's `list` action. If any task whose name starts with "Weekly meal plan", or a matching app, already exists, mention it in touchpoint 1 and ask whether to update it or start a separate one. Never silently create a duplicate.
3. Note the session's time zone if the environment gives one.

## Touchpoint 1: one typed message

Open with what they'll get, then ask, with a short reason beside each question so they know why it matters. Send it as one message, close to this wording (adjust the delivery line if no cart connector exists for their region, and add any note from the pre-checks, such as an existing setup):

> Here's what you'll have when we're done:
> - **A plan every week, without you thinking about it.** Each week Claude picks a fresh set of dinners, plus breakfasts and quick lunches, that fit your household and don't repeat what you've had lately.
> - **A shared app** with the recipes (ingredients, steps, and times), a grocery checklist you and anyone you share it with can check off in the store, a place to ask questions or leave notes for next week, and a star to save the recipes you love so Claude brings them back and plans more like them.
> - **An email with the link**, so the week is ready before you shop. If you use Instacart, Claude can also fill your cart for you to review.
>
> It takes three quick replies. First, tell me:
> 1. **Who eats:** adults, and any kids or babies with their birth month and year. *Sets portions and leftovers, and adds kid-friendly versions of each recipe that grow with their age.*
> 2. **Your city.** *So recipes use what's in season near you, in your units, and the plan arrives on your local time.*
> 3. **Allergies or foods to never include** (or "none"). *These are hard rules: they never appear in a recipe, and packaged items get a "check label" flag.*
> 4. **Emails that should get the plan.** *Add a partner or housemate who shops or cooks with you, and I'll make sure they can use the app too.*
> 5. **A few favorite meals.** *They'll come back now and then, mixed in with new dishes so it never feels like the same week twice.*
> 6. **How you shop:** a store, or a delivery service and which store through it. *The grocery list is grouped the way you'll shop, and for Instacart the cart can be filled for you.*

## Touchpoint 2: one multiple-choice round

Use AskUserQuestion with these four questions (people can always type "Other"). Put the recommended option first. Start each question's text with a few words on why it matters, and use option descriptions to say what each choice means for their plan.

1. **Diet** ("Shapes which proteins rotate through the week."): "Omnivore", "Mostly vegetarian: about 1 meat and 2 fish dinners a week", "Pescatarian", "Vegetarian" (they can type something else, such as vegan or "2 meat dinners").
2. **Dinners at home** ("Sets how many recipes and how much to buy; leftovers cover the next day's lunch."): "5 home, 2 out", "6 home, 1 out", "7 home", "4 home, 3 out".
3. **Cuisines** ("Variety is what keeps a meal plan from getting old."): "Rotate widely, a new cuisine every night", "Mostly familiar cuisines", "Mix of both".
4. **Plan arrives** ("Pick a time before you usually shop."): "Sunday morning", "Saturday morning", "Friday evening".

When you send any follow-up questions, keep the same pattern: one short clause on why each answer is needed.

## Fill gaps quietly, then ask only what's left

Derive what you can yourself:
- Time zone from their city (confirm it in the summary, don't ask separately).
- Units from their country.
- Defaults (shown in the summary so they can change them): a dinner can repeat after 3 weeks; at most 2 favorites a week; medium spice; 45-minute weeknight cooking limit; lunches are leftovers; 2 or 3 breakfast ideas; plan time about 7:45am on the chosen day.

Then check for anything missing, vague, or contradictory, and ask about all of it in ONE message. Examples:
- Missing city, emails, or a kid's birth month and year.
- A partner mentioned without an email.
- A delivery service named without a store, or "delivery" without a service.
- A "mostly" or "flexible" diet without numbers, when they typed their own diet answer.
- Kid preferences with no kids; a favorite the diet or an allergy rules out; home dinners plus nights out that don't add up to the week.

If nothing is missing, skip this message entirely.

**Delivery connector:** if they shop through Instacart (or another service with a connector in the registry), call SearchMcpRegistry, and if a connector with cart tools exists, offer it with SuggestConnectors in this same message: "Connect Instacart and each week's plan will fill your cart for you to review and check out." It must be connected **before** the scheduled task is created so the weekly runs can use it. If they decline or no connector exists, the app's Groceries tab still gives them a one-tap "Copy list" to paste into their service.

## Touchpoint 3: confirm

Show a compact summary grouped as it will appear in the Preferences tab: Household, Location and time zone, Allergies and never include, Diet, Variety, Favorites, Weekly structure, Shopping (with whether the cart will be drafted), Email recipients, Schedule. Mention if Gmail isn't connected (no weekly email until it is). Then ask with AskUserQuestion:
- "Looks right: build it and make this week's plan now" (recommended)
- "Looks right: build it, first plan on the schedule"
- "Change something"

On "Change something", make the edits and confirm again. Build only after a "Looks right" answer.

## Build (no questions to the person)

1. **Publish the app.** Copy `assets/app.html` to your working directory. The app is titled "Our Kitchen Week"; if this account already has a meal-plan app (from the pre-checks), give this one a distinct title that the person chose or that names the household (e.g. "The Rivera Kitchen Week"), changing both `<title>` and the `<h1>` in your copy. Keep the title as `{{APP_NAME}}`. Publish it with the Artifact tool:
   - `capabilities`: `{"db": {}, "user": {"scopes": ["profile"]}, "sample": {}}`
   - `icon`: `"cart"`, `description`: "Weekly meal plan hub: shared grocery checklist, recipes, saved favorites, discussion with Claude, and preferences."
   - Keep the returned URL as `{{APP_URL}}`.
   - **Check the capabilities took.** Some Artifact tools publish without accepting `capabilities` (the result may warn that the page uses `db` but doesn't declare it). If so, declare the same capabilities in a separate call right after publishing (for example `declare_capabilities` with the version the publish returned). Then read the declaration back and confirm `db`, `user`, and `sample` are present before writing any data. Without `db` the app can't save anything.
2. **Load preferences** with one ArtifactData `batch` (on servers without ArtifactData, the same writes with `db_write`):
   - `prefs`/`standing`: `{"sections": [{"title": "...", "items": ["..."]}, ...], "updatedAt": "<now ISO>"}` with these titles where they apply: "Household", "Location", "Allergies and never include", "Diet", "Variety", "Favorites", "Weekly structure", "Shopping", "Email recipients". Record each child's birth month and year, not an age. Variety holds the repeat window, cuisine range, spice, favorites cap, and meat and fish dinners per week. Shopping holds the store, the delivery service if any, and "Draft the cart each week" if a cart connector is connected.
   - `meta`/`current`: `{"week": "", "store": "<store or service>", "summary": "Your first plan is on its way.", "onHand": "", "prepAhead": [], "notesReadThrough": "<now ISO>", "orderService": "<delivery service or empty>", "orderUrl": "", "orderNote": "", "favoritesTake": "", "appVersion": "<version from the app's kitchen-week-version tag>"}`
   - If they named meals they're tired of, add `history`/<last Monday as YYYY-MM-DD> with those dinners so the first plan avoids them.
   - Don't create any `favorites` documents. The favorite meals they typed live in the Favorites preference section; the `favorites` collection fills up as they star recipes in the app.
   Read `prefs`/`standing` back once to confirm it saved.
3. **Schedule.** Fill `references/scheduled-task-prompt.md` (fill `{{APP_URL}}`, `{{APP_NAME}}`, and `{{FALLBACK_EMAILS}}`, replace `{{EXTRA_RULES}}` with an empty string, and check no `{{` remains) and call `create_trigger` with name "Weekly meal plan: {{APP_NAME}}" (task names must be unique, so a second household or a reinstall never collides with an earlier task), `cron_expression` in their time zone (e.g. `CRON_TZ=America/Chicago 47 7 * * 0`, avoiding minute 0), the filled prompt, `initiation: human_request`, and `requires_local_device` unset. Keep the returned task id.
4. **First plan.** If they chose "make this week's plan now", call `fire_trigger`.

## Wrap up (one message)

Keep it short:
- The app is in the publish card above and is private to them.
- To share with a partner or housemate: open the app, tap Share, invite them by email as an **Editor**, and don't turn on a public link. Each person needs a Claude account.
- The week: post notes (nights out, guests, what's already in the fridge) and feedback in Discussion before the next plan; check items off in Groceries; tap the star on any recipe they love to save it to Favorites, where Claude brings it back now and then and plans more dishes like it; edit preferences any time.
- If a delivery cart is set up: each week's email and the Groceries tab link to the cart; they review and check out themselves. Claude never places the order.
- Whether the task's runs approve automatically; if not, they should turn on automatic approval in the task's settings so it can run while they're away.
- If they fired the first plan: it runs in its own session and the email arrives in a few minutes.
- End with the touchpoint 4 offer below.

## Touchpoint 4 (optional): tune your plan

People react better to real recipes than to settings, so offer tuning once there is a plan to react to.

1. **Wait for the plan.** If they fired the first plan, check `meta`/`current` about every 2 minutes (up to about 10 minutes) until `week` is filled in, then read the `recipes` collection. Give a two-line recap: the dinners with their cuisines. If the plan isn't there after 10 minutes, or they chose the scheduled first plan, offer tuning anyway and say the plan will reflect it.
2. **Ask one multiple-choice round** with AskUserQuestion (`multiSelect: true`), question text starting with why it matters: "Want to tune it? Each pick changes every future plan." Options, recommended first:
   - "Match the weather": warmer food on cold or rainy weeks, big salads when it's warm.
   - "Use up leftovers": carries last week's extras into the next plan and buys whole units with less waste.
   - "Sourcing rules": organic, grass-fed, no seed oils, or anything else they buy by.
   - "Goals or cooking rules": e.g. high protein, more greens, a cooking technique they always want.
   They can also type "Other", or skip.
3. **Ask only what's left, in one message.** Sourcing rules, goals, cooking rules, and "Other" need their words; weather may need a warm-day threshold if their climate makes 73F wrong. Skip the message if nothing needs detail.
4. **Apply every answer where the weekly run will read it.** Anything said in chat and not saved is lost by the next plan.
   - Preferences: add or update sections "Goals", "Sourcing rules", or "Cooking rules" in `prefs`/`standing` (read it first for its version), one line per rule in their words. Put a weather threshold in "Location".
   - Modules: for "Match the weather" and "Use up leftovers", read the task's prompt with `list_triggers`, insert the module text from `references/prompt-modules.md` as that file explains, and save it with `update_trigger`. Check the module isn't already there.
   - Post one `update` to `posts` (author "claude") listing what changed, so everyone in the app sees it.
5. **Confirm in two lines:** what changed, and that later tweaks work the same way. They can post feedback in Discussion or edit Preferences, and standing requests in Discussion are turned into Preferences lines by asking Claude here.

## Upgrade an existing setup

Setup copies the app into the person's own artifact and the weekly instructions into their own scheduled task, so a new version of this skill never changes an existing setup on its own. This path brings one up to date without losing their data or their own changes. Keep it to one confirmation.

1. **Find the setup (no questions).** Load the Artifact, ArtifactData, and scheduled-task tools. Find the app with the Artifact tool's `list` (title containing "Kitchen Week", or the app they name) and the task with `list_triggers` (a task whose prompt contains the app's URL). If you find more than one app, or none, ask which one in a single question. Note the app's slug, live version, and sharing (`read`) value.
2. **Read both and work out the versions** as `references/upgrade-notes.md` explains.
3. **Spot their own changes.** Compare the app's page with `assets/app.html` from the version it came from, as best you can tell: its title and heading, and small text changes such as labels, wording, or added lines in the prompt Claude uses to answer Discussion questions. Plan to carry each one into the new page. Compare the task's prompt with the template the same way, so you know whether it has been customized. Never rebuild a customized prompt from the template.
4. **Confirm once** with AskUserQuestion. The question names what's new (from the upgrade notes) and what will be kept, e.g. "Upgrade Our Kitchen Week to 1.1.0? Adds saved favorites. Keeps your data, your title, the 'Baby version' label, and your weekly rules." Options: "Upgrade" (recommended), "Not now".
5. **Upgrade the app.** Copy `assets/app.html`, apply their changes from step 3, and republish it over the same artifact (its slug, the live version as the base, and the sharing value if the tool asks for it). Data lives with the artifact, not the page, so groceries, recipes, posts, history, and preferences stay. Check the capabilities, as in Build step 1, and declare them again if they're missing. Then set `appVersion` in `meta`/`current` (read it first for its version).
6. **Upgrade the task.** Follow `references/upgrade-notes.md` for each newer version: rebuild an untouched template prompt, or insert the version's block into a customized one. Save it with `update_trigger`, keeping the schedule and everything else about the task as it was. Read the task back and confirm the block is there once and the version line is updated.
7. **Tell the household.** Post one `update` to `posts` (author "claude") saying what's new and how to use it, then reply with two or three lines: what changed, what was kept, and that the next weekly plan uses it. Offer to run a plan now with `fire_trigger` if they want to see it.

