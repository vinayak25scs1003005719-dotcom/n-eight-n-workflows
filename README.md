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

### 3. Vinayak Personal Agent — Daily Email Digest

**File:** `Vinayak_Personal_Agent.json`

A fully automated personal assistant that runs every 24 hours. It fetches all unread Gmail messages, summarizes them using the NVIDIA-hosted Mistral Large model, and delivers a clean morning digest to a Discord channel — grouped by category (Client, Urgent, Promotional).

**Flow:**

```
Daily Trigger (every 24h) → Fetch Unread Gmail → Format Emails → Build AI Prompt → NVIDIA / Mistral LLM → Parse Response → Chunk Message → Send to Discord
```

**Credentials needed:**
- Gmail OAuth2
- NVIDIA API key (via HTTP Header Auth) — get one at [build.nvidia.com](https://build.nvidia.com)
- Discord Webhook URL — create one under your Discord server's channel settings

**Configuration:**
- The schedule trigger runs every 24 hours — adjust the interval or time in the `Daily 9AM Trigger` node to match your timezone
- The AI prompt instructs the model to group emails into: **Client emails**, **Urgent/Important**, and **Promotional/Spam**
- Long summaries are automatically chunked into multiple Discord messages (max 1900 chars each)
- Model used: `mistralai/mistral-large-3-675b-instruct-2512` via `https://integrate.api.nvidia.com/v1/chat/completions`

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
- The Personal Agent workflow splits long AI responses into multiple Discord messages to stay within Discord's 2000-character message limit.
