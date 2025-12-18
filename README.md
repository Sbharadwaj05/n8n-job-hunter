# 🎯 AI-Powered Job Hunter

> **Stop Doomscrolling. Start Automating.**
> A fully automated pipeline that aggregates job feeds, filters out junk, and uses Google Gemini AI to score and analyze roles based on *your* specific criteria.

![License](https://img.shields.io/badge/license-MIT-blue.svg) ![n8n](https://img.shields.io/badge/automation-n8n-orange) ![AI](https://img.shields.io/badge/AI-Google%20Gemini-magenta)

## 🧐 What is this?
Finding a niche role (e.g., *Cybersecurity in Bangalore* or *React Dev in London*) on generic job boards is painful. You waste hours filtering through "Senior" roles when you want "Junior", or "Sales" roles disguised as "Tech".

This tool is your personal 24/7 recruiter. It:
1.  **Aggregates** jobs from any RSS feed (Google News, RemoteOK, WeWorkRemotely).
2.  **Deduplicates** listings across multiple sources.
3.  **Filters** applications using a strict Regex Gatekeeper (saving you AI credits).
4.  **Analyzes** the survivors with an LLM (Google Gemini) to:
    * Extract Salary (even if hidden in text).
    * Score the role (0-100) based on your custom playbook.
    * Verify Location eligibility.
5.  **Delivers** a clean, scored list to your Google Sheet.

---

## 🚀 Features
* **Source Agnostic:** Works with any RSS feed.
* **"Sniper" Filtering:** Uses Regex to block irrelevant titles before they reach the AI.
* **AI-Powered Analysis:** Extracts "hidden" data like salary ranges and tech stack requirements.
* **Smart Scoring:** You define the rules (e.g., "+20 points for Python", "-50 points for Manager").
* **Rate Limit Safe:** Built-in throttling to prevent API crashes.

---

## 🛠️ Architecture
**Workflow Visual:**
`RSS Sources` → `Merge (Append)` → `Deduplication (JS)` → `Loop` → `Regex Filter` → `AI Analysis` → `Google Sheets`

### Tech Stack
* **Workflow Engine:** [n8n](https://n8n.io/)
* **AI Model:** Google Gemini 1.5 Flash (Fast & Cheap)
* **Scripting:** JavaScript (Node.js)
* **Database:** Google Sheets

---

## ⚙️ How to Customize (Make it Yours)
This workflow is configured for a **Cybersecurity** role by default, but you can change it to anything in 3 minutes.

### 1. Change the Source (RSS Nodes)
Go to the **RSS Read** nodes and paste your own search URLs.
* *For Python Devs:* `https://remoteok.com/remote-python-jobs.rss`
* *For Local Jobs:* Use Google News RSS: `https://news.google.com/rss/search?q=Marketing+Manager+New+York`

### 2. Change the Filter (If Node)
Update the Regex in the **If Node** to match your target keywords.
* *Current (Security):* `/(Cyber|SOC|Blue Team|Wazuh)/i`
* *Change to (Web Dev):* `/(React|Frontend|TypeScript|Node\.js|Full Stack)/i`

### 3. Change the Brain (AI Prompt)
Open the **Message a Model** node and edit the System Prompt.
* *Current Rule:* "+20 for LFS experience, -100 for Sales."
* *Change to:* "+20 for AWS certification, -100 for WordPress."

---

## 📥 Installation & Setup

### 1. Clone the Repo
```bash
git clone [https://github.com/YOUR_USERNAME/ai-job-hunter.git](https://github.com/YOUR_USERNAME/ai-job-hunter.git)
cd ai-job-hunter
