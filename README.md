## Chatbot with Sentiment Analysis

### 1. Overview:
   - LLM based chatbot (Grok 4.1 Fast via OpenRouter)
   - Transformer based sentiment analysis using DistilBERT SST-2
   - Conversation level sentiment analysis (Tier 1)
   - Per-message sentiment labeling (Tier 2)
   - LLM based conversation summarization
   - Backup offline chatbot (rule-based)
   - Streamlit UI

### 2. How to setup and use

   A. Install all required packages using: `pip install -r requirements.txt`

   B. LLM (OpenRouter) API Key
   
   C. Run the chatbot
   - `streamlit run frontend.py`


### Sentiment Analysis Logic

   - Transformer-based sentiment classifier to evaluate user expressions.
   - DistilBERT SS-2, implemented from HuggingFace Transformers library for binary sentiment classification.
   - Messages are tokenized through the tokenizer and passed through model to obtain class logits.
   - The highest-scoring label is returned, which are then used to compute the overall emotional scale of the conversation.
   - Conversation level sentiment can be derived from aggregating message-level outputs. Thus, I implemented message level logic first, followed by the conversation level logic.

   <ins>Tier 1</ins>
   - After the conversation ends, all stored user message sentiments are collected.
   - These labels are produced by the DistilBERT sentiment analyzer.
   - The system counts how many user messages were POSITIVE vs NEGATIVE.
      - More POSITIVE -> Overall Sentiment: POSITIVE
      - More NEGATIVE -> Overall Sentiment: NEGATIVE
      - Equal → Overall Sentiment: NEUTRAL
    
   <ins>Tier 2</ins>
   - Messages are passed through the DistilBERT sentiment analyzer.
   - The model outputs either a POSITIVE or NEGATIVE label.
   - Sentiment label is displayed next to each user message.
   - These labels are stored and used to obtain conversation sentiment logic as explained in Tier 1.

### 5. Backup Chatbot Logic
   - In case connection to API cannot be made, or there are issues with the API key, a backup chat logic is in place.
   - This is a fully offline, rule-based backup chatbot.
   - Error handling is done in case of LLM failure automatically.
   - Aside from the actual chat logic, an offline backup summarizer is also present.
