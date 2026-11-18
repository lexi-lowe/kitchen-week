---
name: weekly-meal-planner
description: Sets up a household's own weekly meal planner in Claude - a shared app with a grocery checklist, recipes, a discussion board where Claude answers food questions, and editable preferences, plus a weekly scheduled task that plans varied meals, fills the app, drafts a delivery cart where a connector allows it, and emails a link. Use when someone asks to set up the weekly meal planner, a meal plan app, or a shared grocery list with weekly recipes.
---

# Weekly meal planner setup

Build a private copy of the "kitchen week" app for this person and schedule a weekly planning run. Every preference comes from their answers; nothing from anyone else's household is included.

Files:
- `assets/app.html`: the app page (tabs: Groceries, Recipes, Discussion, Preferences).
- `references/scheduled-task-prompt.md`: the weekly task prompt, with placeholders to fill.

**Keep setup to three touchpoints for the person:** (1) one typed message with the essentials, (2) one round of multiple-choice questions, (3) one confirmation. Do everything else yourself, quietly. Only add a follow-up message when an answer is missing, ambiguous, or contradictory, and batch all follow-ups into one message.

## Before asking anything (no questions to the person)

1. Load tools with ToolSearch and check you have: the **Artifact** and **ArtifactData** tools, **scheduled tasks** (`create_trigger`, `list_triggers`, `fire_trigger`), and **Gmail** `send_message`. If Artifact or scheduled tasks are missing, explain that this needs Claude's hosted apps and scheduled tasks (for example in Cowork) and stop. If Gmail is missing, note it for touchpoint 3.
2. Call `list_triggers` and the Artifact tool's `list` action. If a "Weekly meal plan" task or a matching app already exists, mention it in touchpoint 1 and ask whether to update it or start a separate one. Never silently create a duplicate.
3. Note the session's time zone if the environment gives one.

## Touchpoint 1: one typed message

Open with what they'll get, then ask, with a short reason beside each question so they know why it matters. Send it as one message, close to this wording (adjust the delivery line if no cart connector exists for their region, and add any note from the pre-checks, such as an existing setup):

> Here's what you'll have when we're done:
> - **A plan every week, without you thinking about it.** Each week Claude picks a fresh set of dinners, plus breakfasts and quick lunches, that fit your household and don't repeat what you've had lately.
> - **A shared app** with the recipes (ingredients, steps, and times), a grocery checklist you and anyone you share it with can check off in the store, and a place to ask questions or leave notes for next week.
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

1. **Publish the app.** Copy `assets/app.html` to your working directory and publish it with the Artifact tool:
   - `capabilities`: `{"db": {}, "user": {"scopes": ["profile"]}, "sample": {}}`
   - `icon`: `"cart"`, `description`: "Weekly meal plan hub: shared grocery checklist, recipes, discussion with Claude, and preferences."
   - Keep the returned URL as `{{APP_URL}}`.
2. **Load preferences** with one ArtifactData `batch`:
   - `prefs`/`standing`: `{"sections": [{"title": "...", "items": ["..."]}, ...], "updatedAt": "<now ISO>"}` with these titles where they apply: "Household", "Location", "Allergies and never include", "Diet", "Variety", "Favorites", "Weekly structure", "Shopping", "Email recipients". Record each child's birth month and year, not an age. Variety holds the repeat window, cuisine range, spice, favorites cap, and meat and fish dinners per week. Shopping holds the store, the delivery service if any, and "Draft the cart each week" if a cart connector is connected.
   - `meta`/`current`: `{"week": "", "store": "<store or service>", "summary": "Your first plan is on its way.", "onHand": "", "prepAhead": [], "notesReadThrough": "<now ISO>", "orderService": "<delivery service or empty>", "orderUrl": "", "orderNote": ""}`
   - If they named meals they're tired of, add `history`/<last Monday as YYYY-MM-DD> with those dinners so the first plan avoids them.
   Read `prefs`/`standing` back once to confirm it saved.
3. **Schedule.** Fill `references/scheduled-task-prompt.md` (replace `{{APP_URL}}`, `{{APP_NAME}}`, `{{FALLBACK_EMAILS}}`; check no `{{` remains) and call `create_trigger` with name "Weekly meal plan", `cron_expression` in their time zone (e.g. `CRON_TZ=America/Chicago 47 7 * * 0`, avoiding minute 0), the filled prompt, `initiation: human_request`, and `requires_local_device` unset.
4. **First plan.** If they chose "make this week's plan now", call `fire_trigger`.

## Wrap up (one message)

Keep it short:
- The app is in the publish card above and is private to them.
- To share with a partner or housemate: open the app, tap Share, invite them by email as an **Editor**, and don't turn on a public link. Each person needs a Claude account.
- The week: post notes (nights out, guests, what's already in the fridge) and feedback in Discussion before the next plan; check items off in Groceries; edit preferences any time.
- If a delivery cart is set up: each week's email and the Groceries tab link to the cart; they review and check out themselves. Claude never places the order.
- Whether the task's runs approve automatically; if not, they should turn on automatic approval in the task's settings so it can run while they're away.
- If they fired the first plan: it runs in its own session and the email arrives in a few minutes.
