Cloudflare Workers AI Chat (KV Memory + Reset)

A simple chat app running on Cloudflare Workers AI with KV-backed memory and two ways to clear it:

POST /api/reset

Sending /reset as the last user message to /api/chat

Live worker (replace with yours if you fork):
https://edge-chat.prashanthyalamanchili1.workers.dev/

Prerequisites

Cloudflare account with a workers.dev subdomain

Access to Workers AI

A KV namespace created for chat history (e.g. chat_history_prod)

Required Bindings

In Workers & Pages → Your Worker → Settings → Bindings, add:

Type	Name	Value / Notes
Workers AI	AI	Select Workers AI Catalog
KV Namespace	CHAT_HISTORY	Select your KV namespace (e.g. chat_history_prod)
Assets	ASSETS	Static assets (bundled UI)

Compatibility:

Set a recent Compatibility date in Settings → Runtime.

If you used it during setup, enable nodejs_compat under Compatibility flags.

Deploy (via Dashboard)

Import this repo in Cloudflare: Workers & Pages → Create → Import a repository.

After the first deploy, go to Settings → Bindings and add the three bindings shown above.

Click Deploy version.

Deploy (via Wrangler) – optional
npm i -g wrangler
wrangler login

# Create your KV namespace
wrangler kv namespace create chat_history_prod

# Deploy
wrangler deploy


Ensure your wrangler.toml or dashboard bindings include: AI, CHAT_HISTORY, and ASSETS.

API Endpoints
POST /api/chat

Sends messages to the model.
If the last user message is "/reset", the Worker clears KV and replies with Memory reset ✅.

Example:

curl -X POST "https://edge-chat.prashanthyalamanchili1.workers.dev/api/chat" \
  -H "content-type: application/json" \
  -d '{
    "messages": [
      {"role":"user","content":"Say hello!"}
    ]
  }'


Reset via chat (send /reset as the last message):

curl -X POST "https://edge-chat.prashanthyalamanchili1.workers.dev/api/chat" \
  -H "content-type: application/json" \
  -d '{
    "messages": [
      {"role":"user","content":"/reset"}
    ]
  }'

POST /api/reset

Clears KV memory directly.

curl -X POST "https://edge-chat.prashanthyalamanchili1.workers.dev/api/reset"
# => {"success":true}

Configuration (optional)

Edit in src/index.ts if you want to customize:

const MODEL_ID = "@cf/meta/llama-3.3-70b-instruct-fp8-fast";
const SYSTEM_PROMPT = "You are a helpful, friendly assistant. Provide concise and accurate responses.";
const KV_KEY = "conversation"; // Single key used to store chat history

Quick Checks

Visiting / shows the chat UI.

POST /api/chat returns a model response.

POST /api/reset returns {"success": true}.

Sending /reset as the last user message to /api/chat clears memory.

Troubleshooting

405 Method not allowed on /api/reset: Use POST, not GET.

“Cannot read properties of undefined (reading 'fetch')” / asset errors: Ensure the ASSETS binding exists.

Model/AI errors: Ensure the AI binding is configured.

KV not clearing: Confirm CHAT_HISTORY is bound to your KV namespace and your compatibility date is recent.
