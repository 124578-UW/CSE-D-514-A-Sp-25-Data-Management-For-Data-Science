# Aadhaar: The Unique Identity Index of India

This repo contains my DATA 514 project on **Aadhaar**, India’s national identification system and the world’s largest biometric-based identity platform. Aadhaar assigns a random 12-digit number to each resident and links it to demographic and biometric data at national scale. :contentReference[oaicite:0]{index=0}  

My goal is to explore the **data architecture, governance, and query workloads** behind Aadhaar and think about what it takes to support ~1.4B identities and petabytes of sensitive data securely and efficiently.

---

## Project Overview

In this project I:

- Describe what Aadhaar is and why it matters for identity, inclusion, and national-scale data systems.
- Design a **relational schema and ER diagram** for the core entities:
  - `USERS` (demographics)
  - `AADHAAR` (identifier mapping)
  - `BIOMETRICS` (fingerprints, iris, face)
  - `ENROLL_CENTER` / `ENROLL_LOGS`
  - `REGISTER` (benefit registrations)
  - `VERI_LOGS` (verification events)
- Discuss **data governance** requirements:
  - Legal frameworks (Aadhaar Act, data protection rules)
  - Data minimization, virtual IDs, RBAC
  - Secure biometric storage in a central repository (CIDR)
- Propose **system and storage choices**:
  - Oracle (or similar) for relational data
  - HDFS for large biometric objects
  - Search index (e.g., Solr) for fast lookup
  - Private cloud (e.g., OpenStack) for deployment

The slides in this repo walk through the ER diagram (page 6), normalized schema (page 7 & 12), and architecture recommendation (page 13). :contentReference[oaicite:1]{index=1}  

---

## Key Use Cases

I model several representative workloads:

1. **Social benefit register reporting**  
   - Monthly aggregate counts of benefit registrations by state and benefit type.  
   - Emphasizes group-by queries over large fact tables.

2. **Authentication / investigation query**  
   - Given a phone number, look up the Aadhaar number(s) tied to the current registered owner(s).  
   - Highlights indexing, selective predicates, and join strategies at massive scale.

3. **Enrollment duplication analysis**  
   - Monthly summary of duplicate enrollments by enrollment center.  
   - Demonstrates range queries on timestamps and filtering by enrollment result.

For each use case, I outline sample SQL and logical plans, including selectivity assumptions and why certain indexes/partitions are chosen.

---

## What’s in This Repo

- `Presentation-DATA514.pdf` – Slide deck with:
  - Conceptual overview of Aadhaar  
  - Data description and ER diagram  
  - Logical schema and normalization choices  
  - Example queries and logical plans  
  - System and architecture recommendations
- (Optional / Planned)
  - `schema.sql` – DDL for the core tables
  - `queries.sql` – Example queries for the use cases
  - `README.md` – This file

---

## How This Relates to DATA 514

This project ties course concepts to a real, high-stakes system:

- **Data modeling & normalization** for a national ID database
- **Indexing and query optimization** for billion-row tables
- **Partitioning** and storage layout for scalability
- **Transactionality and governance** where PII and biometrics are involved
- **System design** tradeoffs between relational DBs, distributed storage, and search systems

It is less about reproducing Aadhaar exactly and more about reasoning through a plausible technical blueprint for a system of this scale and sensitivity.

---

## Future Directions

Possible extensions:

- Formal performance estimates for each use case (I/O cost models, index selectivity).
- Deeper treatment of privacy-preserving techniques and threat models.
- Simulation or benchmarking on synthetic data to test schema and index design.
