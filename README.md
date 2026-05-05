# n8n Workflows

A collection of personal n8n automation workflows.

---

## Workflows

### 1. Audio Transcription & Summary
**File:** `Audio_Transcription_Summary.json`

Fetches an audio file from a URL, transcribes it using the [Sarvam AI](https://sarvam.ai) speech-to-text API, summarizes the transcript using Google Gemini, and saves the summary to a Google Doc.

**Flow:**
```
Manual Trigger → Fetch Audio (HTTP) → Sarvam STT → Extract Transcript → Gemini Summary → Save to Google Doc
```

**Credentials needed:**
- Sarvam AI API key
- Google Gemini (PaLM) API
- Google Docs OAuth2

---

### 2. Lead Alert Workflow
**File:** `Lead_Alert_Workflow.json`

Monitors a Google Sheet for new rows (leads) every minute. When a new lead is detected, it sends a formatted HTML email alert via Gmail with the lead's contact info, qualification answers, and ad source details.

**Flow:**
```
Google Sheets Trigger (every minute) → Filter New Rows → Send Gmail Alert
```

**Credentials needed:**
- Google Sheets OAuth2
- Gmail OAuth2

---

## Setup

1. Import the `.json` file into your n8n instance via **Workflows → Import from file**
2. Set up the required credentials in n8n under **Settings → Credentials**
3. Replace any placeholder values (marked `YOUR_...`) with your actual IDs and keys
4. Activate the workflow

---

## Notes

- API keys and credential IDs have been stripped from these files before publishing. You will need to reconnect credentials after importing.
- The Lead Alert workflow uses `$getWorkflowStaticData` to track already-processed rows and avoid duplicate alerts.
