# Challenging Problem
This document explains about the challenging technical problem I encountered while building CerebroMail and how I solved it.

**The Challenge**: The most challenging problem I faced was building an agentic AI system that could maintain contextual awareness across multiple email interactions while performing tasks like categorization, action extraction, draft generation, and conversational chat—all without losing the thread of what each email was about or mixing context between different emails.

**The Complexity:** Usually, LLM integrations are effective for single-shot queries. However, the problem is that an agent can comprehend an email thread only when the full context is provided. It needs to know what has been said before, the tone of the user, and at the same time, it should be able to pull out structured action items from the text. This cannot be done by LLM alone, prompt engineering and state management are necessary. Furthermore, prompt engineering and state management have to happen across several API calls.

**Context Preservation Problem:** While processing emails, the AI had to keep the context not only of the email but it also had to take into account the user's custom prompts from the Agent Brain, conversation history from the chat feature, and any previous actions that were taken on that email. At the same time, different emails had to be prevented from contaminating each other's context.

**The Solution:** I designed a modular architecture where each email is an isolated context bubble kept in SQLite, with the help of which prompt templates can be structured to inject the relevant context (email content, user preferences, conversation history) into each Gemini API call. Thus, the AI always has the complete picture and there is no mixing of contexts.

**Technical Implementation:** I developed different service layers (GeminiService, ChatService) whose main work includes the elaboration of rich, structured prompts by the merging of basic email data with user-configured prompts from the Agent Brain, previous chat messages, and task-specific instructions. The Google Gemini's generation config was then used to enforce JSON schema responses for easy parsing.

**State Management:** The system stores the metadata of processed emails in the database and the session storage is utilized to avoid repetitive AI processing. Moreover, the chat feature records the conversation history for each email and thus, the agent is capable of remembering the previous questions asked and providing them with logical, context-aware answers not only within one but several interactions. 

**Future Enhancement:** I have a plan to install RAG (Retrieval-Augmented Generation) as the next step that will allow the agent to search through the entire inbox; implement multi-turn conversation memory with semantic search; and add structured output schemas for all AI responses so as to do away with parsing errors and enable more complicated agentic workflows such as automated email threading and intelligent priority ​‍​‌‍​‍‌​‍​‌‍​‍‌scoring.

CerebroMail RepoLink: [https://github.com/lalitaditya04/CerebroMail](https://github.com/lalitaditya04/CerebroMail)
