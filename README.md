
# 🧠 RAG Chat Frontend

A **lightweight, client-side RAG (Retrieval-Augmented Generation) chat interface** that works with **any RAG-compatible API** using just a **full API URL** and a **Bearer token**.  

✅ **No backend needed**  
✅ **Chat history saved locally in your browser**  
✅ **Zero dependencies — pure HTML/JS**  
✅ **Privacy-first — your data never leaves your machine**  
✅ **Supports CORS proxy for cross-origin APIs**

Perfect for testing, demoing, or embedding a RAG chat into your workflow — especially when your API expects a simple `{"query": "..."}` payload and returns structured answers.

---

## 🚀 Features

- 🔒 Secure: API token never leaves your browser
- 💾 Persistent local chat history (via `localStorage`)
- 📜 Auto-trims long conversations to stay within token limits (~20k tokens)
- 📂 Sidebar to browse and switch between past chats
- 🌐 Works with any RAG API that:
  - Accepts `POST` with `{"query": "..."}` in JSON body
  - Uses Bearer token authentication
  - Returns answer in common fields like `answer`, `output`, `response`, etc.
- 🛡️ Built-in CORS proxy support (optional)

---

## 🛠️ How to Use

### 1. **Get Your API URL & Bearer Token**

This app works with **any RAG API** that follows standard patterns. Examples:

#### 🔹 Cloudflare AI Gateway (Workers AI + Vectorize)
- **API URL**:  
  `https://api.cloudflare.com/client/v4/accounts/<ACCOUNT_ID>/autorag/rags/<RAG_ID>/ai-search`
- **Token**: Your [Cloudflare API Token](https://dash.cloudflare.com/profile/api-tokens) with **"Account AI Read"** permission

#### 🔹 Custom RAG Backend (e.g., FastAPI, Flask, LangServe)
- **API URL**:  
  `https://your-rag-api.example.com/query`  
  (must accept `{"query": "..."}` and return JSON with `answer` field)
- **Token**: Any Bearer token your backend expects (e.g., from Auth0, Firebase, or a static key)

> 💡 **Tip**: If you're building your own RAG API, ensure it responds with a JSON object like:
> ```json
> {
>   "answer": "The capital of France is Paris.",
>   "sources": ["https://example.com/doc1", "https://example.com/doc2"]
> }
> ```

---

### 2. **Run the App**

#### Option A: Open Directly in Browser (Recommended)
1. Download [`index.html`](./index.html)
2. Open it in **Chrome, Edge, or Firefox**
3. Enter:
   - **Full API URL** (e.g., `https://api.yourservice.com/rag`)
   - **Bearer Token** (your secret key)
4. Click **Save** → Start chatting!

> ⚠️ **Note**: Some browsers block `localStorage` on `file://` URLs. If chat history doesn’t persist:
> - Serve via a local server:  
>   ```bash
>   npx serve  # or python3 -m http.server 8000
>   ```

#### Option B: Host Online
- Deploy `index.html` to GitHub Pages, Netlify, Vercel, etc.
- Works out of the box!

#### Option C: Use My Hosted one
- Visit https://rag-chat-dk0.pages.dev/ 
- Use.
---

### 3. **Using the Interface**

| Element | Function |
|--------|--------|
| **API URL field** | Paste your full RAG endpoint |
| **Token field** | Enter your Bearer token (hidden) |
| **Save button** | Stores API URL in browser (token is *never saved*) |
| **New Chat** | Starts fresh conversation |
| **☰ (Sidebar)** | View & switch between past chats |
| **Send / Enter** | Submit message (use `Shift+Enter` for new line) |

> 🔐 **Security Note**: Your **token is never stored**—only the API URL is saved in `localStorage` for convenience.

---

## 🔌 Expected API Contract

Your backend must:

1. Accept `POST` requests with:
   ```json
   { "query": "User's full contextual question" }
   ```
2. Use Bearer auth:
   ```http
   Authorization: Bearer <your-token>
   ```
3. Return JSON with **at least one** of these fields:
   - `answer`
   - `output`
   - `response`
   - `text`
   - Or a string at the root level

Optional: Include `sources` as an array of URLs for citation.

✅ **Example Response**:
```json
{
  "answer": "Paris is the capital of France.",
  "sources": ["https://en.wikipedia.org/wiki/Paris"]
}
```

---

## 🌐 CORS Support

If your API blocks cross-origin requests, the app uses a **public CORS proxy** by default:

```
https://cors.jammesop007.workers.dev/?u=<YOUR_ENCODED_URL>
```

> ⚠️ **For production**: Replace this with your own proxy or ensure your API allows `Origin: null` (for `file://`) or your domain.

To use your own proxy, edit this line in the code:
```js
const CORS_PROXY = "https://your-proxy.com/?u=";
```

---

## 🧪 Testing Locally

Want to test without a real API? Use a mock service:

1. Create a mock endpoint that returns:
   ```json
   { "answer": "This is a test response!", "sources": ["https://example.com"] }
   ```
2. Use the mock URL as your **API URL**
3. Put any string as the token (it won’t be validated)

---

## 📜 License

MIT — feel free to use, modify, and embed in your projects!

---

## 💡 Inspired By

- Cloudflare AI Gateway demos
- LangChain conversational RAG patterns
- Privacy-focused AI interfaces

---

> ✨ **Built for developers, analysts, and AI tinkerers who want full control — without backend hassle.**
```
