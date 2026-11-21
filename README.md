# 🥗 NutriWise AI — LLM-Powered Calorie Tracker

A course-end Python/Google Colab project that estimates calories and macronutrients from everyday meal descriptions, including Indian home-cooked food, using a multi-stage fallback pipeline.

## Features

- **Natural-language meal parsing:** quantities, plurals, fractions, and serving units.
- **Local Indian food reference table:** fast estimates for commonly recognized dishes.
- **USDA FoodData Central:** API lookup for supported simple foods.
- **OpenRouter:** LLM fallback for complex or unfamiliar meals.
- **Pydantic:** validated, consistent nutrition outputs.
- **Gradio:** browser-based estimate, save, daily totals, meal history, and delete controls.
- **JSON persistence:** local meal records identified internally by UUID.
- **Graceful failure:** a clearly labeled placeholder when estimation services fail. Placeholders cannot be saved.

## Run in Google Colab

1. Open [NutriWise_AI_Final.ipynb](./NutriWise_AI_Final.ipynb) in Google Colab.
2. Run the cells sequentially, starting with the dependency-installation cell.
3. When prompted, upload `healthy_meal_plans.csv` for the optional dataset-analysis section. The core calorie tracker works without that file.
4. Enter your **OpenRouter API key privately** at the `getpass` prompt (optional for the local and USDA stages).
5. Run the offline test cell, then launch Gradio in the final application cell.
6. Open the newly generated temporary `gradio.live` link. Keep the Colab runtime running while using the app.

The notebook uses a USDA `DEMO_KEY` for example lookups; it has restrictive rate limits. To use your own USDA key, set the `USDA_API_KEY` environment variable.

## Estimation pipeline

```text
Meal description
      |
      v
Local Indian food table (complete/compatible matches)
      |
      v
USDA FoodData Central (simple food, verified match)
      |
      v
OpenRouter LLM (unresolved/complex descriptions)
      |
      v
Low-confidence placeholder (not saveable)
```

Successful outputs include `name`, `calories`, `protein`, `carbs`, `fat`, `note`, `source` and optional `items`.

## Dataset and limitations

The Healthy Meal Plans dataset has **500 rows, 14 columns, and 20 distinct meal names** in the analyzed version. Its nutrient fields are scaled between 0 and 1 and **must not be interpreted as kcal or grams**. We use it for exploratory analysis and meal-name coverage tests rather than clinical nutrition accuracy measurement.

The local food table values are **illustrative reference servings**, and LLM estimates depend on assumptions and model selection. The app is a course demonstration, not a medical or dietary decision tool. Colab's `data/meals.json` disappears when the runtime is reset unless you copy it to persistent storage.

## Security

Never commit API credentials, notebook execution outputs containing secrets, `.env` files, or personal meal logs. The notebook requests the OpenRouter key interactively and does not need a committed API token.

## Project files

- `NutriWise_AI_Final.ipynb` — primary executable notebook (already included in this repository)
- `indian_food_nutrition.csv` — human-readable copy of starter local values
- `requirements.txt` — package dependencies for local experimentation
- `.gitignore` — excludes secrets and local runtime data

**Status:** The core application and offline tests were verified during development. USDA/OpenRouter live results can vary with network access, rate limits, and selected model.
