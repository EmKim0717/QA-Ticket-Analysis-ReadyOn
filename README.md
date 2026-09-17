QA Ticket Analysis Web Application


Prompted / Authored By	Kimberly Orense
Created Date	September 17, 2026
Revision Date	September 18, 2026
Version	2.0

Target Platform: Web Browsers (macOS / Windows / Linux)
Architecture: Single-Page Application (SPA) / Client-Side Web Application
1. System Overview & Architecture
The QA Ticket Analysis application is a single-page, client-side web tool designed for Quality Assurance (QA) Analysts, Support Leads, and Service Desk Managers. It automates ticket evaluation by combining automated Regex text extraction, AI-powered ticket analysis, ITIL/ITSM Service Management principles, and internal Quality Assurance SOP guidelines.
1.1 Technology Stack
●	Frontend Framework / UI Markup: HTML5, CSS3, JavaScript (ES6+), React 18 (via UMD CDN).
●	Styling & Theme: Tailwind CSS (CDN), custom CSS variables (Theme: Pink #fdf2f8 / #db2777 & White).
●	Component Framework & Icons: Lucide React / Lucide Icons library.
●	AI Engine Integration: Google Gemini API (gemini-3-flash-preview / @google/genai).
●	Data Visualization: Chart.js (CDN) for QA theme analytics and SLA aging visualisations.
●	Data Processing & Export:
○	xlsx-js-style for styled Excel (.xlsx) row export.
○	Tab-Separated Values (TSV) clipboard engine for 1-click Google Sheets integration on macOS.
●	Data Persistence: Browser localStorage for offline ticket state management and custom SOP persistence.
2. Functional Requirements
2.1 Ticket Ingestion & Automated Parsing
●	Pasted Conversation Parsing:
○	Must extract ticket metadata automatically when a Zendesk transcript or email thread is pasted into the conversation container.
●	Customer / Requester Name: Extracted via Regex matching standard headers (e.g., Customer Name, June 29 2026 8:00).
●	Assignee / Support Agent Name: Extracted via regex matching secondary reply lines.
●	Requested Date (Open Date): Extracted from the earliest detected timestamp in the pasted thread.
●	Updated Date (Last Activity): Extracted from the latest detected timestamp in the pasted thread.
●	Manual Override: Input fields for Subject, Requester, Assignee, Requested Date, Updated Date, Priority, and Conversation details remain manually editable prior to saving.
2.2 AI Co-Analyst Module
●	LLM Engine: Integrates Google Gemini API (gemini-3-flash-preview).
●	Dual Framework Evaluation: Evaluates ticket interaction against two benchmark frameworks simultaneously:
○	ITIL v4 / ITSM Standards: Incident vs. Service Request classification, SLA management, Root Cause analysis, Knowledge-Centered Service (KCS), and End-to-End Ownership.
○	Operational QA SOP: Specific communication rules, stop-the-clock macros, unredacted screenshot checks, and non-negotiable process flags.
●	Automated Output Fields: Automatically populates:
○	What was missed: Identifies operational, process, or communication gaps.
○	What should have been done: Actionable corrective steps for the agent.
○	QA Theme: Categorizes issues (e.g., SLA Breach, Improper Triage, Documentation Failure, Communication Gap, Product Knowledge).
○	QA Notes: Qualitative summary and coaching recommendations.
2.3 SLA Color-Coding Logic
The application automatically calculates SLA aging based on Hours Since Updated (Date.now() - Updated Date):
SLA Status	Aging Criteria	Action / Visualization
Green	<= 48 Hours	Updated within 48 hours.
Yellow	> 48 & <= 96 Hours	Updated between 48 and 96 hours.
Red	> 96 Hours	SLA Breach / Stalled ticket.
2.4 Navigation & Views
●	Add & Analyze Ticket: Interactive form, automated Zendesk parser, AI auto-fill button, and clear/reset options.
●	All Tickets List: Multi-column interactive table with sorting, search filtering, inline editing, SLA color highlights, and ticket deletion.
●	Grouped by Assignee: View displaying individual support agent performance cards, ticket totals, top recurring QA themes, and assigned ticket breakdowns.
●	QA SOP & ITSM Standards: Documentation hub containing ITIL v4 principles, 9 Zendesk QA categories, Non-Negotiable flags, QIN protocols, Dispute workflows, and an editable SOP manager.
●	QA Analytics Dashboard: Dynamic charts rendering QA theme distribution, SLA compliance breakdown, and performance trends via Chart.js.
2.5 Data Export & macOS / Google Sheets Integration
●	Excel (.xlsx) Export:
○	Generates native .xlsx files using xlsx-js-style.
○	Preserves full multi-column layout (ID, Subject, Requester, Priority, Requested, Updated, Assignee, What was missed, What should have been done, QA Theme, QA Notes, Hours Since Updated, Ticket Age).
○	Row-Level Cell Styling: Applies full-row background fills matching the exact SLA status color (Green, Yellow, Red) for every cell in the row.
●	Google Sheets / Mac Clipboard Engine:
○	Provides 1-click "Copy for Google Sheets" functionality converting table data to TSV (Tab-Separated Values).
○	One-click action opens https://sheets.new in a new tab for seamless paste (⌘ + V) on macOS devices.
2.6 Interactive AI Chatbot
●	Slide-out conversational panel accessible from all views.
●	Pre-loaded with current ticket dataset context, active QA SOP guidelines, and ITIL framework definitions.
●	Supports real-time Q&A, agent coaching draft generation, QIN draft creation, and ticket trend synthesis.
3. UI/UX & Responsive Layout Requirements
3.1 Display & Spacing
●	Full-Width Layout: Edge-to-edge canvas design (w-full px-4 sm:px-8 lg:px-12) optimized for large monitor displays, Mac Laptops (MacBook Air/Pro), and multi-window workflows.
●	Typography & Styling: Apple System Font Stack (-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto), optimized touch targets, soft pink background accents (#fdf2f8), pink buttons (bg-pink-600), and dark pink header text (text-pink-900).
●	Non-Blocking Toasts: Uses custom macOS-style floating toast notifications for user actions (copy success, delete confirmation, AI completion) instead of blocking modal alerts.
4. Data Model (Schema Definition)
{
  "Ticket": {
    "id": "String (Unique / Generated TS)",
    "subject": "String",
    "requester": "String",
    "assignee": "String",
    "priority": "Enum ('Low', 'Medium', 'High', 'Urgent')",
    "requestedDate": "ISO-8601 DateTime String",
    "updatedDate": "ISO-8601 DateTime String",
    "conversation": "String (Raw pasted text)",
    "whatWasMissed": "String",
    "whatShouldHaveBeenDone": "String",
    "qaTheme": "String",
    "qaNotes": "String",
    "hoursSinceUpdated": "Number (Float)",
    "ticketAgeDays": "Number (Float)",
    "slaStatus": "Enum ('green', 'yellow', 'red')"
  }
}
5. Non-Functional Requirements
●	Performance: Client-side parsing and Regex extraction executed within <100ms. AI processing completed within 2s - 5s depending on network latency.
●	Security & Privacy: API keys stored locally in runtime memory or browser configuration; no ticket data sent to third-party databases except direct encrypted payloads to the Google Gemini API endpoint.
●	Browser Compatibility: Google Chrome 100+, Apple Safari 15+, Mozilla Firefox 100+, Microsoft Edge 100+.
