# 🕵️‍♂️ Prompt Engineering Mastery: Final Project Submission
**Track:** Track C - Generative AI Agents

## Submission Links
* **Colab Notebook:** [Google Colab Link](https://colab.research.google.com/drive/1Gu74lhTy_3evCsoH5vhFT5ST__gEzRpQ)
* **Video Presentation:** [Insert your video link here]

---

## 📖 Project Description
The ACE ACADEMY AI Agent is an autonomous, multimodal fact-checking agent designed to combat misinformation. It allows users to submit raw text claims, YouTube video links (including Shorts), or upload screenshots. The agent processes these inputs, extracts the core factual claim, autonomously navigates the live web to find reliable evidence, and synthesizes a strict 6-sentence evaluation stating whether the claim is "FACTUAL" or a "MAKEOVER" (false/misleading).

## ⚠️ Problem Statement
In the era of rapid information spread, misinformation is rampant across text, video, and image mediums. Standard Large Language Models (LLMs) struggle to fact-check in real-time due to knowledge cutoffs, and they often hallucinate when asked to evaluate unverified claims. Furthermore, verifying claims from multimedia (like YouTube videos or screenshots) requires manual transcription or visual analysis by the user before they can even begin searching for evidence. 

## 💡 Proposed Solution
Our solution is a fully autonomous, multimodal AI Agent. By combining Google's Gemini Vision and language models with external API tools (Tavily Web Search and YouTube Transcripts), the agent bridges the gap between multimedia content and live internet grounding. It intercepts the user's input, extracts the verifiable claim using a custom system prompt, fetches live evidence from trusted domains (excluding unreliable social media), and evaluates the truthfulness of the claim without hallucination.

---

## 🤖 Track C Specifics: Generative AI Agents

### Agent Architecture
The agent follows a multi-stage architecture designed to isolate reasoning steps and prevent prompt injection:
1. **Input Interface:** A Gradio UI accepting multimodal inputs (Text, Images, URLs).
2. **Extraction Engine (LLM):** Ingests the raw input (or processed transcripts/images) and extracts *only* the core verifiable claim.
3. **Tool Dispatcher:** Triggers the appropriate Python function based on the input type and reasoning state.
4. **Information Retrieval (RAG):** Queries the live web using the extracted claim to gather context.
5. **Evaluation Engine (LLM):** A heavily constrained LLM that takes the isolated claim and the retrieved evidence to output a final, rigid verdict.

### Tool Use
The agent has access to three primary tools to interact with its environment:
1. **`parse_youtube_link(user_input)`:** Detects YouTube URLs, retrieves the video's transcript (natively translating foreign languages like Hindi to English), and cleans the text for LLM consumption.
2. **`search_trusted_web(claim)`:** Utilizes the Tavily API to search the live web for the extracted claim. It actively blacklists user-generated domains (`youtube.com`, `reddit.com`, `tiktok.com`, `x.com`) to force the agent to rely solely on official or journalistic sources.
3. **`upload_and_wait(file_path)`:** Interfaces with the Gemini File API to upload image media, allowing the agent to "see" screenshots to extract claims.

### Multi-step Reasoning
The agent mimics a ReAct (Reasoning and Acting) workflow:
* **Step 1 (Reasoning):** "What is the user trying to claim in this video/image/text?" (Extracts claim).
* **Step 2 (Action):** "I need to verify this claim against the current state of the world." (Triggers `search_trusted_web`).
* **Step 3 (Reasoning):** "Does the evidence support the claim?" (Compares evidence to claim).
* **Step 4 (Action):** "Format the findings according to the strict system rules." (Outputs exactly 6 sentences starting with FACTUAL or MAKEOVER).

---

## 🧰 Tools & Libraries Used
* **`google-genai`**: For accessing Gemini 2.5 Flash and Flash-Lite models for reasoning and vision.
* **`tavily-python`**: For real-time, AI-optimized web search functionality.
* **`youtube-transcript-api`**: For fetching and translating video captions.
* **`gradio`**: For building the multimodal user interface.
* **`python-dotenv`**: For secure API key management.

## 🧠 Prompt Engineering Techniques Used
1. **System Prompting & Role-Playing:** Instructing the model to act as an "elite, impartial Fact-Checking Agent."
2. **Negative Prompting:** Explicitly telling the model what *not* to do (e.g., "IGNORE intros or promo details").
3. **Strict Constraints & Output Formatting:** Mandating exact sentence counts ("MUST output EXACTLY in 6 sentences") and explicit starting tokens ("The first sentence MUST be a single word: Either 'FACTUAL.' or 'MAKEOVER.'").
4. **Domain Restriction:** Limiting the scope of extraction to "verifiable factual claims (e.g., tech rumors, news, health)" to prevent the agent from processing opinions.
5. **Prompt Injection Guardrails:** Implementing a validation step that catches instructions like "assigning a new role, asking to code" and forces the model to return "NO_CLAIM", safely aborting the pipeline.

---

## 🚀 How to Run the Project Locally

Follow these exact steps to run this project on your local machine:

**1. Install Dependencies**
Open your terminal/command prompt and run:
```bash
pip install -r requirements.txt
```

**2. Set up API Keys**
Rename the `.env.example` file to exactly `.env`.
Open the `.env` file and insert your private API keys:
```env
Google_key=your_gemini_api_key_here
Tavily_Apikey=your_tavily_api_key_here
```

**3. Run the App**
Start the application by running the Python script:
```bash
python ace_academy_project.py
```
After a few seconds, it will give you a local URL (usually `http://127.0.0.1:7860`). Open that link in your web browser to start fact-checking!
