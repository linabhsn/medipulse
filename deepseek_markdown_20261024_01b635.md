# MediPulse CHUSys Command 🏥

Intelligent Hospital Management & AI Predictive Operations for Moroccan CHUs.

Built for Hacktoberfest 2026 Rabat (MLH Hack Day Event #15514).

## 📌 The Problem

Moroccan public hospitals (CHUs) operate under immense pressure using fragmented, paper-based systems. 

- **Charge nurses** build shift rosters manually, often leading to severe understaffing during unexpected patient surges.
- **Biomedical engineers** discover broken ventilators or monitors only when a patient is already in the ICU.
- **Administrators** struggle to reconcile complex insurance billing (CNSS, CNOPS, AXA) with patient out-of-pocket balances.
- **Emergency coordination** relies on phone trees that take too long during critical Code Red scenarios.

## 💡 The Solution

MediPulse CHUSys Command is a single-file, zero-backend clinical operations dashboard that unifies patient forecasting, biomedical equipment tracking, staff rostering, and financial ledgers into one seamless interface. 

It requires no server, no database, and no build step. Just open the HTML file and run it.

## 🤖 The Role of AI

AI is the core engine of this platform. We integrated two open-weight models to drive predictive staffing and resource allocation.

### 1. DeepSeek (Open-Weight API)
- **Model:** `deepseek-chat`
- **Role:** Analyzes 14 days of historical patient admission data for a specific ward (ER, ICU, etc.) and generates a 7-day forecast.
- **Output:** Returns a strict JSON object containing the forecasted patient numbers and a natural-language clinical rationale.

### 2. Gemma 4 via Google Gemini API
- **Model:** `gemma-4-26b-a4b-it`
- **Role:** Functions identically to DeepSeek but utilizes Google's open-weight Gemma architecture via the Gemini REST API. 
- **Implementation:** Users can toggle between DeepSeek and Gemma 4 directly in the UI header. The app uses a strict system prompt to force Gemma to output raw JSON.

### 🛡️ Transparent Mathematical Fallback
To ensure the platform never fails during a demo or offline scenario, we built a MA7 (7-day Moving Average) fallback engine. If no API key is provided, the system automatically calculates the forecast using the MA7 formula weighted by department-specific multipliers. The UI clearly indicates which model generated the forecast.

## ✨ Core Features

- **AI Forecasting:** Predicts 7-day patient influx and calculates required nurse-to-doctor ratios per shift.
- **Patient Records:** Tracks inpatient details, newborn delivery records, and Moroccan insurance billing.
- **Equipment Tracking:** Monitors biomedical devices and flags defective units with error codes.
- **Staff Roster:** Live check-in/check-out tracking with automatic deficit alerts.
- **Emergency Dispatch:** One-tap call links and broadcast alerts for Code Red scenarios.
- **Judge Console:** A built-in terminal that exposes AI functions for direct verification.

## 🚀 How to Run

1. Clone the repository or download the files.
2. Double-click `index.html` to open it in any modern browser.
3. (Optional) Click **"API Keys"** in the top right to paste your DeepSeek or Google Gemini keys.
4. Use the toggle in the header to switch between **DeepSeek** and **Gemma 4**, then click **Execute Forecast**.
5. To unlock admin features (adding staff/equipment), click **Admin Portal** and enter the PIN: `1234`.

## 🛠️ Tech Stack

- HTML5, Vanilla JavaScript, Tailwind CSS (via CDN)
- DeepSeek API and Google Gemini API
- Chart.js (Data Visualization)
- SheetJS (XLSX import)

## ⚠️ Limitations

- **No Backend:** Data resets upon page reload. This is a front-end prototype.
- **Client-side API Keys:** Keys are stored in the browser's `localStorage`.
- **Synthetic Data:** All patient names and records used in this demo are fictional. Use the "Import Database" button to test with real CSV/XLSX data.

## 👥 Authors

- Lina Bouhsissin
- Hamza Boumarg
- Malak Jaouane

## 📄 License

This project is licensed under the MIT License. See the LICENSE file for details.