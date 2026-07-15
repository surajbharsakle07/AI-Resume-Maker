# Resume Composer — AI Resume Maker

Full-stack resume builder with a Node.js/Express backend that proxies
**streaming** responses from the Anthropic Claude API. The frontend is
plain, responsive HTML/CSS/JS — no build step required.

```
resume-maker-app/
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
├── .gitignore
├── .env.example
├── package.json
├── server.js          # Express server + secure streaming proxy to Claude
└── public/
    ├── index.html
    ├── styles.css
    └── app.js          # form, live preview, SSE streaming client
```

## How it works

- The browser never talks to Anthropic directly and never sees an API key.
- The browser calls `POST /api/enhance` on **our own server**.
- The server builds the prompt, calls the Claude API with `stream: true`,
  and re-streams the tokens back to the browser over Server-Sent Events
  as they arrive — so AI text appears progressively, not all at once.
- `ANTHROPIC_API_KEY` is read only from `process.env` on the server and
  is never bundled into any file the browser downloads.

## Run locally (no Docker)

```bash
npm install
cp .env.example .env
# edit .env and paste your real ANTHROPIC_API_KEY
npm start
# open http://localhost:8080
```

## Run with Docker

```bash
cp .env.example .env
# edit .env with your real key

docker build -t resume-maker-app .
docker run --rm -p 8080:8080 --env-file .env resume-maker-app
# open http://localhost:8080
```

Or with Compose:

```bash
docker compose up --build
```
