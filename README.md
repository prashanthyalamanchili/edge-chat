Cloudflare Workers AI Chat App

A simple chat app running on Cloudflare Workers AI with KV-backed memory and two ways to clear it:

Project Overview
 
A minimal chat application running on Cloudflare Workers AI. It serves a simple web UI at /, exposes a chat API at /api/chat powered by a Workers AI model, and persists conversation memory in Cloudflare KV so the bot remembers context across turns. It also includes a memory reset feature you can trigger either by calling /api/reset (POST) or by sending /reset as the last user message to the chat API.

What we built / done

We wired the UI to a Cloudflare Worker that calls the model @cf/meta/llama-3.3-70b-instruct-fp8-fast, added a KV binding (CHAT_HISTORY) to store conversation history, and implemented two reset paths: a POST to /api/reset and an inline /reset command in the chat flow. We also configured an ASSETS binding to serve the static files (public/index.html and public/chat.js) for the web UI.

How to run locally

1.	Open Command Prompt / Terminal.
2.	Go to your user directory (Windows: cd %USERPROFILE%, macOS/Linux: cd ~).
3.	Clone the repository:
git clone https://github.com/prashanthyalamanchili/edge-chat.git
4.	Enter the folder:
cd edge-chat
5.	Install dependencies:
npm install
6.	Sign in to Cloudflare (one-time):
npx wrangler login
7.	Start the dev server:
npx wrangler dev --remote
What to open after it starts
• Wrangler prints a local preview URL in the terminal;you can also press “b” in that terminal to open the browser.
• Open the printed URL in your browser to see the chat UI .
