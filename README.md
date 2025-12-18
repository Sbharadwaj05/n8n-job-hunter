# 🎯 AI-Powered Job Hunter

> **Stop Doomscrolling. Start Automating.**
> 
> A fully automated n8n workflow that aggregates job feeds, filters out junk, and uses Google Gemini AI to score and analyze roles based on *your* specific criteria.

![n8n](https://img.shields.io/badge/automation-n8n-orange) ![AI](https://img.shields.io/badge/AI-Google%20Gemini-magenta) ![License](https://img.shields.io/badge/license-MIT-blue.svg)

---

## 🧐 What is this?

**TL;DR:** An automated n8n workflow that finds cybersecurity jobs in Bangalore/India, filters them with AI, scores each role based on your resume, and dumps results into Google Sheets—while you sleep.

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
* **Customizable:** Change search queries, filter keywords, and scoring criteria in minutes.

---

## 🛠️ Architecture

**Workflow Visual:**
```
RSS Sources → Merge → Deduplication (JS) → Loop → Regex Filter → AI Analysis → Google Sheets
```

### Tech Stack
* **Workflow Engine:** [n8n](https://n8n.io/) (Self-hosted or Cloud)
* **AI Model:** Google Gemini 1.5 Flash (Fast, accurate, and cost-effective)
* **Scripting:** JavaScript (Node.js)
* **Database:** Google Sheets

---

## 📥 Installation & Setup

### Prerequisites
- n8n installed (local or cloud): [Installation Guide](https://docs.n8n.io/hosting/)
- Google account (for Sheets and Gemini API)
- 15 minutes of setup time

---

### 1. Clone the Repo
```bash
git clone https://github.com/YOUR_USERNAME/ai-job-hunter.git
cd ai-job-hunter
```

### 2. Import Workflow into n8n
1. Open your n8n dashboard (`http://localhost:5678` or cloud instance).
2. Click **"Add Workflow"** (top right) → **"Import from File"**.
3. Select the `workflow.json` file from this repository.

### 3. Configure API Credentials

You'll see nodes with "Warning" icons. Click each to add credentials:

#### A. Google Gemini API Key
1. Go to [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Click **"Get API Key"** → **"Create API Key"**
3. Copy the key
4. In n8n, click the **"Message a model"** node → **Credentials** → **Add New**
5. Paste your API key and save

#### B. Google Sheets OAuth
1. Click the **"Append or update row in sheet"** node
2. Click **Credentials** → **Add New** → **OAuth2**
3. Follow the Google OAuth flow to connect your account

---

### 4. Create Your Google Sheet

**CRITICAL:** You must create a Google Sheet with these **exact** column headers in Row 1:

| A | B | C | D | E |
|---|---|---|---|---|
| **Job Title** | **Score** | **Salary** | **Analysis** | **Link** |

**Steps:**
1. Go to [Google Sheets](https://sheets.google.com) → Create a new blank sheet
2. Add the 5 headers above in Row 1 (case-sensitive)
3. Copy the sheet URL from your browser
4. In n8n, open the **"Append or update row in sheet"** node
5. Paste your sheet URL in the **"Document"** field
6. Select **"Sheet1"** (or whatever you named it) in the **"Sheet"** field

---

### 5. Customize Your Resume & Criteria

**This is the most important step!** The AI uses your resume to score jobs.

1. Open the **"Edit Fields"** node in n8n
2. Find the `resume` field
3. Replace the placeholder with your actual background:

```text
Name: [Your Name]
Role: [Your Target Role] | [Key Skills]
Summary: [Brief 2-3 sentence description]
Experience: [Relevant work history and projects]
Skills: [Tools, technologies, certifications]
```

**Example:**
```text
Name: Jane Doe
Role: Junior Full Stack Developer | React & Node.js
Summary: Computer Science graduate with 2 personal projects deployed to production. Experienced with React, Express, and PostgreSQL.
Experience: Built an e-commerce site with Stripe payments, contributed to open-source React library.
Skills: React, Node.js, Express, PostgreSQL, Git, Docker, AWS (basic)
```

---

### 6. Customize the AI Scoring Prompt (Optional)

The AI uses a scoring system to rank jobs. You can customize this!

1. Open the **"Message a model"** node
2. Find the prompt that starts with: `"You are a Job Filter..."`
3. Modify the scoring rules:

**Example changes:**
- **For Web Dev:** Change `"100 for Cyber Security Intern"` to `"100 for React Developer in Bangalore"`
- **For Remote Jobs:** Add `"Remote (Worldwide)"` to acceptable locations
- **For Salary Requirements:** Add `"+20 if salary > $70k"`

---

### 7. Customize Job Search Feeds (Optional)

By default, the workflow searches for **Cybersecurity jobs in Bangalore/India**. Change this to match your needs:

1. Click the **"RSS Read1"**, **"RSS Read2"**, and **"RSS Read3"** nodes
2. Replace the URLs with your own searches:

**Examples:**

**For Python Developer in Remote:**
```
https://news.google.com/rss/search?q=Python+Developer+Remote+(Junior+OR+Entry+Level)
```

**For Marketing Jobs in New York:**
```
https://news.google.com/rss/search?q=Marketing+Manager+New+York&hl=en-US&gl=US&ceid=US:en
```

**For RemoteOK (Web Dev):**
```
https://remoteok.com/remote-dev-jobs.rss
```

**For WeWorkRemotely (Design):**
```
https://weworkremotely.com/categories/remote-design-jobs.rss
```

---

### 8. Customize the Regex Filter (Optional)

The **"If"** node uses regex to block irrelevant jobs *before* they hit the AI (saving you money).

**Current filter (Cybersecurity):**
```regex
/(Cyber|SOC|Blue\s?Team|Red\s?Team|Pentest|Penetration|Vulnerability|Threat|Security|InfoSec|SIEM|Forensic|Incident|Malware|AppSec|DevSecOps|Wazuh|Splunk)/i
```

**Change to (Web Development):**
```regex
/(React|Frontend|TypeScript|Node\.js|Full Stack|JavaScript|Vue|Angular|Web Developer)/i
```

**Change to (Data Science):**
```regex
/(Data Scientist|Machine Learning|ML Engineer|AI|Python|TensorFlow|PyTorch|Data Analyst|SQL)/i
```

---

## 🚦 Quick Start Checklist

Before running your first job hunt, verify:

- [ ] Workflow imported into n8n
- [ ] Google Gemini API key added
- [ ] Google Sheets account connected via OAuth
- [ ] New Google Sheet created with **exact headers** (Job Title, Score, Salary, Analysis, Link)
- [ ] Sheet URL pasted into the "Append or update row" node
- [ ] Resume customized in "Edit Fields" node
- [ ] AI scoring prompt reviewed (optional)
- [ ] RSS feed URLs match your job search (optional)
- [ ] Regex filter updated for your target roles (optional)

---

## ▶️ Running the Workflow

### First Test Run
1. Click the **"When clicking 'Execute workflow'"** node
2. Click the **"Test workflow"** button (top right)
3. Wait 30-60 seconds
4. Check your Google Sheet—you should see scored jobs!

### Scheduling (Auto-Run Every Day)
1. Replace the **"Manual Trigger"** node with a **"Schedule Trigger"** node
2. Set it to run every 24 hours (e.g., 9 AM daily)
3. Activate the workflow with the toggle switch (top right)

---

## ⚠️ Troubleshooting

### ❌ "AI Node shows Red X error"
**Problem:** You're hitting rate limits.

**Solution:**
1. Open the **"Loop Over Items"** node
2. Set **Batch Size** to `1`
3. Open the **"Wait"** node
4. Increase wait time to `3 seconds`

---

### ❌ "No jobs appearing in Google Sheet"
**Problem:** Your Regex filter is too strict.

**Solution:**
1. Click the **"If"** node
2. Check the output arrow:
   - **Green arrow** = jobs passed the filter ✅
   - **Grey arrow** = filter blocked everything ❌
3. If grey, make the regex more lenient (add more keywords with `|`)

---

### ❌ "Error: Column 'Job Title' not found"
**Problem:** Your Google Sheet headers don't match exactly.

**Solution:**
1. Open your Google Sheet
2. Verify Row 1 has **exactly**: `Job Title`, `Score`, `Salary`, `Analysis`, `Link`
3. Check for typos, extra spaces, or capitalization errors

---

### ❌ "Workflow runs but all scores are 0"
**Problem:** The AI prompt doesn't match your job search.

**Solution:**
1. Open the **"Message a model"** node
2. Update the scoring criteria to match your target roles
3. Make sure "100 for [your ideal job title]" is in the prompt

---

## 🎨 Advanced Customization

### Add More RSS Feeds
1. Drag a new **"RSS Read"** node onto the canvas
2. Connect it to the **"Merge"** node (increase inputs to 4, 5, etc.)
3. Add your RSS URL

### Add Slack Notifications
1. Add a **"Slack"** node after the **"Code in JavaScript"** node
2. Configure it to send you a message for jobs scoring > 80

### Filter by Salary Range
1. Open the **"Code in JavaScript"** node
2. Add a filter: `if (parsedData.score > 70 && parsedData.salary.includes('$80k'))` 
3. Only high-scoring, high-paying jobs will be saved

---

## 🤝 Contributing

PRs are welcome! Here are some ideas:

- [ ] Add LinkedIn scraping support
- [ ] Add Telegram notifications
- [ ] Add Indeed RSS integration
- [ ] Add "Apply" button that auto-fills job applications
- [ ] Add sentiment analysis on company reviews

To contribute:
1. Fork this repo
2. Create a feature branch: `git checkout -b feature/linkedin-scraper`
3. Commit changes: `git commit -m "Add LinkedIn support"`
4. Push: `git push origin feature/linkedin-scraper`
5. Open a Pull Request

---

## 📄 License

MIT License - feel free to fork and modify!

---

## 🙏 Acknowledgments

- Built with [n8n](https://n8n.io/) (open-source workflow automation)
- Powered by [Google Gemini](https://deepmind.google/technologies/gemini/) (AI analysis)
- Inspired by every job seeker tired of clicking "Next Page" 1000 times

---

## 💡 Tips for Best Results

1. **Run it daily:** Set a schedule trigger for 9 AM every morning
2. **Tune your regex:** Start broad, then narrow down as you see results
3. **Adjust AI scoring:** If everything scores 0, your prompt is too strict
4. **Use multiple RSS feeds:** More sources = more opportunities
5. **Check your sheet regularly:** The best jobs go fast!

---

## 📧 Support

If you run into issues:
1. Check the [Troubleshooting](#-troubleshooting) section
2. Open a [GitHub Issue](https://github.com/YOUR_USERNAME/ai-job-hunter/issues)
3. Join the [n8n Community](https://community.n8n.io/)

---

**Happy Job Hunting! 🎉**

*Remember: The best job hunters don't work harder—they automate smarter.*
