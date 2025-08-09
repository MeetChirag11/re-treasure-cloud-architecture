# ReTreasure — Azure Cloud Architecture (Data Delivery)

## Overview
ReTreasure is an e‑commerce brand that buys and sells antique, vintage, and unique items across marketplaces (eBay, Etsy, Shopify) and in-store. This portfolio project documents our Azure-first data architecture that unifies data from multiple sources, powers analytics, and supports real-time decision-making.

**Mission:** Design and implement a scalable, secure, and data-driven cloud architecture that enables ReTreasure to efficiently manage operations, deliver personalized customer experiences, and make informed decisions through seamless integration of multiple data sources and destinations.

---

## Visual Vision
> A customer-and-operations-centric data platform where **all key data** (customers, orders, store operations, website behavior, geo, and FX rates) flows into a **Lakehouse** and is transformed into **business-ready** insights for fulfillment, finance, marketing, and leadership.

Add your diagram images to the `/diagrams` folder and reference them here:

- ![Cloud Vision](diagrams/cloud-vision.png)
- ![High-Level Architecture](diagrams/high-level-architecture.png)

---

## Proposed Cloud Architecture

**Core services (Azure):**
- **Azure Data Factory (ADF):** Batch ELT pipelines from marketplaces, ERP/POS, and external files.
- **Azure Event Hubs** (or **IoT/Stream source**): Real-time clickstream / events.
- **Azure Data Lake Storage Gen2:** Lakehouse storage for **Bronze / Silver / Gold** layers.
- **Azure Synapse Analytics** (or **Databricks** for transformation): SQL/Notebook transforms, orchestration, and analytics.
- **Power BI:** KPI dashboards and self-serve analytics.
- **Azure Key Vault / RBAC / Purview (optional):** Secrets, access governance, data catalog.

**Lakehouse layers:**
- **Bronze (Raw):** Land raw batch/stream data exactly as received, with source metadata and access controls.
- **Silver (Curated):** Clean, validate schemas, deduplicate, standardize types; apply business rules and QA checks.
- **Gold (Business-Ready):** Aggregate and enrich for analytics/ML—KPIs, dimensional models, feature sets.

**Processing modes:**
- **Batch:** Nightly/hourly loads via ADF.
- **Streaming:** Near‑real‑time events via Event Hubs → stream processing (e.g., Structured Streaming) → Lakehouse.

---

## Proposed Pipeline

1. **Ingestion**
   - **Batch:** ADF copies data from REST APIs (eBay/Etsy/Shopify), POS/ERP exports, CSV/JSON files.
   - **Streaming:** Event Hubs ingests clickstream and transactional events.

2. **Bronze Layer (Raw)**
   - Store immutable raw data with source tags and timestamps.
   - Basic governance (PII mask where required), logging of ingest success/fail.

3. **Silver Layer (Curated)**
   - Schema validation, type fixing, null handling, deduplication.
   - Conform dimensions (Products, Customers, Stores), apply business rules.
   - Data quality checks with pass/fail metrics.

4. **Gold Layer (Business‑Ready)**
   - Build star schemas / marts (Sales, Inventory, Marketing, Finance).
   - Compute KPIs (conversion rate, LTV, inventory turnover, fulfillment rate).
   - Serve to analytics (Power BI) and ML features.

5. **Serving**
   - **Power BI** dashboards for leadership, marketing, operations, and finance.
   - ML pipelines for demand forecasting and personalization.

**Failure Handling (example):**
- Detect failure in any step → log + notify (Teams/Email).
- **Retry up to 3 times** with exponential backoff.
- On persistent failure → quarantine bad records, create incident, continue downstream where safe.
- Backfill mechanism to reprocess historical partitions.

Add your pipeline diagrams to `/diagrams` and reference them:
- ![ETL/ELT Pipeline](diagrams/pipeline.png)
- ![Failure Handling](diagrams/pipeline-failure.png)

---

## Data Sources (Examples)

| Source Type        | Description                                              | Update Freq | Format |
|--------------------|----------------------------------------------------------|-------------|--------|
| Geo‑Location       | Customer locations, delivery zones, regional prefs       | Daily       | JSON   |
| Store (POS/ERP)    | In‑store inventory, POS transactions                     | Hourly      | CSV    |
| Website/App        | Clickstream, sessions, cart events                       | Real‑time   | JSON   |
| Currency Exchange  | FX rates for pricing and reporting                       | Hourly      | XML    |
| Online Transactions| Sales records, payment statuses, order details           | Real‑time   | JSON   |

## Data Destinations (Examples)

| Destination             | Purpose                                   | Key Metrics                      | Integration |
|-------------------------|--------------------------------------------|----------------------------------|-------------|
| Order Fulfillment       | Shipping, tracking, delivery performance   | Delivery time, fulfillment rate  | API         |
| Accounting & Finance    | Financial reports, tax calculations        | Revenue, margins, tax liability  | Scheduled   |
| Advertising & Marketing | Campaign targeting and performance         | Conversion rate, ROI, engagement | API         |
| Business Analytics      | KPI dashboards and insights                | Sales growth, LTV, turnover      | Power BI    |

---

## Conclusion
The ReTreasure platform unifies batch and streaming data into a governed Lakehouse, enabling trusted analytics, operational dashboards, and ML use‑cases. Azure-native services, robust failure handling, and layered transformations ensure performance, reliability, and scale.

---

## Author
- **Meet Chirag** 

---

## How to Use This Repo
- Put your architecture and pipeline images in `/diagrams`.
- Keep the final submission PDF in the repo root (see filename below).
- Link this repository in your submission.

