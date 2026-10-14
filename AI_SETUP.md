# ANRX AI setup

The frontend is a single-page AI workspace with a procedural animated 3D-style orb. The live chat calls the Vercel serverless endpoint at `/api/chat`. Provider credentials stay on the server and are never embedded in `index.html`.

## 1. Configure the AI provider

In Vercel, open the ANRX project → **Settings → Environment Variables**. Add:

- `AI_API_KEY`: your provider API key (secret; never add this to frontend code).
- `AI_BASE_URL`: the OpenAI-compatible API base URL, without the final `/chat/completions` path. For OpenRouter, use `https://openrouter.ai/api/v1`. For OpenAI, use `https://api.openai.com/v1`.
- `AI_MODEL`: the exact model identifier available to your provider. The default is `openrouter/free`; availability and limits are controlled by the provider.

You may use `OPENROUTER_API_KEY` or `OPENAI_API_KEY` instead of `AI_API_KEY`. If multiple keys are set, `AI_API_KEY` takes precedence.

## 2. Redeploy

After saving environment variables, redeploy the latest deployment. Then open the site and send a message. The status indicator will change when a real model response is received.

## 3. Cost and security

- The visual 3D orb and interface do not require a paid 3D service.
- AI access is provided by the selected model provider. Free models may have rate limits, queues, or availability changes; some models require paid credits.
- Do not put an API key in browser JavaScript or commit one to GitHub.
- This initial version does not include user accounts, saved chat history, or per-user rate limiting. Add these before opening the endpoint to broad public traffic.

## API contract

POST `/api/chat` with JSON `{ "messages": [{ "role": "user", "content": "..." }] }`. It returns `{ "reply": "...", "model": "..." }` when the provider succeeds.
