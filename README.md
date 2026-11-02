# 🎙️ Transcription Summarizer AI Agent (n8n Workflow)

An automated **n8n workflow** that transforms video and audio URLs into accurate transcriptions and structured summaries. Using **Groq's Whisper model** for lightning-fast audio processing and a **Google Gemini-powered AI Agent**, this workflow automatically logs both the full transcript and key takeaways directly into a new **Google Doc**.

---

## 🚀 Features

* **Chat-Triggered Execution:** Simply drop a direct video or audio link into the n8n chat interface to start the process.
* **Ultra-Fast Transcription:** Uses Groq's `whisper-large-v3` API to translate and transcribe media files with high accuracy and minimal latency.
* **Autonomous Google Docs Management:** The AI Agent automatically creates a new Google Doc, inserts the raw transcription, appends a formatted summary, and fetches the final document.
* **Structured AI Summaries:** Powered by Google Gemini, the agent extracts:
  * 🎯 Key themes and main points
  * 💡 Important details and insights
  * ✅ Action items and next steps
  * 📌 Conclusions and recommendations
  * 📊 Notable data or statistics

---

## 🛠️ Workflow Architecture

1. **When chat message received (`Chat Trigger`):** Listens for a user message containing a media URL (`chatInput`).
2. **Download video (`HTTP Request`):** Fetches the binary media file from the provided URL.
3. **Groq Audio Transcription (`HTTP Request`):** Sends the binary file to `https://api.groq.com/openai/v1/audio/translations` using the `whisper-large-v3` model.
4. **AI Agent (`LangChain Agent`):** Orchestrates the summarization and document creation process using:
   * **LLM:** Google Gemini (`gemini-3.5-flash-lite` / Gemini Flash)
   * **Tools:** Google Docs Tool (`Create document`, `Update document`, `get document`)

---

## 📋 Prerequisites

Before importing and running this workflow, ensure you have the following:

1. **n8n Instance:** Self-hosted or n8n Cloud.
2. **Groq API Key:** Get one from the [Groq Console](https://console.groq.com/) for Whisper audio transcription.
3. **Google Gemini API Key:** Available via [Google AI Studio](https://aistudio.google.com/).
4. **Google Cloud OAuth2 Credentials:** Configured with the **Google Docs API** enabled to allow n8n to create and edit documents in your Google Drive.

---

## ⚙️ Setup & Installation

1. **Import the Workflow:**
   * Open your n8n dashboard.
   * Click on **Add Workflow** > **Import from File** and select `Transcription Summarizer Agent.json`.

2. **Configure Groq API (HTTP Request Node):**
   * Open the **HTTP Request** node (positioned after the *Download video* node).
   * Under **Header Parameters**, locate the `Authorization` header.
   * Replace `<YOUR_GROQ_API>` with your actual Groq API key (keep the `bearer ` prefix, e.g., `bearer gsk_xxxx...`).

3. **Connect Google Gemini Credentials:**
   * Open the **Google Gemini Chat Model** node.
   * Select or create new credentials using your Google Gemini API key.
   * *(Optional)* Verify the `modelName` parameter matches your preferred Gemini model (e.g., `models/gemini-3.1-flash-lite` or `models/gemini-3.5-flash-lite`).

4. **Connect Google Docs Credentials:**
   * Open the **Create document**, **get document**, and **Update document** tool nodes.
   * Authenticate your Google Docs OAuth2 account on each node, and specify a `Folder ID` in the **Create document** node if you want the docs saved in a specific Google Drive folder.

5. **Activate & Test:**
   * Click **Chat** at the bottom of the n8n canvas.
   * Paste a direct link to an audio or video file and watch the agent return your Google Doc link alongside a bulleted summary!
