# 🎯 AI-Powered Job Hunter

> **Stop Doomscrolling. Start Automating.**
> A fully automated pipeline that aggregates job feeds, filters out junk, and uses Google Gemini AI to score and analyze roles based on *your* specific criteria.

![n8n](https://img.shields.io/badge/automation-n8n-orange) ![AI](https://img.shields.io/badge/AI-Google%20Gemini-magenta) ![License](https://img.shields.io/badge/license-MIT-blue.svg)

---

## 🧐 What is this?

Finding a niche role (e.g., *Cybersecurity in Bangalore* or *React Dev in London*) on generic job boards is painful. You waste hours filtering through "Senior" roles when you want "Junior", or "Sales" roles disguised as "Tech".

This tool is your personal 24/7 recruiter. It:
1. **Aggregates** jobs from any RSS feed (Google News, RemoteOK, WeWorkRemotely).
2. **Deduplicates** listings across multiple sources so you don't see the same job twice.
3. **Filters** applications using a strict **Regex Gatekeeper** (saving you AI credits by blocking irrelevant titles).
4. **Analyzes** the survivors with an LLM (Google Gemini) to:
   * Extract **Salary** (even if hidden in text like "$60k-$90k").
   * **Score** the role (0-100) based on your custom playbook.
   * Verify **Location** eligibility.
5. **Delivers** a clean, scored list to your Google Sheet.

---

## 🚀 Features

* **Source Agnostic:** Works with any RSS feed (Google News, Specialized Boards).
* **"Sniper" Filtering:** Uses Regex to block irrelevant titles *before* they reach the AI.
* **AI-Powered Analysis:** Extracts "hidden" data like salary ranges and tech stack requirements.
* **Smart Scoring:** You define the rules (e.g., "+20 points for Python", "-50 points for Manager").
* **Rate Limit Safe:** Built-in throttling to prevent API crashes.

---

## 🛠️ Architecture

**Workflow Visual:**
```
RSS Sources → Merge (Append) → Deduplication (JS) → Loop → Regex Filter → AI Analysis → Google Sheets
```

### Tech Stack
* **Workflow Engine:** [n8n](https://n8n.io/) (Self-hosted or Cloud)
* **AI Model:** Google Gemini 1.5 Flash (Fast, accurate, and cost-effective)
* **Scripting:** JavaScript (Node.js)
* **Database:** Google Sheets

---

## 📥 Installation & Setup

### 1. Clone the Repo
```bash
git clone https://github.com/YOUR_USERNAME/ai-job-hunter.git
cd ai-job-hunter
```

### 2. Import into n8n
1. Open your n8n dashboard (`http://localhost:5678`).
2. Click **"Add Workflow"** (top right) > **"Import from File"**.
3. Select the `workflow.json` file from this repository.

### 3. Configure Credentials
You will see nodes with "Warning" signs. Double-click them to add your keys:
* **Google Gemini Node:** Get a free API key from [Google AI Studio](https://aistudio.google.com/).
* **Google Sheets Node:** Connect your standard Google account (OAuth2).

### 4. Setup the Google Sheet
Create a new Google Sheet. You **MUST** add these exact headers in Row 1, or the automation will fail:

| A | B | C | D | E | F |
|---|---|---|---|---|---|
| Job Title | Score | Location | Salary | Analysis | Link |

Copy the URL of this sheet and paste it into the "Sheet ID" field in the final n8n node.

---

## ⚙️ How to Customize (Make it Yours)

This workflow is configured for a **Cybersecurity** role by default, but you can change it to anything in 3 minutes.

### 1. Change the Source (RSS Nodes)
Go to the **RSS Read** nodes and paste your own search URLs.
* **For Python Devs:** `https://remoteok.com/remote-python-jobs.rss`
* **For Local Jobs:** Use Google News RSS: `https://news.google.com/rss/search?q=Marketing+Manager+New+York`

### 2. Change the Filter (If Node)
Update the Regex in the **If Node** to match your target keywords.
* **Current (Security):** `/(Cyber|SOC|Blue Team|Wazuh)/i`
* **Change to (Web Dev):** `/(React|Frontend|TypeScript|Node\.js|Full Stack)/i`

### 3. Change the Brain (AI Prompt)
Open the **Message a Model** node and edit the System Prompt.
* **Current Rule:** "+20 for LFS experience, -100 for Sales."
* **Change to:** "+20 for AWS certification, -100 for WordPress."

---

## ⚠️ Troubleshooting

**Q: The AI Node shows a "Red X" error.**
* **A:** You are hitting rate limits.
  * Open the **Loop Over Items** node and set **Batch Size** to `1`.
  * Ensure the **Wait** node (after the AI node) is set to `2 seconds`.

**Q: No jobs are appearing in the sheet.**
* **A:** Your Regex filter might be too strict. Check the **If Node** outputs. If the arrow coming out is grey, it means the filter blocked everything.

**Q: "Item Lists" node is missing?**
* **A:** This workflow uses a custom JavaScript **Code** node for deduplication instead, which is more reliable across different n8n versions.

---

## 🤝 Contributing

PRs are welcome! If you add support for LinkedIn scraping, Telegram notifications, or Slack alerts, please open a request.

---

## 📄 License

MIT License - feel free to fork and modify!

---

## 🙏 Acknowledgments

Built with [n8n](https://n8n.io/) and powered by [Google Gemini](https://deepmind.google/technologies/gemini/).

---

**Happy Job Hunting! 🎉**
