WhatsApp AI Agent with Conversation Memory
A Make.com automation that receives incoming WhatsApp messages, retrieves the sender's stored conversation history, generates a context-aware response with an LLM, saves the conversation, and sends the reply back through WhatsApp.
Overview
The scenario combines Make.com, a WhatsApp messaging API, Groq's OpenAI-compatible chat completions endpoint, and Supabase to provide a lightweight AI assistant with persistent conversation history.
Workflow
Receive message — a custom webhook receives an incoming WhatsApp message.
Find conversation — Supabase searches the `chat_sessions` table using the sender's cleaned phone number.
Route by history — a router separates first-time conversations from senders with an existing session.
Build the prompt — the scenario combines the system instructions, the incoming message, and (when available) prior chat history.
Generate a response — an HTTP request calls Groq's chat completions API using `openai/gpt-oss-120b`.
Persist the conversation — Supabase creates a new session or updates the existing session with the user and assistant messages.
Send the reply — an HTTP request sends the generated text to the sender through WaSenderAPI.
Stack
Automation: Make.com
Messaging: WaSenderAPI
LLM inference: Groq API (`openai/gpt-oss-120b`)
Conversation storage: Supabase
Integration style: Webhooks and HTTP API requests
Repository Contents
`whatsapp-ai-agent-blueprint-sanitized.json` — sanitized Make.com blueprint. API credentials are placeholders and must be configured in Make.com.
Setup Notes
Import the JSON blueprint into Make.com.
Reconfigure the incoming webhook and connect your own Supabase account.
Verify that the `chat_sessions` table exists and has the fields expected by the scenario:
`customer_phone`
`chat_history`
`last_updated`
Add your own Groq API key to the relevant HTTP Authorization headers.
Add your own WaSenderAPI token to the WhatsApp send-message HTTP modules.
Review the system prompt and update it for your use case.
Test both paths: a new sender with no stored session and a returning sender with existing history.
Confirm the scenario handles API failures and malformed/empty messages as required before production use.
> The exported blueprint contains placeholders instead of real API tokens. Reconnect services and configure credentials in Make.com after import. Imported scenarios may require connection, webhook, and module settings to be selected again.
Security
Never commit API keys, access tokens, webhook secrets, customer phone numbers, or private conversation logs.
Store credentials in Make.com connections or a secrets manager wherever supported.
If a credential has been exposed, revoke it and generate a replacement before continuing.
Use synthetic data in screenshots and demos.
Limitations
This repository documents the exported workflow; it does not include credentials, a live WhatsApp account, a Supabase project, or a hosted deployment. The scenario's actual behavior depends on the connected services and their current configuration.
License
Add a license only if you want to grant others explicit permission to reuse or redistribute this project.
