### PakFreight Assignment Summary (Tasks 1–5)

---

#### **Task 1 — The Three Tiers & Warehouse Foundations**

* **Three Tiers:**
* **Staging (Tier 1):** Raw landing zone & quality check (no cross-source joins or reporting).
* **Storage / EDW (Tier 2):** Integrated, historical data persistence (no direct user queries).
* **Presentation / Marts (Tier 3):** Dimensional star schemas optimized for BI.


* **Inmon Properties:** Subject-oriented, integrated, time-variant, and non-volatile. Overwriting customer addresses breaks *non-volatile/time-variant* rules; use **SCD Type 2** instead.
* **EDW vs. ODS:** Dispatchers/agents need **ODS** (real-time ops); CFO/Board need **EDW** (historical analysis).
* **Independent Marts:** Lacking a central EDW causes departmental silos and conflicting numbers.

#### **Task 2 — Inmon, Kimball, & the Lifecycle**

* **Comparison:** Inmon is top-down (3NF, enterprise consistency, slow start); Kimball is bottom-up (dimensional, fast value, conformed dimensions).
* **CLDS vs. SDLC:** Warehouses use Data-Driven Life Cycles (`Data $\rightarrow$ Requirements`) because raw data anomalies dictate final schema rules.

#### **Task 3 — Data Detective & The Stovepipe Game**

* **Anomalies Found:** Unread headers, mixed units (KG vs. Tons), mismatched date locales, whitespace, duplicate rows, blank rows, and a summary `TOTAL` row.
* **The Stovepipe Game:**
* Operations included the `TOTAL` row ($661.65\text{ t}$).
* Finance read rail tons as kilograms ($283.70\text{ t}$).
* Sales kept duplicate rows ($584.75\text{ t}$).
* **True Reconciled Total:** **578,650 kg** across 34 rows.


* **Pipeline Control:** Add an automated Power Query row filter to drop any row containing summary strings like `"TOTAL"`.

#### **Task 4 — Power BI Pipeline Hands-On**

* **Execution:** Raw files were connected and transformed via Power Query (`_stg` queries), standardized (dates, `RL-` IDs, UPPERCASE statuses, rail tons $\times 1,000$), cleansed of bad rows, and appended into a single trusted **`Shipments`** table.
* **Validation:** Exactly 34 rows, 8 columns, **578,650 kg** total weight, and 3 distinct statuses. Staging queries had "Enable Load" disabled.

#### **Task 5 — Latency, Drift, Lineage, & Roles**

* **Latency:** Match freshness to user needs (dispatchers need real-time; CFO needs monthly batches). "Always real-time" is inefficient and costly.
* **Schema Drift:** Added columns pass through safely; renamed columns break downstream queries and require ETL contract alerts.
* **Lineage & Troubleshooting:** Unbroken audit trail: Source $\rightarrow$ Staging $\rightarrow$ Storage $\rightarrow$ BI. If totals look low, check road filters, rail unit conversion, and marine row counts in order.
* **Ownership:** Clear ownership across Staging, Storage, and Presentation prevents departmental silos and conflicting data.
