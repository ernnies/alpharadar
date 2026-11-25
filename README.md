# AlphaRadar

**AI-Powered Crypto Opportunity Discovery Engine**

AlphaRadar is an AI-assisted crypto market intelligence platform designed to help users **discover, investigate, and monitor potential market opportunities**. It brings together crypto market data, sector momentum, news catalysts, transparent opportunity scores, watchlists, and alerts in one research-focused dashboard.

> **Project status:** In development. This README describes the intended MVP and development setup; listed features and integrations should not be interpreted as already implemented.

## Overview

Crypto markets move quickly, and useful information is spread across price feeds, sector dashboards, and news sources. AlphaRadar aims to make research more efficient by combining those inputs into explainable, evidence-backed signals.

Rather than promising profitable trades, AlphaRadar is designed to answer:

- **What's changing?** Identify unusual momentum, trading volume, and sector activity.
- **Why might it matter?** Explain possible catalysts with links to supporting sources.
- **What are the risks?** Highlight liquidity, volatility, weak evidence, and stale data.
- **What should I monitor?** Track assets and receive customizable alerts.

AlphaRadar is being developed as an entry for the **SoSoValue WaveHack Buildathon**. The project prioritizes meaningful SoSoValue integration, practical utility, working functionality, and clear user experience.

## Planned MVP Features

| Feature | Description | Status |
| --- | --- | --- |
| Opportunity Radar | Rank assets using market momentum, volume changes, sector strength, and catalysts | Planned |
| Market Dashboard | Browse market trends, asset performance, and relevant charts | Planned |
| AI Catalyst Analysis | Generate source-grounded summaries explaining market developments | Planned |
| Transparent Opportunity Scores | Show weighted indicators and evidence quality, not opaque buy/sell tips | Planned |
| Risk Signals | Flag volatility, liquidity concerns, insufficient evidence, and outdated data | Planned |
| Watchlists | Save assets to monitor | Planned |
| Smart Alerts | Configure notifications for meaningful market changes | Planned |
| Signal History | Record prior scores and track subsequent market outcomes | Planned |

### Example Scoring Model

The initial **experimental** opportunity score ranges from 0–100 and combines normalized inputs:

| Indicator | Initial weight |
| --- | ---: |
| Price momentum | 30% |
| Volume anomaly | 25% |
| News catalyst | 20% |
| Sector strength | 15% |
| Liquidity quality | 10% |

These weights are starting assumptions, not validated trading rules. Weights, lookback windows, eligibility requirements, and risk thresholds should be tested against historical data. The scoring engine will compute indicators deterministically; AI-generated text will explain signals rather than fabricate quantitative inputs. Missing or stale data should reduce confidence or prevent a signal from being ranked.

## Technology Stack

### Frontend

- **Next.js** (App Router), **React**, **TypeScript**
- **Tailwind CSS** and **shadcn/ui** for styling and UI components
- **Lucide React** for icons; **Framer Motion / Motion** for animations
- **TradingView Lightweight Charts** and **Recharts** for visualizations
- **TanStack Query** and **Axios** for API communication
- **Zustand** for local client state
- **React Hook Form** and **Zod** for forms and validation
- **Supabase** for authentication and persistence
- **Sonner** for in-app notifications

### Backend (planned)

- **FastAPI** and **Python** for market analysis and APIs
- **SoSoValue API** as the primary intelligence provider, subject to available endpoints and access
- Additional permitted market-data provider(s), where needed
- **OpenAI API** for structured, evidence-grounded catalyst explanations
- **PostgreSQL** for application and signal data
- **Redis + Celery** for scheduled ingestion and asynchronous analysis
- **Pandas / NumPy** for calculations where appropriate

### Deployment (planned)

- **Vercel** for the frontend
- **Railway** or another compatible service for FastAPI and worker processes
- **Supabase** for managed PostgreSQL and authentication
- **Sentry** for application monitoring

## Repository Layout

The Next.js application is created **directly inside `frontend/`**, not inside another nested `web/` directory.

```text
alpharadar/
├── frontend/                # Next.js / React application
│   ├── public/
│   ├── src/
│   │   ├── app/             # App Router pages and layouts
│   │   ├── components/      # Reusable UI and dashboards
│   │   ├── features/        # Opportunities, markets, alerts, watchlists
│   │   ├── hooks/
│   │   ├── lib/             # API clients and utilities
│   │   ├── providers/
│   │   ├── store/
│   │   └── types/
│   ├── package.json
│   └── .env.local           # Local secrets/config (do not commit)
├── backend/                 # Planned FastAPI service
│   ├── app/
│   │   ├── api/
│   │   ├── providers/
│   │   ├── scoring/
│   │   ├── ai/
│   │   ├── alerts/
│   │   └── workers/
│   └── tests/
├── docs/                    # Architecture and buildathon notes (planned)
└── README.md
```

> The layout above is a **target structure**. Some folders will be created as features are implemented.

## Getting Started

### Prerequisites

- Node.js and npm versions compatible with your installed Next.js release (a current Node.js LTS release is recommended)
- Git
- Python 3.11+ for the planned backend
- API credentials for connected services when you enable those features

### 1. Clone the repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd alpharadar
```

Replace `<YOUR_REPOSITORY_URL>` with the actual GitHub repository URL.

### 2. Set up the frontend

If the `frontend/` application has **already been initialized**, run:

```bash
cd frontend
npm install
```

If you are starting from a fresh repository **without** the `frontend/` application, run from the repository root:

```bash
npx create-next-app@latest frontend --typescript --tailwind --eslint --app --src-dir --import-alias "@/*" --use-npm
cd frontend
```

If `frontend/` already exists but is empty, initialize Next.js in that folder with `npx create-next-app@latest .` using the same flags instead. Do **not** re-run initialization inside a configured Next.js project.

### 3. Install project packages (if not already in `package.json`)

```bash
npm install lucide-react framer-motion clsx tailwind-merge class-variance-authority
npm install lightweight-charts recharts
npm install axios @tanstack/react-query zustand
npm install react-hook-form zod @hookform/resolvers
npm install @supabase/supabase-js @supabase/ssr
npm install date-fns sonner
npm install -D prettier prettier-plugin-tailwindcss
```

Initialize **shadcn/ui** if not already configured:

```bash
npx shadcn@latest init
```

Add UI primitives as they are needed:

```bash
npx shadcn@latest add button card badge input label table tabs dialog dropdown-menu select skeleton separator sheet sidebar tooltip progress alert switch checkbox avatar scroll-area
```

> The UI component commands write files into the application. Review conflicts before overwriting components you've already customized.

### 4. Configure frontend environment variables

Create `frontend/.env.local`:

```dotenv
NEXT_PUBLIC_APP_NAME=AlphaRadar
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=
```

Populate Supabase values only after creating a project. Code paths that rely on Supabase should handle unconfigured credentials in development.

**Never expose the SoSoValue or OpenAI secret API keys through `NEXT_PUBLIC_` variables.** Keep secrets in the backend or protected server-side environment variables. Do not commit `.env.local` or other secret-bearing files.

### 5. Run the frontend

```bash
npm run dev
```

Visit **http://localhost:3000**.

### 6. Backend setup (planned)

The Python backend will be set up separately in `backend/`. Once its requirements and entry point are committed, this README will be updated with exact installation, environment, and start commands. The default planned local backend address is `http://localhost:8000`.

## Architecture

```text
SoSoValue API ──────┐
                    ├─> Data ingestion & normalization
Other market feeds ─┘                 │
                                     ▼
                         Deterministic scoring engine
                                     │
                          Risk & evidence checks
                                     │
                                     ▼
                         AI explanation service
                                     │
                                     ▼
                       PostgreSQL / signal history
                                     │
                       Alerts & API delivery
                                     │
                                     ▼
                           Next.js dashboard
```

Core design principles:

1. **Traceability:** Associate insights with source data and timestamps.
2. **Separation of concerns:** Keep market calculations separate from AI-generated prose.
3. **Graceful degradation:** Allow deterministic scores to work even if the AI service is temporarily unavailable.
4. **Security:** Validate server-side inputs, protect API keys, and apply authentication and authorization to private data.
5. **Transparency:** Label simulated, delayed, incomplete, or stale results explicitly.

## Development Roadmap

- [ ] Initialize and configure the Next.js frontend
- [ ] Build the responsive dashboard shell and navigation
- [ ] Verify SoSoValue API access and supported data endpoints
- [ ] Integrate market and sector data
- [ ] Implement normalization and opportunity scoring
- [ ] Add source-grounded AI catalyst explanations
- [ ] Build asset details and price charts
- [ ] Add authentication and watchlists
- [ ] Add scheduled scans and configurable alerts
- [ ] Save signal history and outcome measurements
- [ ] Add unit, integration, and end-to-end tests
- [ ] Deploy the public demo and document setup
- [ ] Publish buildathon demo and progress updates

Update the checkboxes as each item is genuinely completed.

## Buildathon Deliverables

The submission package is intended to include:

- A public source-code repository
- A deployed, accessible demo
- Documentation covering installation, architecture, APIs, and limitations
- An overview of the product and its value proposition
- A wave progress changelog
- A short demo video and screenshots, if requested or useful

Confirm final submission requirements and deadlines with the official program platform.

## Testing and Quality

The frontend is intended to use **Vitest** and **Playwright** for component and end-to-end tests. The backend is intended to use **Pytest** for indicator, scoring, API, and integration tests. Test commands will be documented when the relevant configuration is committed.

For a credible financial research product, especially test:

- Indicator calculations and normalization
- Missing, delayed, or inconsistent market data
- Duplicate news and alert deduplication
- Permissions for private watchlists
- Evidence linking and AI output validation
- Historical outcome tracking without look-ahead bias

## Responsible Use

AlphaRadar is an **informational research tool**, not a broker, financial adviser, or promise of investment returns. Scores express an experimental ranking methodology; they do not establish the probability of profit. Crypto assets are volatile, and users should independently evaluate risks before making financial decisions.

The initial MVP does **not** require wallet custody, transaction signing, or automated trading.

## Contributing

Issues and pull requests are welcome as the repository evolves. For substantial changes, open an issue describing the proposed change and its impact. Do not include real credentials, private user data, or proprietary market-data exports in contributions.

## License

**License not yet selected.** Add a `LICENSE` file before describing the repository as open source under a specific license.

---

**AlphaRadar — Discover the signal. Understand the catalyst. Evaluate the risk.**
