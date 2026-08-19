# ⚡ Automated Purchase Request & Data Extraction Pipeline

An automated data pipeline built with **Power Automate**, **Outlook**, and **Excel Online** to extract unstructured text from incoming email purchase requests and auto-populate structured log tables in real-time.

---

## 🎯 Problem Statement

In operational environments (such as Manufacturing and Engineering), purchase requests (PRs) and fixture orders are frequently submitted via free-text emails by engineers and designers. 

**Key Operational Challenges: **
* **Manual Data Entry Overhead: ** Administrative teams manually read each email and copy-paste details (part numbers, target dates, cost centers) into tracking spreadsheets.
* **Human Error & Tracking Delays: ** High email volume leads to misplaced requests, typos in part numbers, and slow turnaround times.
* **Lack of Real-Time Visibility: ** Lack of a centralized, automatically updated log makes tracking order status and auditing bottlenecks difficult.

---

## 💡 Solution Overview

This project automates the entire ingestion-to-logging workflow:
1. **Trigger: ** Listens for incoming emails in Outlook matching specific subject keywords (e.g., `Purchase Request`, `Fixture`).
2. **Text Parsing (ETL): ** Extracts dynamic fields from the email body using text expressions (anchored parsing) and cleans target strings.
3. **Data Ingestion: ** Inserts the extracted metadata directly into a structured **Excel Online** table with an automatically generated request timestamp and status.

---

## 🛠️ Tech Stack & Tools

* **Automation Engine: ** Power Automate
* **Trigger Source: ** Office 365 Outlook / Gmail
* **Data Destination: ** Excel Online (Office 365) / SharePoint List
* **Core Concepts: ** Workflow Automation, Data Extraction, Text Parsing, ETL

---

## ⚙️ Step-by-Step Implementation

### Step 1: Excel Table Setup
Created a standardized tracker in Excel Online with formatted table columns:
* `Part Number` | `Email Subject` | `Fixture Part Number` | `Quantity` | `Expected Delivery Date` | `Designer` 

### Step 2: Power Automate Trigger & Condition Configuration
* **Connector:** `Office 365 Outlook` / `Gmail`
* **Trigger Event: ** `When a new email arrives (V3) `
* **Sender Filter (Condition): ** Evaluates incoming messages to ensure the email `From` header matches specific design team addresses (e.g., designated designers only) before triggering the extraction pipeline, filtering out irrelevant incoming mail.
  
### Step 3: Generative AI & Natural Language Data Extraction
Standard text manipulation functions (e.g., `Compose`, `substring () `, `index of () `) failed due to high format variance across engineering teams (e.g., inconsistent label naming like `Fixture PN`, `Tool Part No`, and multi-line-item requests). 

To solve this, an **AI Builder Prompt (GPT-4.1 mini)** was integrated into the pipeline:
* **HTML Preprocessing:** Converted raw email body content via `HTML to text` to eliminate background markup.
* **LLM Entity Extraction:** Leveraged structured AI prompts to extract multi-line fixture part numbers, component part numbers, quantities, target delivery dates, and designer names regardless of formatting style.
* **JSON Schema Generation:** Instructed the AI model to output clean JSON arrays handling single or multi-item email requests.
* **JSON Parsing & Iteration:** Used `Parse JSON` followed by an `Apply to each` control loop to iterate through extracted objects and write each fixture line item independently to Excel Online.

### Step 4: Data Insertion
* **Action:** `Add a row into a table` (Excel Online).
* Mapped parsed text outputs into their respective Excel schema columns.
* Set default `Status` column value to `"Logged"`.

---

## 📊 Results & Business Impact

* ⚡ **100% Elimination of Manual Entry: ** Zero manual copy-pasting required for incoming requests.
* 🎯 **100% Data Accuracy: ** Eliminates human typos in critical technical part numbers.
* ⏱️ **Instant Processing: ** Processing time reduced from hours/days down to seconds per email.

---

## 🎥 Workflow Demo

*(Insert GIF or Screenshot of your Power Automate Flow & Excel output here)*
