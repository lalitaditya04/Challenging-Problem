# Challenging​‍​‌‍​‍‌​‍​‌‍​‍‌ Problem
This document describes the most technically challenging problem I had when developing CerebroMail and the way I resolved it.

**The Challenge:** The most difficult thing for me was to come up with an agentic AI system that can keep the contextual awareness of the interactions over the multiple emails and perform the tasks like categorization, action extraction, draft generation, and conversational chat, etc., without forgetting the content of each email or mixing the context of different emails.

**The Complexity:** In general, LLM (large language model) integrations are suitable for single-shot queries. However, to make an agent that can understand the full context of an email thread, keep track of the interactions, create drafts based on the user's preferred tone, and at the same time extract structured action items is beyond the capability of LLM alone. It needs prompt engineering and management of the state across multiple API calls.

**Context Preservation Problem:** While reading the emails, the AI had to be aware of the content of the email, the user's custom prompts from the Agent Brain, the conversation history from the chat feature, and any actions that were taken on that email - at the same time, it had to ensure that different emails did not contaminate the context of each other.

**The Solution:** I used a modular design in which every email has its isolated context bubble saved in SQLite with structured prompt templates that insert relevant context (email content, user preferences, conversation history) into each Gemini API call, so the AI always has a complete picture without any context leakage.

**Technical Implementation:** I created distinct service layers (GeminiService, ChatService) that produce rich, structured prompts by mixing the basic email data with user-configured prompts from the Agent Brain, previous chat messages, and task-specific instructions. Then they employed Google Gemini's generation config to impose JSON schema responses for uniform parsing.

**State Management:** The apparatus keeps track of the metadata of the processed emails in the database and employs session storage for AI processing that is not done twice. At the same time, the chat feature stores conversation history per email, thus the agent is able to recall previous questions and deliver coherent, context-aware answers across various interactions.

**Future Enhancement:** In the future, I am going to use RAG (Retrieval-Augmented Generation) so that the agent can look at the whole inbox and find the emails that are relevant to the query. I will also enable multi-turn conversation memory using semantic search and add structured output schemas for all AI responses so as to remove parsing errors and allow more complex agentic workflows such as email threading automation and intelligent priority ​‍​‌‍​‍‌​‍​‌‍​‍‌scoring.

THe Agentic Mail is called CerebroMail repoLink: [https://github.com/lalitaditya04/CerebroMail](https://github.com/lalitaditya04/CerebroMail)
