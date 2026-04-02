# 4Padel Inventory System

A lightweight web app for real-time stock control at 4Padel Berlin, built as an internal operations tool to replace manual counting and eliminate inventory blind spots.

---

## The Problem

4Padel Berlin operates with a rotating team of minijobbers across morning and afternoon shifts. Before this system, stock counting was done manually — either on paper or not at all — which led to three recurring issues:

- **No visibility between shifts:** there was no structured record of how many units of each product were available at any given time.
- **Reactive restocking:** orders were placed reactively, often after running out, rather than based on actual consumption trends.
- **No accountability trail:** it was impossible to know who had counted what, or when the last count had taken place.

The club needed a simple, low-friction solution that any staff member could use in under two minutes, without training and without accessing any internal systems.

---

## The Solution

A single-page web app hosted on GitHub Pages, accessible from any browser on the club tablet. No installation required.

The app allows each staff member to:
1. Log in with their name and a personal PIN
2. Select their shift (morning or afternoon)
3. Enter the current quantity of each product
4. Submit — which writes a new row to a shared Google Sheet in real time

Each submission generates a unique entry ID in the format `YYYYMMDD-HHMM-XX` (date, time, and staff initials), ensuring full traceability.

### Tech stack

- **Frontend:** vanilla HTML, CSS, JavaScript — no frameworks, no dependencies beyond Chart.js
- **Auth:** Google OAuth 2.0 via Google Identity Services
- **Data layer:** Google Sheets API v4 (append rows on submit, read rows for dashboard)
- **Hosting:** GitHub Pages (free, zero maintenance)

---

## How It Works in Practice

**At the start of each shift**, the staff member on duty:
1. Opens the app on the club tablet
2. Selects their name and enters their 4-digit PIN (based on their birthday DD/MM)
3. Selects the shift — morning or afternoon
4. Walks through the product list and enters the current physical count for each item
5. Reviews and confirms the submission

The entire process takes under 2 minutes. The app is designed for tablet use with large tap targets, +/− buttons for quick adjustments, and a clean single-column layout.

**On submission**, the app:
- Generates a unique entry ID
- Appends a new row to the `actual` tab in the connected Google Sheet
- The row contains: ID, timestamp, staff name, shift, and one column per product (19 total)

---

## Downstream Control Logic

The Google Sheet connected to this app contains a separate logic layer (not part of this repository) that:

- **Identifies the latest entry** per shift using the entry ID and timestamp
- **Calculates consumption** by comparing consecutive entries across shifts
- **Triggers restock alerts** when any product falls below a defined threshold
- **Sends automated email notifications** to the operations manager when restocking is needed

This separation of concerns — the app only writes raw data, the Sheet handles all business logic — keeps the frontend simple and the control logic easy to modify without touching the app.

---

## Dashboard

The app includes a built-in mini dashboard visible after login:

- Total number of stock entries recorded
- Name of the last staff member to submit
- A line chart showing the trend of any selected product across the last 10 entries

---

## Project context

This tool was self-initiated as part of my operations improvement work at 4Padel Berlin. It was designed, built, and deployed independently — from identifying the operational gap to shipping a working solution used by the full team.

It reflects my approach to ops problems: find the simplest solution that actually gets used, connect it to existing workflows, and make the data actionable downstream.

---

*Built by Facundo Zayas — Industrial Engineer, Data & Operations Analyst*  
*Berlin, 2026*
