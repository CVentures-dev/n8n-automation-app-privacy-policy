# WhatsApp Pitch-Deck Assistant — User Guide (July 2025)

This bot lets you drop a DocSend link **or** attach a PDF pitch-deck in WhatsApp, instantly turns it into a PDF, and—when you tell it—files the deck to the right company in Affinity.

---

## 1. How the system is wired (just for context)

| Workflow | What it does | When it runs | Key output |
|-----------|--------------|--------------|------------|
| **WhatsApp 2.0** | Listens to your messages, replies, and orchestrates everything else. | Every new WhatsApp message from the four whitelisted numbers. | Chat replies + calls the helper workflows |
| **docsend2pdf** | Converts a DocSend link to a PDF and immediately sends that PDF back into the chat. | Whenever the bot sees a DocSend link | `mediaId` + the PDF file |
| **process_pdf** | Runs OCR on the PDF, turns slides into Markdown, and extracts **company name** & **website**. | Any time the bot needs metadata about a deck | `{ "company": “…", "domain": "…" }` |
| **save_pdf** | Uploads the PDF to the correct *Files* tab in Affinity. | Only after you say **upload / save / add to Affinity** | Affinity confirmation |

---

## 2. What *you* can do in WhatsApp

> The bot answers in under five seconds with **“Message received. Processing…”** so you know it heard you.

| Your goal | What to send | What happens next |
|-----------|--------------|-------------------|
| **Preview a deck** (no upload yet) | DocSend link (alone) **OR** attach a PDF | Bot sends back the PDF named like `ACME_deck.pdf`. The deck is kept “pending” so you can upload later. |
| **Upload after preview** | `upload`  *(or `save`, `add to Affinity`)* | Bot suggests matching companies from Affinity. Reply with the number or “0” to create a new one. |
| **Send & upload in one go** | DocSend link **or** PDF **plus** the word `upload` (or `save` / `add to Affinity`) in the same message | Same flow as above, but it starts the upload immediately. |
| **Get help** | `help` | Quick cheat-sheet of the above commands. |

*Upload keywords are **case-insensitive**; any of the three will work.*

---

## 3. Conversation examples  

### A. Quick look, then upload
You: https://docsend.com/view/xyz123

Bot: (sends PDF) ACME_deck.pdf


You: upload

Bot: I found these orgs — 1) Acme Inc 2) Acme Corp 0) None of these – create new


You: 1

Bot: Deck uploaded to Acme Inc. ✅

### B. One-shot upload
You: (attach “BetaDeck.pdf”, caption: “save to affinity”)


Bot: Deck uploaded to Beta Technologies. ✅

---

## 4. What to expect  

| Situation | Bot says… |
|-----------|-----------|
| DocSend needs an email | “DocSend requires an email—please reply with one.” |
| Several Affinity matches | Lists them and asks you to pick a number. |
| You typed `upload` but no pending deck | “I don’t see any deck to upload. Please send one first.” |
| Unsupported file type | “Unsupported format. Please send text or a PDF.” |

---

## 5. Good-to-know limits & tips  

* **One deck at a time** – the bot remembers only the latest un-uploaded deck per chat.  
* **Stay whitelisted** – for now only four numbers are allowed; ping Ops to add more.  
* **Include your email with DocSend links** when possible; it saves one round-trip.  
* **Max file size ≈ 50 MB** – very large decks may fail.  
* **Data stays private** – files live only in DocSend, WhatsApp, Mistral (for OCR), and Affinity. No extra copies are stored.  

---

## 6. Quick start (TL;DR)

1. **Drop a DocSend link** → PDF appears in chat.  
2. **Type “upload”** → pick the company, done.  
