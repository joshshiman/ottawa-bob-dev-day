---
layout: carbon
title: "Lab 1: Build the Tracker"
---

# AI Government Expense Tracker — Build Guide

Complete guide for building an AI-powered government expense tracker using IBM Bob and watsonx.ai.

This guide assumes no prior context. Follow every step to build a working application from scratch.

---

## What This Application Does

✅ Upload up to 10 PDF receipts (office supplies, equipment, services, utilities).
✅ Auto-detect document type from filename.
✅ Extract structured expense data using IBM watsonx.ai Granite 3 LLM.
✅ Display results in a table with 8 columns.
✅ Show 4 metric cards (Files Processed, Line Items, Total Amount, Avg Confidence).
✅ Generate 4 interactive Plotly charts (by vendor, by category, by document type).
✅ Export data to CSV.
✅ Generate an AI-written plain-English summary of your government expenses.


---


## Prerequisites

Before starting this lab, make sure you have completed all the prerequisites:

👉 **[Complete Prerequisites →](/ottawa-bob-dev-day/labs/prerequisite/)**

This includes:
- Installing Python 3.10-3.13
- Installing IBM Bob
- Creating IBM Cloud account and IBMid
- Getting your watsonx.ai credentials (API Key, Project ID, Cloud URL)

Once you have your credentials ready, continue below to start building!

---

## Project Structure

You will create these files:

```
ai-government-expense-tracker/
├── app.py                 # Streamlit UI
├── doc_processing.py      # PDF parsing + LLM extraction + charts
├── model_gateway.py       # watsonx.ai REST API layer
├── requirements.txt       # Python dependencies
└── .env                   # Your credentials
```

---

## Step 1: Create Your Project Folder with Bob

Open Bob and paste this prompt:

```
Create a new project folder called "ai-government-expense-tracker" on my Desktop.
Then create a requirements.txt file inside it with these dependencies:

streamlit
pandas
plotly
docling
python-dotenv
requests

Return confirmation when done.
```
<img width="1066" height="770" alt="Screenshot 2026-06-23 at 2 30 18 PM" src="https://github.com/user-attachments/assets/c85c8ce9-c113-4711-b820-6c37d728e473" />



Bob will create the folder and the requirements.txt file for you.

> 💡 **Important**: After Bob creates the folder, navigate to it in your terminal:
> ```bash
> cd ~/Desktop/ai-government-expense-tracker
> ```
> Every time you open a new Terminal window, you must run this command first before doing anything else. If you skip this step, all subsequent commands will fail with "module not found" or "file not found" errors.

---

## Step 2: Install Dependencies

After Bob creates the requirements.txt file, install the packages:

```bash
pip3 install -r requirements.txt
```
---

## Step 3: Create `.env` File

First, make sure you are in your project folder, then create the '.env' (note the dot at the beginning) file, paste these contents: 
```bash

API_KEY=paste_your_api_key_here
PROJECT_ID=paste_your_project_id_here
CLOUD_URL= paste_your_URL_here For example: https://us-south.ml.cloud.ibm.com
LLM_NAME=ibm/granite-3-8b-instruct

```
No output means it worked. Verify the file was created:
```bash
cat .env
```
You should see the four lines above.

3d. Fill In Your Real Credentials


> 🔒 **Security Note**: Never commit `.env` to version control. Add it to `.gitignore`.

---

## Step 4: Generate `model_gateway.py` with Bob (Bob Advanced Model is Recommended)

<img width="1067" height="768" alt="Screenshot 2026-06-23 at 2 18 25 PM" src="https://github.com/user-attachments/assets/49a04130-006a-4633-82e2-141589a54cf2" />


This file handles the connection to watsonx.ai using the REST API.


4a Open Bob and paste this prompt:

```
Generate a Python file called model_gateway.py that connects to IBM watsonx.ai using the REST API (not the SDK).

Requirements:
- Load API_KEY, PROJECT_ID, CLOUD_URL, and LLM_NAME from a .env file using python-dotenv
- Implement IAM token exchange: POST to https://iam.cloud.ibm.com/identity/token to get a Bearer token
- Cache the token for 50 minutes (IBM tokens expire after 60 minutes)
- Expose a single public function: invoke_llm(prompt: str) -> str
- Call the watsonx.ai text generation endpoint: {CLOUD_URL}/ml/v1/text/generation?version=2023-05-29
- Use these generation parameters:
    max_new_tokens: 2048
    temperature: 0.0
    repetition_penalty: 1.05
    stop_sequences: ["```"]

Return only the complete Python file with no explanations.
```


4b. Save the file
Click Apply in Bob, or copy the generated code and save it as model_gateway.py in your project folder.

4c: Verify your API Key
### For Windows
```bash
curl.exe -s -o nul -w "%{http_code}" -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=YOUR_API_KEY"
```
### For Mac
```bash
curl -s -o /dev/null -w "%{http_code}" -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=YOUR_API_KEY"
```
❕Replace `YOUR_API_KEY` with the value from your `.env` file.
 
| Output | Meaning |
|--------|---------|
| `200` | ✅ API Key is valid |
| `400` | ❌ Wrong `PROJECT_ID` — check it's the 36-character UUID |
| `401` | ❌ API Key is invalid — contact your lab organizer |

### 4d. Test the watsonx.ai connection
 
```bash
python3 -c "
from model_gateway import invoke_llm
print(invoke_llm('Say hello in one sentence.'))
"
```
 
**Expected:** LLM replies with a sentence ✅
 
---

## Step 5: Generate `doc_processing.py` with Bob


<img width="1087" height="798" alt="Screenshot 2026-06-23 at 2 20 16 PM" src="https://github.com/user-attachments/assets/d918ef25-7896-479c-ae2d-098d3c0f13ec" />


This file handles PDF parsing and AI-powered data extraction.

Open Bob and paste this prompt:

```
Generate a Python file called doc_processing.py for processing government expense PDF receipts.

Requirements:

PDF parsing:
- Use Docling to convert PDF bytes to Markdown text
- Configure Docling with: do_ocr=True, do_table_structure=True (high-fidelity mode — do NOT disable these)
- Use PdfPipelineOptions and PdfFormatOption to pass options (do NOT pass do_ocr or do_table_structure as direct arguments to converter.convert())
- Correct usage:
    from docling.datamodel.base_models import InputFormat
    from docling.datamodel.pipeline_options import PdfPipelineOptions
    from docling.document_converter import DocumentConverter, PdfFormatOption
    pipeline_options = PdfPipelineOptions()
    pipeline_options.do_ocr = True
    pipeline_options.do_table_structure = True
    converter = DocumentConverter(
        format_options={InputFormat.PDF: PdfFormatOption(pipeline_options=pipeline_options)}
    )
    result = converter.convert(tmp_path)
- Save PDF bytes to a temp file, convert, then delete the temp file
- Cache the DocumentConverter instance at module level to avoid re-initialization on every call

Document type detection:
- Implement _detect_doc_type(filename: str, text: str) -> str
- Detection uses BOTH filename and document text content, with weighted scoring
- Score each candidate type by counting keyword matches in filename (weight 3) and in text (weight 1)
- Keywords per type:
    office_supplies: office, supplies, paper, pens, folders, stationery, toner, ink, printer, desk, chair, filing
    equipment: equipment, computer, laptop, monitor, keyboard, mouse, hardware, software, technology, device, machinery
    services: services, consulting, maintenance, repair, cleaning, security, professional, contractor, vendor, support
    utilities: utilities, electricity, water, gas, internet, phone, telecommunications, energy, power, heating, cooling
- Return the type with the highest score
- If all scores are 0, return "generic"
- No single type has blanket priority — scoring determines winner

LLM extraction (TWO-STEP PROCESS):
- CRITICAL: The prompts must instruct the LLM to proceed in EXACTLY two steps:
    STEP 1: Create a pipe-separated text table
    STEP 2: Convert that table to JSON format
- Implement _get_extraction_prompt(doc_type: str, text: str) -> str that returns the appropriate prompt
- Each prompt must have this structure:

For office_supplies type:
    "Analyze this office supplies invoice and extract all charges. Ignore any [image] tags.
    
    STEP 1: Create a table with these columns separated by | (pipe):
    Date | Vendor | Category | Description | Currency | Amount
    
    Categories: Paper Products, Writing Instruments, Filing & Storage, Desk Accessories, Printer Supplies, Technology Accessories, Furniture, Taxes & Fees, Shipping, Miscellaneous
    
    Rules:
    - One line per charge
    - Date format: YYYY-MM-DD (or leave empty if not found)
    - Amount: numeric only (no currency symbols)
    - Include header row
    - Use | to separate columns
    
    Document:
    {text}
    
    Table:
    
    STEP 2: Now convert the table above into a JSON array. Each row becomes a JSON object with these fields:
    - date (from Date column)
    - vendor (from Vendor column)
    - doc_type (leave as empty string)
    - category (from Category column)
    - description (from Description column)
    - currency (from Currency column)
    - amount (from Amount column)
    - confidence (set to 0.9)
    
    Return ONLY the JSON array with no markdown, no code fences, no explanation:
    ["

For equipment type:
    Same structure but with categories: Computer Hardware, Software Licenses, Peripherals, Networking Equipment, Maintenance, Installation, Taxes & Fees, Miscellaneous

For services type:
    "Analyze this services invoice and extract all charges. Ignore any [image] tags.
    
    STEP 1: Create a table with these columns separated by | (pipe):
    Date | Vendor | Category | Description | Currency | Amount
    
    Categories: Consulting, Maintenance, Repair, Cleaning, Security, Professional Services, Contractor Fees, Taxes & Fees, Miscellaneous
    
    Rules:
    - One line per item/charge
    - Date format: YYYY-MM-DD (or leave empty if not found)
    - Amount: numeric only (no currency symbols)
    - Include header row
    - Use | to separate columns
    
    Document:
    {text}
    
    Table:
    
    STEP 2: Now convert the table above into a JSON array. Each row becomes a JSON object with these fields:
    - date (from Date column)
    - vendor (from Vendor column)
    - doc_type (leave as empty string)
    - category (from Category column)
    - description (from Description column)
    - currency (from Currency column)
    - amount (from Amount column)
    - confidence (set to 0.9)
    
    Return ONLY the JSON array with no markdown, no code fences, no explanation:
    ["

For utilities type:
    Same structure but with categories: Electricity, Water, Gas, Internet, Telephone, Telecommunications, Energy, Taxes & Fees, Service Charges, Miscellaneous

For generic type:
    "Analyze this invoice and extract all charges. Ignore any [image] tags.
    
    STEP 1: Create a table with these columns separated by | (pipe):
    Date | Vendor | Category | Description | Currency | Amount
    
    Categories: Office Supplies, Equipment, Services, Utilities, Maintenance, Professional Services, Technology, Taxes & Fees, Miscellaneous
    
    Rules:
    - One line per charge
    - Date format: YYYY-MM-DD (or leave empty if not found)
    - Amount: numeric only (no currency symbols)
    - Include header row
    - Use | to separate columns
    
    Document:
    {text}
    
    Table:
    
    STEP 2: Now convert the table above into a JSON array. Each row becomes a JSON object with these fields:
    - date (from Date column)
    - vendor (from Vendor column)
    - doc_type (leave as empty string)
    - category (from Category column)
    - description (from Description column)
    - currency (from Currency column)
    - amount (from Amount column)
    - confidence (set to 0.9)
    
    Return ONLY the JSON array with no markdown, no code fences, no explanation:
    ["

Vendor extraction:
- Implement _extract_vendor_from_text(text: str) -> str
    - Look for vendor name in the first 10 lines of the document text
    - Skip lines that match common header keywords: invoice, folio, date, page, guest number, charges, credits, description
    - Return the first line containing at least one letter and at least 3 characters
    - Fallback: return the first sequence of capitalized words found in the top lines
    - Return "Unknown" only if nothing is found
- Implement _normalize_expenses(rows: list, filename: str, text: str) -> list
    - If vendor field is empty or "Unknown", call _extract_vendor_from_text(text) and fill it in
    - Always set doc_type from _detect_doc_type(filename, text) — do NOT rely on the LLM's returned doc_type field
    - Use this mapping for doc_type: office_supplies -> "Office Supplies", equipment -> "Equipment", services -> "Services", utilities -> "Utilities", generic -> ""
    - Strip whitespace from all string fields
    - Backfill any missing required fields with empty string (for strings) or 0.0 (for amount/confidence)

Amount parsing:
- Implement _parse_amount(amount_str) -> float
- Handle currency symbols: $, €, £, ¥, ₹
- Handle thousands separators: 1,234.56
- Handle European decimal format: 1.234,56 and 1,5 (comma as decimal separator with any number of decimal digits)
- Always apply abs() to the final parsed amount (all expenses are positive)
- Return 0.0 if parsing fails

JSON parsing:
- Implement _parse_json_from_llm(llm_output: str) -> list
- Prepend "[" to the output before parsing (because the prompt ends with "[" to prime the LLM)
- Strip any markdown code fences (``` or ```json) before parsing
- Strategy 1: use brace-depth scanning to find the [...] array boundary and parse it
- Strategy 2: extract all top-level {...} objects using brace counting and parse each individually
- Strategy 3: attempt json.loads() on the full cleaned output as fallback
- Return an empty list [] if no valid JSON is found — do not raise exceptions

File caching:
- Cache processed files at module level using a dict mapping MD5 hash -> extracted rows
- On each file, compute MD5 of the PDF bytes and check the cache before calling Docling or the LLM
- Export a clear_cache() function that resets the file cache dict to {}

Output functions:
- process_invoices(uploaded_files, max_workers: int = 2, progress_callback=None) -> tuple[pd.DataFrame, dict[str, str]]
    - Process files in parallel using ThreadPoolExecutor(max_workers=max_workers)
    - After each file completes (success or failure), call progress_callback(completed, total, filename) if provided
    - Catch per-file exceptions and store them in debug_info as "ERROR: {exception}"
    - If a file produces 0 normalized rows and no exception, set its debug string to "ERROR: 0 rows extracted — {debug details}"
    - Return (DataFrame, debug_info) where debug_info maps filename -> debug string
    - DataFrame columns: Date, Vendor, Doc Type, Category, Description, Currency, Amount, Confidence
    - If no rows at all, return an empty DataFrame with those columns

- analyze_invoices(df, budgets: dict | None = None) -> tuple of 4 Plotly figures:
    1. Horizontal bar chart: total expenses by vendor, sorted ascending, color #3B82F6
    2. Donut chart: expenses by category (hole=0.4)
    3. Bar chart: expenses by document type (Office Supplies=#3B82F6, Equipment=#A855F7, Services=#10B981, Utilities=#F59E0B)
    4. Grouped bar chart: Budget vs Actual per category (Office Supplies, Equipment, Services, Utilities)
       - "Actual" bars colored per category; "Budget" bars use rgba(0,0,0,0.15) fill with colored border (width 2)
       - barmode="overlay", legend horizontal at y=1.1
       - If budgets is None, use {Office Supplies: 0, Equipment: 0, Services: 0, Utilities: 0}

- CRITICAL: Bar charts and pie/donut charts must use SEPARATE layout config objects
    - Do NOT apply xaxis or yaxis to any pie or donut chart — this causes a crash
    - Bar chart layout: include xaxis, yaxis, font, plot_bgcolor, paper_bgcolor
    - Pie/donut chart layout: include only font, plot_bgcolor, paper_bgcolor (no axis keys)
- All charts: transparent background (rgba(0,0,0,0)), Inter font

Use only straight ASCII quotes (" and ') throughout. Do not use curly or smart quotes.
Include all return statements explicitly — do not omit any return.
Return only the complete Python file with no explanations.
```
----

## Step 6 — Generate `app.py` with Bob


<img width="1087" height="798" alt="Screenshot 2026-06-23 at 2 21 28 PM" src="https://github.com/user-attachments/assets/e5385aad-fa06-4c2d-a9b5-6cca4936d5c2" />


Open Bob and paste this prompt:
```
Generate a complete Python file called app.py for a Streamlit web application named "AI Government Expense Tracker".

Requirements:

Imports:
- import streamlit as st
- import pandas as pd
- from doc_processing import process_invoices, analyze_invoices, clear_cache
- from model_gateway import invoke_llm

General rules:
- Return only the complete Python file content.
- Do not include explanations.
- Use only straight ASCII quotes (" and ').
- Do not use curly quotes.
- Include all required helper functions directly inside app.py.
- Do not assume any external helper exists unless explicitly listed above.
- Do NOT include Astra DB, database connections, authentication, or chat interface.
- Do NOT include budget sidebar, budget inputs, or budget tracking.
- Keep the code clean, runnable, and self-contained except for the imported modules above.

Page setup:
- Use st.set_page_config with:
  - page_title="AI Government Expense Tracker"
  - page_icon="🏛️"
  - layout="wide"

Styling:
- Add custom CSS using st.markdown(..., unsafe_allow_html=True)
- Use Inter font
- App background color: #F1F5F9
- Cards should have white background, rounded corners, light border, and subtle shadow
- Hide the Streamlit footer and main menu
- Create a hero banner with:
  - dark gradient background from #0F172A to #1D4ED8
  - app title
  - short subtitle
  - a badge reading "Powered by IBM watsonx.ai"

Session state:
- Initialize the following if not already set:
  - st.session_state.df = None
  - st.session_state.summary = None
  - st.session_state.processed_hashes = set()

Helper function:
- Define a function generate_summary(df: pd.DataFrame) -> str inside app.py
- This function must:
  - compute total amount
  - compute number of line items
  - compute category breakdown as category name + subtotal
  - compute top vendor and top vendor amount
  - compute document type breakdown
  - compute date range and average daily spend
  - handle date parsing errors gracefully with try/except
  - build a prompt and send it to invoke_llm()
  - strip markdown remnants from the model response, including **, ##, *, and leading -
  - store the cleaned result in a variable named summary
  - return summary explicitly

Summary prompt requirements:
- The prompt inside generate_summary(df) must instruct the LLM exactly as follows in meaning:
  - You are a government expense analyst.
  - Write exactly 3 short sentences.
  - Cover only:
    1. total spend and date range
    2. largest spending category and top vendor
    3. one specific actionable recommendation to reduce costs
  - Do not restate every number.
  - Do not use markdown.
  - Do not use bullet points.
  - Do not use headers.
  - Do not use bold text.
  - Do not add preamble, commentary, self-evaluation, or revision notes.
  - Return plain text only.

Main layout:
- Show hero banner at the top
- Below the banner, show a file uploader:
  - label should clearly indicate PDF upload
  - accept PDF only
  - allow multiple files
  - maximum 10 files
- If more than 10 files are uploaded, show an error and truncate to the first 10

Buttons:
- Create exactly 4 controls in one row:
  1. Submit (primary)
  2. Analyze (secondary)
  3. Generate Summary (secondary)
  4. Export CSV — a st.download_button that appears only when st.session_state.df is not None
- Also include a Clear All button that:
  - resets st.session_state.df to None
  - resets st.session_state.summary to None
  - resets st.session_state.processed_hashes to an empty set()
  - calls clear_cache()
  - calls st.rerun()

Submit behavior:
- When Submit is clicked:
  - if no files are uploaded, show a warning
  - filter out files whose MD5 hash (computed from file.getvalue()) is already in st.session_state.processed_hashes
  - if all files were already processed, show a warning and stop
  - for new files only:
    - show a progress bar with st.progress(0) and a status text placeholder
    - define a progress_callback(completed, total, filename) function that updates the progress bar and status text
      with text like: "Processing file 2 of 3: office_supplies.pdf..."
    - call: df, debug_info = process_invoices(new_files, max_workers=2, progress_callback=progress_callback)
    - after processing:
      - clear the progress bar and status text
      - append new rows to st.session_state.df using pd.concat (or assign directly if df was None)
      - add successfully processed file hashes to st.session_state.processed_hashes
        (a file is successful if its filename does not appear in debug_info with a value starting with "ERROR")
      - reset st.session_state.summary to None
      - show a success message
      - if any files failed, show a warning listing their filenames

Results section:
- Show this section only if st.session_state.df is not None and not empty
- Display exactly 3 metric cards:
  1. Files Processed (from len(st.session_state.processed_hashes))
  2. Line Items (from len(df))
  3. Total Amount formatted as $X,XXX.XX

DataFrame display:
- Show a styled dataframe with emoji column headers
- Use exactly these 7 display columns in this order:
  - 📅 Date
  - 🏢 Vendor
  - 📄 Doc Type
  - 🏷️ Category
  - 📝 Description
  - 💱 Currency
  - 💵 Amount
- Map from raw columns: Date, Vendor, Doc Type, Category, Description, Currency, Amount
- Do not include Confidence anywhere
- Use st.dataframe with use_container_width=True and hide_index=True

Analyze behavior:
- When Analyze is clicked:
  - if no processed data exists, show a warning
  - otherwise call: vendor_chart, category_chart, doc_type_chart, _ = analyze_invoices(st.session_state.df)
  - display the 3 charts (vendor, category, doc type) using st.plotly_chart(..., use_container_width=True)
  - do NOT display the 4th chart (budget vs actual) — there are no budget inputs in this app

Generate Summary behavior:
- When Generate Summary is clicked:
  - if no processed data exists, show a warning: "Please upload and submit receipts first"
  - otherwise:
    - show st.spinner("Generating AI summary...")
    - call generate_summary(st.session_state.df)
    - store the result in st.session_state.summary
- If st.session_state.summary is not None and not empty, display it using st.info()

Export CSV behavior:
- If processed data exists:
  - convert st.session_state.df to CSV without index
  - expose through st.download_button with file_name="expenses.csv" and mime="text/csv"

Error handling:
- Do not crash if date parsing fails
- Do not crash if some expected columns are missing — safely create them with empty strings before display
- All return statements must be explicit
- The final code must be runnable as a Streamlit app

Return only the complete Python file and nothing else.

```


## Pre-Run Validation

Before starting the app, run these three commands to catch issues early:

### 1. Check Python Version

```bash
python3 --version
# Expected: Python 3.10.x, 3.11.x, 3.12.x, or 3.13.x
```

### 2. Test API Key

```bash
curl.exe -s -o nul -w "%{http_code}" -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=YOUR_API_KEY"
```

Replace `YOUR_API_KEY` with your actual key. Expected output: `200`

- Got `400`? Wrong `PROJECT_ID` in `.env` — check it's the 36-character UUID
- Got `401`? API Key is invalid — regenerate it in IBM Cloud

### 3. Test All Imports

```bash
python3 -c "import streamlit, pandas, plotly, docling, requests, dotenv; print('✅ All OK')"
# Expected: ✅ All OK
```

---



## Run the Application

### First-Run Warning

⚠️ **IMPORTANT**: The first run will download Docling ML models (~2GB). This takes 5–10 minutes depending on your internet speed. Subsequent runs are instant. **Do not interrupt the process.**

### Start the App

```bash
streamlit run app.py
```

If `streamlit` is not on your PATH:

```bash
python3 -m streamlit run app.py
```
For example:
```bash
cd /Users/austinzhang/Desktop/Test\ Bob && streamlit run app.py
```

The app opens automatically at **http://localhost:8501**.

<img width="1227" height="712" alt="Screenshot 2026-06-23 at 2 24 01 PM" src="https://github.com/user-attachments/assets/0660675d-e7b7-4d28-a910-d8c859c5aeb2" />



## Usage

1. **Upload PDFs**: Click the file uploader and select up to 10 government expense receipt PDFs
2. **Submit**: Click "⚡ Submit" to extract data (takes 10–30 seconds per file)
3. **View Results**: See table with 8 columns and 4 metric cards
4. **Analyze**: Click "📊 Analyze" to generate 3 interactive charts
5. **Summarize**: Click "📝 Generate Summary" to get an AI-written summary of your government expenses
6. **Export**: Click "⬇️ Export CSV" to download the extracted data

---

---

## Tips for Success

- **Use Ask Mode first** — if you don't understand what Bob generated, switch to Ask Mode and paste the code to get an explanation
- **One file at a time** — generate and test each file before moving to the next
- **Rename your PDFs** — include keywords like `office_supplies.pdf` or `equipment_purchase.pdf` so the app detects the type correctly
- **If Bob's output looks cut off** — ask it to regenerate: *"Please regenerate the complete file, do not truncate"*
- **Use the solution folder** — if you're stuck, `solution/` has a working reference implementation

---

## If you still have time, check LAB2 and LAB3.

## Congratulations! 🎉

You've extended your AI Government Expense Tracker with a full budget tracking system — all built using IBM Bob.

Your app now has:
- PDF parsing using IBM Docling
- AI extraction using watsonx.ai Granite 3
- A clear UI design
- Interactive charts with Plotly
- Detailed Summary of all your government spending
- Easy to download CSV version
- Budget tracking and overspend alerts



---

## Support

If you encounter issues not covered in this guide:

1. Check the [Cheat Sheet](/ottawa-bob-dev-day/labs/cheat-sheet/) for quick fixes
2. Verify all three Pre-Run Validation commands pass
3. Ensure your `.env` file has correct credentials
4. Try with a single, simple PDF first
5. Check the `solution/` folder as a last resort

---

**End of Build Guide**

You now have a complete, working AI Government Expense Tracker. Happy building! 🚀
