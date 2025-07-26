# MoodCook: AI-Powered Mood-Based Recipe App

## Overview
**MoodCook** is a React web app that suggests recipes tailored to your mood and available ingredients. It leverages OpenAI's GPT-4 for recipe suggestions, Firebase Firestore for user data, and Spoonacular for nutritional info.

---

## Features
- **Mood Selection:** Choose your mood with expressive icons (happy, tired, stressed, etc.).
- **Ingredient Input:** Enter available ingredients in a dynamic input field.
- **AI Recipe Suggestions:** GPT-4 suggests recipes matching your mood and pantry.
- **Recipe Details:** View why a recipe fits your mood and see nutritional info (Spoonacular API).
- **User Preferences & History:** Stored securely in Firebase Firestore.
- **Responsive UI:** Clean, mobile-friendly design.

---

## Tech Stack
- **Frontend:** React, Material-UI (or Tailwind CSS)
- **Backend/API:** OpenAI GPT-4, Spoonacular API
- **Database:** Firebase Firestore
- **Auth (optional):** Firebase Authentication

---

## App Structure

```
/src
    /components
        MoodSelector.jsx
        IngredientInput.jsx
        RecipeList.jsx
        RecipeDetail.jsx
        History.jsx
    /pages
        Home.jsx
        RecipeDetailPage.jsx
    /services
        openai.js
        spoonacular.js
        firestore.js
    App.jsx
    index.js
```

---

## Example UI Flow

1. **Home Page:**  
     - Select mood (icon grid)
     - Enter ingredients (chips or text input)
     - Submit to get recipe suggestions

2. **Recipe List:**  
     - View AI-suggested recipes
     - Click a recipe for details

3. **Recipe Detail Page:**  
     - See why it matches your mood (GPT-4 explanation)
     - Nutritional info (Spoonacular)
     - Option to save to favorites/history

---

## Sample Code Snippets

**Mood Selector (React):**
```jsx
// components/MoodSelector.jsx
const moods = [
    { label: "Happy", icon: "😊" },
    { label: "Tired", icon: "😴" },
    { label: "Stressed", icon: "😰" },
    // ...add more moods
];
export default function MoodSelector({ selected, onSelect }) {
    return (
        <div className="mood-selector">
            {moods.map(mood => (
                <button
                    key={mood.label}
                    className={selected === mood.label ? "active" : ""}
                    onClick={() => onSelect(mood.label)}
                >
                    <span role="img" aria-label={mood.label}>{mood.icon}</span>
                    {mood.label}
                </button>
            ))}
        </div>
    );
}
```

**OpenAI API Call:**
```js
// services/openai.js
import axios from "axios";
export async function getMoodRecipes(mood, ingredients) {
    const prompt = `Suggest 3 recipes for someone who feels ${mood} and has these ingredients: ${ingredients.join(", ")}. Explain why each recipe fits the mood.`;
    const response = await axios.post("https://api.openai.com/v1/chat/completions", {
        model: "gpt-4",
        messages: [{ role: "user", content: prompt }],
    }, {
        headers: { Authorization: `Bearer ${process.env.REACT_APP_OPENAI_API_KEY}` }
    });
    return response.data.choices[0].message.content;
}
```

**Firestore Save Example:**
```js
// services/firestore.js
import { db } from "./firebase";
import { addDoc, collection } from "firebase/firestore";
export async function saveSearch(userId, mood, ingredients, recipes) {
    await addDoc(collection(db, "searchHistory"), {
        userId, mood, ingredients, recipes, timestamp: Date.now()
    });
}
```

---

## Getting Started

1. **Clone the repo & install dependencies:**
     ```bash
     npx create-react-app moodcook
     cd moodcook
     npm install firebase axios @mui/material
     ```
2. **Set up Firebase & APIs:**  
     - Add Firebase config to `.env`  
     - Get OpenAI & Spoonacular API keys

3. **Run the app:**
     ```bash
     npm start
     ```

---

## Next Steps
- Add authentication (optional)
- Enhance UI/UX with animations
- Expand mood/ingredient options
- Add sharing features

---

**Happy Cooking! 🍳**
