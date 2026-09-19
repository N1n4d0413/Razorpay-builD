# 🚀 AI Commerce Agent

> **An AI-powered commerce network that understands customer intent, discovers the right products across merchants, and enables intelligent cross-merchant product discovery and checkout.**

## 📌 Overview

Traditional e-commerce search is usually limited to a single merchant's catalogue.

If a customer asks for:

> "Black sports shoes, size 9, under ₹1,500"

and the merchant cannot fulfill the exact request, the customer often has to leave and search somewhere else.

**AI Commerce Agent** changes this by turning product discovery into an intelligent, multi-merchant experience.

The agent understands the customer's natural-language intent, searches the merchant's catalogue, compares available offers, and — when the original merchant cannot fulfill the request — can discover relevant products from participating merchants.

The customer remains in control of the final purchase decision.

---

## 🎯 Problem

Modern commerce has several disconnected problems:

* Customers have to search through individual merchant catalogues.
* Product data differs significantly between merchants.
* The same product may have different names, attributes, units, and schemas.
* A merchant may have a relevant product but lack a specific variant or inventory.
* Customers may abandon a purchase when their exact requirement cannot be fulfilled.
* Merchants have limited visibility into unmet customer demand.
* Cross-merchant discovery and referral attribution are difficult to coordinate.

---

## 💡 Solution

The AI Commerce Agent creates an intelligent layer between **customers and participating merchants**.

### Core flow

```text
                    CUSTOMER
                       │
                       ▼
              AI COMMERCE AGENT
                       │
             Understand Intent
                       │
                       ▼
              Merchant Catalogue
                       │
              ┌────────┴────────┐
              │                 │
           Fulfilled         Not Fulfilled
              │                 │
              │                 ▼
              │        Commerce Intelligence
              │                 │
              │       ┌─────────┼─────────┐
              │       ▼         ▼         ▼
              │    Vendor B  Vendor C  Vendor D
              │      ₹1399     ₹1449     ₹1520
              │       │         │         │
              └───────┴─────────┴─────────┘
                              │
                              ▼
                       CUSTOMER CHOOSES
                              │
                              ▼
                       RAZORPAY CHECKOUT
                              │
                              ▼
                           PAYMENT
```

The AI facilitates **discovery**, rather than forcing the customer toward a particular merchant.

---

# 🧠 Key Features

## 1. Natural-Language Commerce

Customers can describe what they want naturally instead of navigating complicated filters.

Example:

```text
"I need a smartwatch for fitness tracking,
under ₹5,000, preferably with good battery life."
```

The system converts the request into structured commerce intent and semantic search requirements.

---

## 2. Multi-Merchant Product Discovery

If the current merchant cannot satisfy the customer's exact requirements, the system can search participating merchants for suitable alternatives.

Example:

```text
Customer Request
       ↓
Vendor A
       ↓
❌ Black / Size 9 unavailable
       ↓
AI Commerce Network
       ├── Vendor B → ₹1,399
       ├── Vendor C → ₹1,449
       └── Vendor D → ₹1,520
       ↓
Customer chooses
```

The customer is shown alternatives and makes the final decision.

---

## 3. Intelligent Product Matching

Different merchants may represent the same product differently.

For example:

```text
Merchant A:
"Samsung Galaxy Watch 7 44mm Black"

Merchant B:
"Galaxy Watch7 - 44mm - Black"

Merchant C:
"Samsung Watch 7 BT 44 Black"
```

The normalization and product intelligence pipeline converts these into a common representation before semantic retrieval and matching.

---

## 4. Hybrid RAG

Commerce search should not depend purely on semantic similarity.

The system combines:

```text
Structured Filtering
        +
Vector Search
        +
Business Rules
        ↓
Candidate Products
        ↓
Ranking
```

For example:

```text
price <= ₹3000
AND
stock > 0
AND
category = footwear

+
semantic similarity
+
merchant / offer signals
```

This makes the retrieval process suitable for commerce rather than treating it as a generic document-search problem.

---

## 5. Product Intelligence

The platform processes heterogeneous merchant datasets through a common pipeline:

```text
Raw Merchant Dataset
        ↓
Schema Detection
        ↓
Field Mapping
        ↓
Value Normalization
        ↓
Category Normalization
        ↓
Validation
        ↓
Canonical Product
```

The current dataset contains:

* 30 merchants
* 150,000 products
* Different merchant schemas
* Missing fields
* Different naming conventions
* Different prices
* Inventory differences
* Product variants
* Overlapping products
* Cross-category relationships

---

## 6. Consent-Based Customer Intelligence

Where appropriate and with customer consent, transaction-derived signals can improve product ranking.

Potential signals include:

* Typical order value
* Recent spending
* Purchase categories
* Accepted price ranges
* Purchase frequency
* Previously purchased products

For example:

```text
Customer usually purchases:
Sports footwear
₹2,000–₹3,000 range

        ↓

Agent prioritizes relevant products
within the customer's preferred range
```

**Customer financial information is not exposed to merchants.**

The architecture is designed around permissioned and privacy-preserving intelligence.

---

# 🔄 Closed-Loop Commerce Intelligence

The system can create a feedback loop between customer demand and merchant supply.

```text
Customer Intent
      ↓
Could Merchant A Fulfill?
      ↓
      NO
      ↓
Cross-Merchant Discovery
      ↓
Customer Clicks
      ↓
Purchase / No Purchase
      ↓
Which Merchant Converted?
      ↓
Which Product / Variant Was Missing?
      ↓
Which Price Converted?
      ↓
Merchant Analytics
      ↓
Inventory / Pricing Decisions
      ↓
Better Future Recommendations
```

This allows the system to learn from **unfulfilled demand and conversion outcomes**, rather than only performing static product search.

---

# 💰 Referral & Merchant Economics

When a merchant cannot fulfill a customer's request, another participating merchant may fulfill it.

The system can maintain referral attribution such as:

```text
Origin Merchant: Vendor A
Receiving Merchant: Vendor B

Product: Sports Shoes
Order Value: ₹1,399

Referral: AI Commerce Network
```

This creates the foundation for a merchant referral ecosystem.

Potential commercial mechanisms include:

* Referral fees
* Merchant incentives
* Network participation economics
* Inventory intelligence
* Demand insights

The exact settlement and commercial model would depend on the supported Razorpay marketplace/partner capabilities and the final implementation.

---

# 🏗️ Architecture

```text
                         ┌──────────────────────┐
                         │     React + Vite     │
                         │      Frontend        │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │       FastAPI        │
                         │       Backend        │
                         └──────────┬───────────┘
                                    │
                ┌───────────────────┼───────────────────┐
                │                   │                   │
                ▼                   ▼                   ▼
        ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
        │ AI Agent /   │    │ PostgreSQL   │    │  LLM Layer   │
        │ LangGraph    │    │ + pgvector   │    │ Gemini/Groq  │
        └──────┬───────┘    └──────┬───────┘    └──────────────┘
               │                   │
               ▼                   ▼
        Commerce Tools       Products / Offers
        - Search Products    Inventory
        - Compare Offers     Embeddings
        - Check Inventory    Relationships
        - Find Alternatives  Referrals
        - Build Bundle
               │
               ▼
        ┌──────────────────────┐
        │   Razorpay APIs      │
        │   Checkout / Payment │
        └──────────────────────┘
```

The planned stack uses FastAPI, PostgreSQL + pgvector, LangGraph, React/Vite, local embeddings, and Razorpay APIs.

---

# 🛠️ Technology Stack

| Layer              | Technology               |
| ------------------ | ------------------------ |
| Frontend           | React + Vite             |
| Backend            | Python + FastAPI         |
| AI Agent           | LangGraph                |
| LLM                | Gemini / Groq            |
| Database           | PostgreSQL               |
| Vector Search      | pgvector                 |
| Embeddings         | Open-source local models |
| Data Processing    | Pandas + Pydantic        |
| Fuzzy Matching     | RapidFuzz                |
| ML / Clustering    | scikit-learn             |
| Payments           | Razorpay APIs            |
| Containerization   | Docker                   |
| Backend Deployment | Render                   |
| Version Control    | GitHub                   |

The architecture intentionally avoids unnecessary infrastructure such as Redis and dedicated vector databases during the MVP stage.

---

# 📊 Current Progress

| Phase                      | Status        |
| -------------------------- | ------------- |
| Project Setup              | ✅ Complete    |
| PostgreSQL + DB Connection | ✅ Complete    |
| Database Schema            | ✅ Complete    |
| Dataset Generation         | ✅ Complete    |
| Data Normalization         | ✅ Complete    |
| PostgreSQL Ingestion       | ✅ Complete    |
| Product Intelligence       | 🟡 95%        |
| RAG                        | ⬜ In Progress |
| AI Agent / LangGraph       | ⬜ Planned     |
| Razorpay Integration       | ⬜ Planned     |
| Frontend                   | ⬜ Planned     |
| Deployment                 | ⬜ Planned     |

### Current Dataset

```text
30 Merchants
       ×
5,000 Products / Merchant
       =
150,000 Products
```

The data pipeline has successfully normalized and ingested all 150,000 products and corresponding inventory records.

---

# 🔬 Embedding & Product Intelligence

Three 384-dimensional embedding models have been evaluated/generated locally:

* BGE-small
* GTE-small
* all-MiniLM-L6-v2

The system currently has embeddings generated for all 150,000 products for each model, followed by vector-search validation.

The final benchmark evaluates:

```text
Recall@5
Recall@10
Recall@25
Recall@50
Recall@100
```

The benchmark will determine the embedding model used by the final RAG pipeline.

---

# 🤖 AI Agent

The planned agent will operate through controlled tools rather than directly accessing the database.

```text
AI Agent
   │
   ├── search_products()
   ├── search_merchants()
   ├── check_inventory()
   ├── find_similar_products()
   ├── find_complementary_products()
   ├── compare_offers()
   ├── build_bundle()
   ├── calculate_total()
   ├── create_order()
   └── record_referral()
```

This keeps the AI responsible for **reasoning and orchestration**, while deterministic tools handle commerce operations.

---

# 💳 Razorpay Integration

Razorpay is not treated merely as the final payment button.

The broader architecture uses Razorpay as part of the commerce intelligence and payment layer:

```text
Customer
   ↓
AI Product Discovery
   ↓
Merchant / Offer Selection
   ↓
Order Creation
   ↓
Razorpay Checkout
   ↓
Payment
   ↓
Referral Attribution
   ↓
Merchant Analytics
```

The planned payment flow creates the Razorpay order server-side before passing the resulting order ID to Checkout.

---

# 🔐 Privacy by Design

Merchant catalogues remain isolated.

```text
Merchant A Private Catalogue
             │
             │
             ▼
      Commerce Intelligence
             │
             ▼
      Permissioned Signals
             │
             ▼
      Cross-Merchant Discovery
```

The system does **not** require Vendor B to receive Vendor A's private catalogue.

Similarly, customer-level financial information should remain private to the customer's experience and only appropriate derived signals should be used for ranking.

---

# 🧩 Engineering & AI Concepts

This project combines several areas of computer engineering and AI:

* **Natural Language Understanding** — interpreting customer shopping requests.
* **Machine Learning** — semantic embeddings, similarity, and product intelligence.
* **Data Science** — merchant datasets, benchmarking, ranking and conversion analytics.
* **Software Engineering** — APIs, databases, modular architecture and deployment.
* **Algorithmic Foundations of Optimization** — candidate filtering and ranking.
* **Human-Computer Interaction** — conversational commerce and customer-controlled discovery.

---

# 🚀 Roadmap

### Phase 1 — Foundation

* [x] Backend setup
* [x] PostgreSQL
* [x] Database schema
* [x] Dataset generation
* [x] Normalization
* [x] Data ingestion

### Phase 2 — Product Intelligence

* [x] Semantic representation
* [x] Embedding generation
* [x] Vector search
* [x] Retrieval validation
* [ ] Final embedding benchmark

### Phase 3 — Hybrid RAG

* [ ] Query understanding
* [ ] Structured intent extraction
* [ ] SQL filtering
* [ ] Vector retrieval
* [ ] Candidate ranking
* [ ] Reranking

### Phase 4 — AI Agent

* [ ] LangGraph workflow
* [ ] Commerce tools
* [ ] Vendor comparison
* [ ] Alternative discovery
* [ ] Bundle generation

### Phase 5 — Razorpay

* [ ] Order creation
* [ ] Checkout
* [ ] Payment verification
* [ ] Referral attribution
* [ ] Merchant analytics

### Phase 6 — Product

* [ ] React chat interface
* [ ] Product cards
* [ ] Merchant alternatives
* [ ] Bundle interface
* [ ] Checkout experience

### Phase 7 — Deployment

* [ ] Docker deployment
* [ ] Backend hosting
* [ ] Cloud PostgreSQL
* [ ] Frontend deployment
* [ ] End-to-end testing

The immediate next technical milestone is completing the embedding benchmark and then beginning the hybrid RAG layer.

---

# 📁 Project Structure

```text
ai-commerce/
│
├── backend/
│   ├── app/
│   ├── agents/
│   ├── rag/
│   ├── normalization/
│   ├── clustering/
│   ├── payments/
│   └── main.py
│
├── frontend/
│
├── data/
│   ├── raw/
│   └── normalized/
│
├── scripts/
│
├── tests/
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── .env.example
```

---

# ⚙️ Getting Started

## 1. Clone the repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd ai-commerce
```

## 2. Create a virtual environment

```bash
python -m venv venv
```

Activate it:

### Windows

```bash
venv\Scripts\activate
```

### Linux / macOS

```bash
source venv/bin/activate
```

## 3. Install dependencies

```bash
pip install -r requirements.txt
```

## 4. Configure environment variables

Create a `.env` file:

```env
DATABASE_URL=<POSTGRESQL_CONNECTION_STRING>

LLM_API_KEY=<YOUR_LLM_API_KEY>

RAZORPAY_KEY_ID=<YOUR_RAZORPAY_KEY_ID>
RAZORPAY_KEY_SECRET=<YOUR_RAZORPAY_KEY_SECRET>
```

## 5. Start the backend

```bash
uvicorn backend.app.main:app --reload
```

The API will be available locally at:

```text
http://localhost:8000
```

---

# 🧪 Development Philosophy

The project follows a **deployability-first** architecture.

Instead of introducing infrastructure prematurely:

```text
Required
   ↓
Implement
   ↓
Validate
   ↓
Benchmark
   ↓
Scale only when necessary
```

For example, Redis and distributed background workers are intentionally excluded from the MVP until they become necessary.

---

# 🌐 Vision

The long-term vision is to move from:

```text
SEARCH → PRODUCT → CHECKOUT
```

to:

```text
INTENT
   ↓
UNDERSTANDING
   ↓
DISCOVERY
   ↓
COMPARISON
   ↓
PERSONALIZATION
   ↓
PURCHASE
   ↓
FEEDBACK
```

Instead of every merchant operating as an isolated shopping destination, participating merchants can become part of an **AI-native commerce network**.

---

## 👥 Team

Built for the **Razorpay AI Commerce challenge**.

---

## 📜 License

Add your preferred license here.

---

> **AI Commerce Agent — Turning fragmented product catalogues into an intelligent, customer-controlled commerce network.**
