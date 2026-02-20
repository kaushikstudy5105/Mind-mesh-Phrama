<<<<<<< HEAD
<p align="center">
  <img src="./src/assets/pharma-logo.png" alt="PharmaGuard Logo" width="80" />
</p>

<h1 align="center">PharmaGuard — Genomic Toxicity Analyzer</h1>

<p align="center">
  <strong>AI-powered pharmacogenomic risk analysis platform</strong><br/>
  Upload VCF files, select drugs, and instantly assess drug interaction risks using CPIC guidelines &amp; Gemini AI.
</p>

<p align="center">
  <a href="#live-demo">Live Demo</a> •
  <a href="#video-walkthrough">Video</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#tech-stack">Tech Stack</a> •
  <a href="#installation">Installation</a> •
  <a href="#api-documentation">API Docs</a> •
  <a href="#usage-examples">Usage</a> •
  <a href="#team">Team</a>
</p>

---

## 🌐 Live Demo

> **[https://pharmaguard.vercel.app](https://pharmaguard.vercel.app)** *(update with your actual deployment URL)*

## 🎥 Video Walkthrough

> **[Watch on LinkedIn](https://www.linkedin.com/in/YOUR_PROFILE/recent-activity/)** *(update with your LinkedIn video URL)*

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                    FRONTEND                         │
│    Vite + React + TypeScript + shadcn/ui            │
│                                                     │
│  ┌──────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  Upload   │  │   Drug       │  │   Results    │  │
│  │  VCF File │──│   Selector   │──│   Dashboard  │  │
│  └──────────┘  │  + Custom     │  │  + Charts    │  │
│                │    Drug Input │  │  + Summary   │  │
│                └──────────────┘  └──────────────┘  │
│         Clerk Auth │                     │          │
└────────────────────┼─────────────────────┼──────────┘
                     │   REST API          │
┌────────────────────┼─────────────────────┼──────────┐
│                    ▼     BACKEND         ▼          │
│              FastAPI (Python)                       │
│                                                     │
│  ┌──────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  VCF     │  │  Pharmaco-   │  │  Risk        │  │
│  │  Parser  │──│  genomics    │──│  Engine      │  │
│  │          │  │  Resolver    │  │  (CPIC)      │  │
│  └──────────┘  └──────────────┘  └──────┬───────┘  │
│                                         │          │
│  ┌──────────────────────────────────────▼───────┐  │
│  │         Gemini 1.5 Pro LLM Service           │  │
│  │   ┌──────────────┐  ┌─────────────────────┐  │  │
│  │   │ CPIC-Grounded│  │ Custom Drug         │  │  │
│  │   │ Explanations │  │ AI Analysis         │  │  │
│  │   └──────────────┘  └─────────────────────┘  │  │
│  └──────────────────────────────────────────────┘  │
│                         │                           │
│                    ┌────▼─────┐                     │
│                    │ Supabase │                     │
│                    │ (Storage)│                     │
│                    └──────────┘                     │
└─────────────────────────────────────────────────────┘
```

### Data Flow

1. **Upload** → VCF file parsed & validated for VCF v4.2 compliance
2. **Select Drugs** → Choose from 6 CPIC-supported drugs and/or add custom drug names
3. **Analyze** → All drugs analyzed in parallel:
   - **Supported drugs**: VCF → Gene variants → Diplotype resolution → CPIC risk lookup → LLM clinical explanation
   - **Custom drugs**: VCF variants → LLM-powered pharmacogenomic assessment
4. **Results** → Interactive dashboard with risk cards, charts, and AI-generated clinical summaries

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Frontend** | Vite, React 18, TypeScript | Fast dev server, component-based UI |
| **UI Components** | shadcn/ui, Radix UI, Tailwind CSS | Accessible, themeable components |
| **Charts** | Recharts | Interactive data visualization |
| **Authentication** | Clerk | Secure sign-in/sign-up flows |
| **Backend** | FastAPI (Python 3.10+) | High-performance async API |
| **AI/LLM** | Google Gemini 1.5 Pro | Clinical explanation generation |
| **Database** | Supabase | Analysis result storage |
| **Validation** | Pydantic v2 | Strict JSON schema enforcement |
| **Guidelines** | CPIC | Pharmacogenomic risk assessment rules |

---

## 📦 Installation

### Prerequisites

- **Node.js** 18+ and npm
- **Python** 3.10+
- **Google Gemini API Key** ([Get one here](https://aistudio.google.com/app/apikey))
- **Supabase Project** ([Create one here](https://supabase.com))
- **Clerk Account** ([Sign up here](https://clerk.com))

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_ORG/Hackathon-medic2.git
cd Hackathon-medic2
```

### 2. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your keys:
#   GEMINI_API_KEY=your_gemini_key
#   SUPABASE_URL=your_supabase_url
#   SUPABASE_KEY=your_supabase_key

# Start the server
uvicorn app.main:app --reload --port 8000
```

### 3. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Configure environment
echo "VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key" > .env

# Start the dev server
npm run dev
# Opens at http://localhost:8080
```

### 4. Verify

- **Backend API docs**: http://localhost:8000/docs
- **Frontend app**: http://localhost:8080
- **Health check**: http://localhost:8000/api/health

---

## 📡 API Documentation

Base URL: `http://localhost:8000`

Interactive Swagger UI available at `/docs`.

### Endpoints

#### `GET /api/health`
Health check endpoint.

```json
// Response
{ "status": "healthy", "app": "PharmaGuard", "version": "1.0.0" }
```

---

#### `GET /api/supported-drugs`
Returns the list of CPIC-supported drugs and their primary genes.

```json
// Response
{
  "drugs": [
    { "name": "CODEINE", "primary_gene": "CYP2D6" },
    { "name": "WARFARIN", "primary_gene": "CYP2C9" },
    { "name": "CLOPIDOGREL", "primary_gene": "CYP2C19" },
    { "name": "SIMVASTATIN", "primary_gene": "SLCO1B1" },
    { "name": "AZATHIOPRINE", "primary_gene": "TPMT" },
    { "name": "FLUOROURACIL", "primary_gene": "DPYD" }
  ]
}
```

---

#### `POST /api/validate-vcf`
Validates a VCF file without running analysis.

| Parameter | Type | Description |
|-----------|------|-------------|
| `file` | `UploadFile` | VCF file (`.vcf`, UTF-8) |

```json
// Response
{
  "is_valid": true,
  "errors": [],
  "warnings": [],
  "sample_id": "PATIENT_001",
  "variant_count": 42,
  "pharmacogene_variants_found": 6
}
```

---

#### `POST /api/analyze`
Full pharmacogenomic risk analysis. Accepts both predefined CPIC drugs and custom drug names.

| Parameter | Type | Description |
|-----------|------|-------------|
| `file` | `UploadFile` | VCF file (`.vcf`, UTF-8) |
| `drugs` | `string` (Form) | Comma-separated drug names (e.g., `"CODEINE,WARFARIN,METFORMIN"`) |

```json
// Response
{
  "results": [
    {
      "patient_id": "PATIENT_001",
      "drug": "CODEINE",
      "timestamp": "2026-02-20T00:00:00",
      "risk_assessment": {
        "risk_label": "Safe",
        "confidence_score": 0.85,
        "severity": "none"
      },
      "pharmacogenomic_profile": {
        "primary_gene": "CYP2D6",
        "diplotype": "*1/*1",
        "phenotype": "NM",
        "detected_variants": [
          { "rsid": "rs3892097", "chromosome": "chr22", "position": 42526694, "genotype": "G/G", "impact": "Normal function" }
        ]
      },
      "clinical_recommendation": {
        "cpic_guideline_reference": "https://cpicpgx.org/guidelines/cpic-guideline-for-codeine-and-cyp2d6/",
        "recommended_action": "Use label-recommended dosage",
        "dose_adjustment": "None",
        "alternative_drugs": [],
        "monitoring_required": false
      },
      "llm_generated_explanation": {
        "summary": "Patient has CYP2D6 *1/*1, indicating Normal Metabolizer status...",
        "mechanism_of_action": "CYP2D6 converts codeine to its active metabolite morphine...",
        "variant_significance": "The rs3892097 G/G genotype indicates normal CYP2D6 function...",
        "dosing_rationale": "Standard dosing is appropriate for Normal Metabolizers..."
      },
      "quality_metrics": {
        "vcf_parsing_success": true,
        "variant_match_confidence": 0.85,
        "llm_grounded_on_guidelines": true,
        "processing_time_ms": 1200
      }
    }
  ],
  "total_drugs_analyzed": 1,
  "overall_processing_time_ms": 1500
}
```

---

## 🚀 Usage Examples

### Web Interface

1. **Sign in** with your Clerk account at `http://localhost:8080`
2. **Upload** a `.vcf` file using the drag-and-drop upload zone
3. **Select drugs** — click predefined drug chips (Codeine, Warfarin, etc.) or type a custom drug name and click **Add**
4. **Run Analysis** — click the analyze button; all drugs are processed in parallel
5. **View Results** — explore the interactive dashboard:
   - **Overview Charts** — toxicity breakdown, risk distribution
   - **Per-Drug Analysis** — detailed cards with risk labels, gene profiles, and AI explanations
6. **View Summary** — navigate to the full summary page for a comprehensive report

### cURL Example

```bash
# Analyze Codeine and a custom drug against a VCF file
curl -X POST http://localhost:8000/api/analyze \
  -F "file=@sample.vcf" \
  -F "drugs=CODEINE,METFORMIN"
```

```bash
# Validate a VCF file
curl -X POST http://localhost:8000/api/validate-vcf \
  -F "file=@sample.vcf"
```

```bash
# List supported drugs
curl http://localhost:8000/api/supported-drugs
```

### Python Example

```python
import requests

# Full analysis
with open("sample.vcf", "rb") as f:
    response = requests.post(
        "http://localhost:8000/api/analyze",
        files={"file": ("sample.vcf", f, "text/plain")},
        data={"drugs": "CODEINE,WARFARIN,SIMVASTATIN"},
    )

data = response.json()
for result in data["results"]:
    print(f"{result['drug']}: {result['risk_assessment']['risk_label']} "
          f"(confidence: {result['risk_assessment']['confidence_score']})")
```

---

## 🔑 Key Features

| Feature | Description |
|---------|-------------|
| **VCF Parsing** | Validates VCF v4.2 files with detailed error/warning reports |
| **6 CPIC Drugs** | Codeine, Warfarin, Clopidogrel, Simvastatin, Azathioprine, Fluorouracil |
| **Custom Drugs** | Add any drug name — analyzed via AI with pharmacogenomic context |
| **Parallel Processing** | All drugs analyzed simultaneously for faster results |
| **AI Explanations** | Gemini 1.5 Pro generates CPIC-grounded clinical summaries |
| **Risk Classification** | 5-tier system: Safe → Low → Moderate → High → Critical |
| **Interactive Dashboard** | Charts, risk cards, and detailed per-drug analysis panels |
| **Secure Auth** | Clerk-powered authentication with protected routes |
| **Dark Theme** | Biotech Neon Clinical design with teal/navy palette |
| **Error Handling** | Clear messages for invalid files, graceful fallbacks for missing data |

---

## 📁 Project Structure

```
Hackathon-medic2/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   └── routes.py          # API endpoints
│   │   ├── models/
│   │   │   └── schemas.py         # Pydantic response schemas
│   │   ├── services/
│   │   │   ├── vcf_parser.py      # VCF file parsing & validation
│   │   │   ├── pharmacogenomics.py # Diplotype/phenotype resolution
│   │   │   ├── risk_engine.py     # CPIC risk prediction rules
│   │   │   ├── llm_service.py     # Gemini AI clinical explanations
│   │   │   └── supabase_client.py # Database persistence
│   │   ├── config.py              # App settings & env vars
│   │   └── main.py                # FastAPI entrypoint
│   ├── vcf_samples/               # Sample VCF files for testing
│   ├── requirements.txt
│   └── .env
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── DrugSelector.tsx    # Drug selection + custom input
│   │   │   ├── FileUploadCard.tsx  # VCF file upload zone
│   │   │   └── ResultsPanel.tsx    # Analysis results dashboard
│   │   ├── pages/
│   │   │   ├── Index.tsx           # Main analysis page
│   │   │   ├── Summary.tsx         # Full summary report
│   │   │   ├── SignInPage.tsx      # Clerk sign-in
│   │   │   └── SignUpPage.tsx      # Clerk sign-up
│   │   ├── lib/
│   │   │   └── api.ts             # Backend API client
│   │   ├── types/
│   │   │   └── pharma.ts          # TypeScript types & transformers
│   │   └── App.tsx                # Routes & providers
│   ├── package.json
│   └── .env
│
└── README.md
```

---

## 👥 Team

| Name | Role | LinkedIn |
|------|------|----------|
| **Team Member 1** | Full-Stack Developer | [LinkedIn](https://linkedin.com/in/PROFILE) |
| **Team Member 2** | Backend / AI Engineer | [LinkedIn](https://linkedin.com/in/PROFILE) |
| **Team Member 3** | Frontend / UI Designer | [LinkedIn](https://linkedin.com/in/PROFILE) |
| **Team Member 4** | Data / Genomics | [LinkedIn](https://linkedin.com/in/PROFILE) |

---

## 📝 License

This project was built for **hackathon purposes** and is intended for **research use only**.
Not intended for clinical diagnosis or medical decision-making.

---

<p align="center">
  <sub>Built with ❤️ at Hackathon 2026 · Powered by CPIC Guidelines &amp; Google Gemini</sub>
</p>
=======
# Mind-mesh-Phrama
RIFT2026
>>>>>>> 2799ee56bdaa9a6155d6860ea2e196a778d085cd
