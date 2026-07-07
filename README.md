# Business Idea Generator

An AI-powered SaaS application that generates business ideas for AI agents in real time. Built as a full-stack project with a **Next.js** frontend and a **FastAPI** Python backend, deployed together on **Vercel**.

![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-Python-009688?logo=fastapi&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-Streaming-412991?logo=openai&logoColor=white)

## Features

- **Real-time AI streaming** — Server-Sent Events (SSE) stream responses from OpenAI as they are generated
- **Markdown rendering** — Headings, lists, and formatted content rendered with `react-markdown`
- **Modern UI** — Tailwind CSS with gradient backgrounds, dark mode support, and responsive layout
- **Full-stack on Vercel** — Next.js frontend and FastAPI backend deployed as a single project with no extra configuration

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | Next.js 16 (Pages Router), React 19, TypeScript |
| Styling | Tailwind CSS 4, `@tailwindcss/typography` |
| Backend | FastAPI, Uvicorn |
| AI | OpenAI API (`gpt-5-nano`) with streaming |
| Deployment | Vercel (Next.js + Python serverless functions) |

## Architecture

```
Browser                    Vercel
┌─────────────┐           ┌──────────────────────────────────┐
│  index.tsx  │  SSE      │  api/index.py  (FastAPI)         │
│  EventSource├──────────►│  GET /api  →  OpenAI streaming   │
│  ReactMarkdown           │                                  │
└─────────────┘           └──────────────────────────────────┘
```

1. The React frontend opens an `EventSource` connection to `/api`
2. Vercel routes `/api` requests to the Python FastAPI handler in `api/index.py`
3. FastAPI streams OpenAI completion chunks as Server-Sent Events
4. The frontend accumulates chunks and renders them as Markdown

## Project Structure

```
saas/
├── api/
│   └── index.py          # FastAPI backend — SSE streaming endpoint
├── pages/
│   ├── _app.tsx          # App wrapper (global styles)
│   ├── _document.tsx     # Custom HTML document
│   └── index.tsx         # Homepage — Business Idea Generator UI
├── styles/
│   └── globals.css       # Tailwind + Markdown content styles
├── public/               # Static assets
├── week1/                # Course guides (Days 1–5)
├── requirements.txt      # Python dependencies
├── package.json          # Node.js dependencies
└── next.config.ts        # Next.js configuration
```

## Prerequisites

- [Node.js](https://nodejs.org/) 18+
- [Python](https://www.python.org/) 3.9+
- An [OpenAI API key](https://platform.openai.com/api-keys)
- [Vercel CLI](https://vercel.com/docs/cli) (for deployment and local full-stack dev)

## Getting Started

### 1. Clone and install dependencies

```bash
git clone https://github.com/YOUR_USERNAME/saas.git
cd saas

npm install
pip install -r requirements.txt
```

### 2. Configure environment variables

Create a `.env.local` file in the project root:

```env
OPENAI_API_KEY=sk-your-key-here
```

> **Never commit API keys.** The `.env.local` file is listed in `.gitignore`.

### 3. Run locally

Because this project combines a Next.js frontend with a Python backend, use the Vercel CLI to run both together:

```bash
vercel dev
```

Open [http://localhost:3000](http://localhost:3000) to see the app generate a business idea in real time.

Alternatively, deploy a preview to test the full stack:

```bash
vercel link    # first time only
vercel .
```

## Deploy to Production

```bash
# Add your OpenAI key to Vercel (first time only)
vercel env add OPENAI_API_KEY

# Deploy to production
vercel --prod
```

Vercel automatically detects the Next.js app and the Python files in `api/` — no `vercel.json` is required.



## Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start Next.js dev server (frontend only) |
| `npm run build` | Build for production |
| `npm run start` | Start production Next.js server |
| `npm run lint` | Run ESLint |
| `vercel dev` | Run full-stack app locally (frontend + Python API) |
| `vercel --prod` | Deploy to production |

## License

This project is for educational purposes.
