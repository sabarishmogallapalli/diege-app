# DIEGE — Web Interface

> An editorial, mood-first cinematic search logbook that maps long-form human scenarios directly to curated film recommendations.

[![React](https://img.shields.io/badge/React-18.x-61DAFB?style=flat-square&logo=react&logoColor=black)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Material-UI](https://img.shields.io/badge/MUI-5.x-007FFF?style=flat-square&logo=mui&logoColor=white)](https://mui.com/)
[![Sass](https://img.shields.io/badge/Sass-SCSS-CC6699?style=flat-square&logo=sass&logoColor=white)](https://sass-lang.com/)
[![License](https://img.shields.io/badge/License-MIT-green.style=flat-square)](#)

🌐 **Live Application:** [https://diege-app.vercel.app](https://diege-app.vercel.app) *(or your deployed Vercel/Netlify link)*

---

## 1. The Problem & Solution

Traditional movie discovery platforms rely on strict categorical filtering (Genre: *Drama*, Year: *2019*, Rating: *>8.0*). This fails when a user has a specific, complex mood or environmental narrative in mind—such as *"A melancholic Sunday evening looking for sweeping, atmospheric cinematography without a heavy storyline."*

**DIEGE** solves this by providing a minimalist, logbook-style interactive interface where users describe complete scenarios in plain English. The frontend streams these atmospheric prompts down to an NLP vector space engine, presenting the matched results in an editorial, journal-inspired UI.

---

## 2. System Architecture & Data Flow

```
[ User Input / Prompts ]
          │
          ▼
   <Main Component>  ──( State: Selected Platform Filters [8, 9, 1899] )
          │
          ▼
   <App Component>   ──( Asynchronous HTTP POST Payload )
          │
          ├──► [ FastAPI NLP Vector Backend on Render ]
          │            │
          │            ▼ (Cosine Similarity Search over 384D Embeddings)
          │            │
          │            ▼
          ◄────────────┘ (JSON Response: Film Metadata + Vibe Match Score + Low Confidence Flag)
          │
          ▼
   [ Logbook Grid ]  ──( Asynchronous TMDB Visual Extras Fetch: Posters, Directors, Credits )
```

* **State Hydration:** Captures user scenario strings and platform ID filters (`[8, 9, 1899]`) and routes them asynchronously to the FastAPI backend.
* **Low-Confidence Fallback Handling:** Intercepts vector noise or invalid inputs and gracefully displays system fallback states to preserve UX continuity.
* **Metadata Augmentation:** Integrates with TMDB's REST API on the client side to fetch poster art and director credits without blocking core vector search execution.

---

## 3. Local Setup Instructions

### Prerequisites
* **Node.js** `>=18.0.0`
* **npm** or **yarn**

### Quickstart

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/diege-web.git
   cd diege-web
   ```

2. **Install project dependencies:**
   ```bash
   npm install
   ```

3. **Configure Environment Variables:**
   Create a `.env` file in the root directory:
   ```env
   REACT_APP_API_URL=http://127.0.0.1:8000
   REACT_APP_TMDB_API_KEY=your_tmdb_api_key_here
   ```

4. **Launch Local Development Server:**
   ```bash
   npm start
   ```
   *The application will launch on `http://localhost:3000`.*

---

## 4. Key Components Map

```
src/
├── assets/             # Branding assets, logbook typography, and SASS styles
├── components/
│   ├── CardSpotlightEffect.tsx  # Dynamic interactive mouse hover backdrop
│   └── Main.tsx                 # Core UI dashboard, dynamic placeholders, & platform controls
├── App.tsx             # State orchestrator & FastAPI API bridge
└── index.tsx           # Entry React DOM mount point
```