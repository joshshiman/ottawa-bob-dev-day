---
layout: carbon
title: "Lab 3: Design with Bob"
---


# ✨ BONUS LAB 3: Advanced Customization

Your app works. Now let's make it truly yours with two powerful customizations:

1. **Change the AI Model** — Experiment with different watsonx.ai models
2. **Design Your UI** — Customize the look and feel with Bob's help

---

## Part 1: Switch Between AI Models

Want to see how different AI models perform? Let's add a model selector to your app!

### Available Models

IBM watsonx.ai offers several Granite models you can use:

- **`ibm/granite-3-8b-instruct`** (default) — Fast, efficient, great for structured data
- **`ibm/granite-3-2b-instruct`** — Smaller, faster, good for simple tasks
- **`ibm/granite-13b-instruct-v2`** — Larger, more capable, better reasoning
- **`meta-llama/llama-3-70b-instruct`** — Very powerful, best quality (slower)
- **`meta-llama/llama-3-8b-instruct`** — Good balance of speed and quality

### Step 1: Add Model Selection to Your App

Open Bob and use this prompt:

```
Add a model selector dropdown to my Streamlit app in the sidebar.
The dropdown should let users choose between these watsonx.ai models:
- ibm/granite-3-8b-instruct (default)
- ibm/granite-3-2b-instruct
- ibm/granite-13b-instruct-v2
- meta-llama/llama-3-70b-instruct
- meta-llama/llama-3-8b-instruct

Store the selected model in st.session_state and pass it to the
invoke_llm function. Update model_gateway.py to accept an optional
model_id parameter that overrides the LLM_NAME from .env.

Add a small info box explaining what each model is good for.
```

### Step 2: Test Different Models

After Bob makes the changes:

1. Restart your app: `streamlit run app.py`
2. Upload the same receipt with different models
3. Compare the results — notice differences in:
   - **Speed** (smaller models are faster)
   - **Accuracy** (larger models catch more details)
   - **Formatting** (some models structure data better)

### Step 3: Find Your Favorite

Try each model and see which one works best for your use case:
- Need speed? → Use `granite-3-2b-instruct`
- Need accuracy? → Use `llama-3-70b-instruct`
- Need balance? → Stick with `granite-3-8b-instruct` (default)

---

## Part 2: Design Your UI with Bob

Now that you can switch models, let's make your app look amazing.

### What Bob Can Do For You

Tell Bob what you want in plain English — here are some ideas:

**🎨 Visual Style**
- "Change the color scheme to match my company's brand colors"
- "Make the app look more professional / modern / minimal"
- "Add a dark mode"
- "Upload a screenshot of our company website and redesign the app to match it"

**🧩 Layout & Organization**
- "Add a 'How it works' section with step-by-step instructions"
- "Reorganize the layout so charts appear side by side"
- "Add a sidebar with app settings"

**✨ Animations & Polish**
- "Add hover effects to all buttons and cards"
- "Add a fade-in animation when results appear"
- "Make the hero banner more visually impressive"

**📊 Data Display**
- "Show a summary table at the top before the detailed results"
- "Add a progress bar while receipts are being processed"
- "Make the confidence score show as a color-coded badge"

---

### How to Use Bob for This

1. Open Bob → switch to **Ask Mode**
2. <img width="981" height="691" alt="Screenshot 2026-06-11 at 2 26 33 PM" src="https://github.com/user-attachments/assets/8987e26a-8bc3-4ca8-b57e-4c7a62ce3997" />

3. Describe what you want — be as specific or as vague as you like
4. Review what Bob suggests, ask follow-up questions, iterate
5. When you're happy, click **Apply** and restart the app

> 💡 **Tip — Brand Matching:**
> Take a screenshot of your company's website or internal portal,
> drag it into Bob, and say:
> *"Redesign my app to match the brand colors and style in this screenshot."*
> Bob will analyze the image and update your CSS automatically.

---

### There's No Wrong Answer

This lab is open-ended on purpose. The goal is to get comfortable
having a conversation with Bob — describing what you want, reacting
to what it produces, and iterating until you're satisfied.

That's the skill. Not copying prompts.

---

## Part 3: Validate Against Government Guidelines

Now let's add a powerful compliance feature — automatically check if expenses follow official Canadian government guidelines!

### What This Does

This feature will:
- ✅ Analyze each expense against official government travel policies
- ✅ Flag expenses that exceed allowable limits
- ✅ Provide specific policy references for non-compliant items
- ✅ Generate a compliance report with recommendations

### Step 1: Add Compliance Checker

Open Bob and use this prompt:

```
Add a government compliance checker to my expense tracker app.

When expenses are processed, also check them against Canadian government
travel guidelines from this official source:
https://www.canada.ca/en/treasury-board-secretariat/services/travel-relocation/travel-government-business.html

For each expense, determine:
1. Is it within allowable limits?
2. Does it require additional documentation?
3. What is the relevant policy section?

Add a new "Compliance Status" column to the results table with values:
- ✅ Compliant
- ⚠️ Needs Review
- ❌ Non-Compliant

Add a "Compliance Report" section that shows:
- Total compliant expenses
- Items flagged for review
- Specific policy violations with links to guidelines
- Recommendations for corrections

Use the watsonx.ai model to analyze expenses against the policy document.
You may need to fetch and parse the policy page, or provide key policy
limits as context to the LLM.
```

### Step 2: Test Compliance Checking

After Bob implements the feature:

1. Upload sample receipts with various amounts
2. Look for the new "Compliance Status" column
3. Review the "Compliance Report" section
4. Click policy links to see official guidelines

### Step 3: Understand Common Guidelines

Key Canadian government travel limits to watch for:

**Meals:**
- Breakfast: $20.35
- Lunch: $20.80
- Dinner: $51.05
- Incidentals: $17.30 per day

**Accommodation:**
- Varies by city (e.g., Ottawa: $126/night standard)
- Major cities may have higher limits

**Transportation:**
- Taxis: Reasonable amounts with receipts
- Rental cars: Economy class, requires justification
- Flights: Economy class for domestic travel

> 💡 **Note:** These are example limits. The actual implementation should
> fetch current rates from the official government website or use the
> latest policy document.

### Step 4: Customize for Your Organization

You can extend this further by asking Bob:

```
Modify the compliance checker to also check against our organization's
internal expense policies. Add these custom rules:
- All expenses over $100 require manager approval
- Office supplies limited to $500 per month
- Equipment purchases require 3 quotes
```

---

## Putting It All Together

You now have:
- ✅ A working government expense tracker
- ✅ The ability to switch between AI models
- ✅ Automatic compliance checking against government guidelines
- ✅ A custom-designed UI that matches your style

### Challenge Ideas

Want to go further? Try these:

1. **Model Comparison View** — Show results from 2 models side-by-side
2. **Performance Metrics** — Track and display processing time for each model
3. **Model Recommendations** — Suggest the best model based on document type
4. **Custom Model Parameters** — Let users adjust temperature and max_tokens
5. **Save Preferences** — Remember user's favorite model and UI settings
6. **Multi-Policy Support** — Check against multiple government departments' policies
7. **Approval Workflow** — Add manager approval for flagged expenses
8. **Policy Updates** — Automatically fetch latest policy limits from government website

---

**Have fun experimenting!** 🚀
