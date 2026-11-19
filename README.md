# Kitchen Week

Kitchen Week sets up a weekly meal planner for your household. Once a week, Claude plans a varied set of home-cooked dinners, breakfasts, and quick lunches around your diet, allergies, favorites, and schedule. It puts the recipes and a grocery checklist in a private app you can share with a partner or housemate, and emails you a link.

## What you get

- **Groceries:** a shared checklist grouped by store section. Anyone you share the app with can check items off (checked items drop to the bottom), add extra items, and copy the list to paste into a delivery service.
- **Recipes:** each week's recipes with ingredients, steps, and times, plus a version for babies or young children when your household has them.
- **Favorites:** tap the star on any recipe to save it for good, with a note for next time ("double the sauce"). Claude brings saved recipes back now and then, exactly as saved, and leans new dishes toward what your favorites have in common. Removing one asks why, so the next plan learns from that too.
- **Discussion:** ask Claude questions about the week's food, leave notes for next week (nights out, guests, food already in the fridge), and give feedback (make again, skip next time).
- **Preferences:** your standing preferences, editable any time.
- **Variety built in:** no dinner repeats within your repeat window, a different cuisine each night, rotating proteins, cooking styles, and starches, and seasonal produce for where you live.
- **Delivery cart (optional):** if you shop on Instacart and connect the Instacart connector, each plan adds the week's groceries to your Instacart cart for you to review. Claude never places, pays for, or checks out an order.

## Set it up

Install Kitchen Week from the Claude directory, then ask Claude: "Set up my weekly meal planner." Setup takes three short replies: one typed message with the basics, one round of multiple-choice questions, and a confirmation. Claude then publishes your app, saves your preferences, schedules the weekly plan, and can make this week's plan right away. Once the first plan is in, Claude offers an optional round to tune it: match meals to the weather, use up last week's leftovers, and add sourcing rules or goals.

## Updating

A new version of Kitchen Week doesn't change an app you already set up; your app and weekly task are your own copies. To get new features, ask Claude: "Upgrade my meal planner." Claude finds your app and weekly task, shows what's new, and updates both after one confirmation. Your groceries, recipes, discussion, preferences, and your own changes stay.

## Requirements

- A Claude plan where Claude can publish hosted apps (artifacts) and run scheduled tasks, such as Cowork. The skill checks for these first and tells you if something is missing.
- Code execution turned on ("Create and edit files" in Settings > Capabilities).
- The Gmail connector, for the weekly email. Without it, the app still works but nobody is emailed.
- Optional: the Instacart connector, to draft a cart each week.
- To share the app, invite each person by email as an Editor from the app's Share menu. Each person needs a Claude account.

## Data and privacy

This plugin contains one skill and an HTML app template. It runs no scripts, starts no servers, and sends nothing to any service run by the plugin author.

- **What it collects:** during setup Claude asks for household details (who eats, children's birth month and year, city), allergies, diet and food preferences, favorite meals, how you shop, and the email addresses that should get the plan.
- **Where it is stored:** in the database of your own private Claude-hosted app, which only you and the people you share it with can open. Discussion posts, grocery check-offs, and saved favorites are stored there too.
- **What is sent where:**
  - The weekly plan runs as a scheduled task on your Claude account.
  - Gmail: one email per week to the recipients you choose, sent from your own Gmail account through the Gmail connector.
  - Instacart (only if you connect it): grocery item names and quantities, to search products and fill your cart at the store you chose.
  - Claude: questions asked in the Discussion tab, together with that week's recipes, saved favorites, and your preferences, are sent to Claude on the asking person's own Claude account to get an answer.
  - Web search (only if you turn on "Match the weather"): each weekly run searches the web for your city's forecast. Only the city is used in the search.
- **Removing it:** delete the app and its "Weekly meal plan: <app name>" scheduled task from your Claude account at any time.
