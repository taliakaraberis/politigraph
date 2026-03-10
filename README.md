# Politigraph - Politician Transparency Dashboard
A visual transparency dashboard for tracking politician money flows, voting records, career timelines, and age demographics. The data may be sad, but that doesn't mean we can't have fun with the visuals!

This dashboard ships with illustrative data. Here's how to wire in real data:

### 1. Campaign Finance — FEC.gov (Free, No Key Required)

```javascript
// Search for a politician's committee
const response = await fetch(
  `https://api.open.fec.gov/v1/candidates/search/?q=${encodeURIComponent(name)}&api_key=DEMO_KEY&per_page=5`
);
const { results } = await response.json();
const candidateId = results[0].candidate_id;

// Get their receipts (money IN)
const receipts = await fetch(
  `https://api.open.fec.gov/v1/schedules/schedule_a/?candidate_id=${candidateId}&api_key=DEMO_KEY&per_page=20&sort=-contribution_receipt_amount`
);

// Get their disbursements (money OUT)
const disbursements = await fetch(
  `https://api.open.fec.gov/v1/schedules/schedule_b/?candidate_id=${candidateId}&api_key=DEMO_KEY&per_page=20`
);
```

Get a free API key at: https://api.open.fec.gov/developers/

### 2. Voting Record + Career — ProPublica Congress API (Free)

```javascript
const PROPUBLICA_KEY = 'your_key_here'; // free at propublica.org/datastore/api

// Search for member
const member = await fetch(
  `https://api.propublica.org/congress/v1/members/search.json?query=${name}`,
  { headers: { 'X-API-Key': PROPUBLICA_KEY } }
);

// Get voting record
const votes = await fetch(
  `https://api.propublica.org/congress/v1/members/${memberId}/votes.json`,
  { headers: { 'X-API-Key': PROPUBLICA_KEY } }
);

// Get career bio
const bio = await fetch(
  `https://api.propublica.org/congress/v1/members/${memberId}.json`,
  { headers: { 'X-API-Key': PROPUBLICA_KEY } }
);
```

Get a free key at: https://www.propublica.org/datastore/api/propublica-congress-api

### 3. Governors & State Data

- **Ballotpedia API** (paid, but has free tier) — state-level politicians
- **OpenStates API** — state legislature data: https://openstates.org/api/
- **Wikipedia API** — governor bios and career data (free, no key)

---
## API Key info
This app uses demo keys. You can click the "cog" (Settings) in the upper right hand corner (next to Analyze button) to store your own keys locally. If you want your own keys pre-baked, you can fork this repo, open that file, find const API_KEYS = { near the top of the script, and replace 'DEMO_KEY' with your actual API keys.


## 🗺 Roadmap / Planned Visualizations

- [x] Money In/Out (donut + sankey)
- [x] Voting record lean chart
- [x] Career timeline
- [x] Age demographics histogram
- [ ] Network graph (donor connections between politicians)
- [ ] Stock trading activity (via SEC EDGAR)
- [ ] Geographic donor map
- [ ] Bill sponsorship network
- [ ] Lobbying connections
- [ ] Social media sentiment over time

---

## 🛠 Tech Stack

- **D3.js v7** — all visualizations
- **Vanilla JS** — no framework needed
- **IBM Plex fonts** — via Google Fonts
- **GitHub Pages** — free hosting

---

## 📋 Data Sources

| Data Type | Source | Cost | Coverage |
|-----------|--------|------|----------|
| Federal campaign finance | FEC.gov | Free | Federal candidates |
| Voting records | ProPublica Congress API | Free (key req'd) | U.S. Congress |
| Career/bio | ProPublica / Bioguide | Free | U.S. Congress |
| State legislators | OpenStates | Free tier | All 50 states |
| Stock trades | SEC EDGAR / Quiverquant | Free/Paid | Congress members |
| Lobbying | OpenSecrets | Free tier | Federal |

---

## ⚠️ Disclaimer

This tool displays publicly available government data for educational and transparency purposes. Always verify data against primary sources (FEC.gov, Congress.gov).
