# 🦅 Raptor (formerly Aperture) — Project Overview & Onboarding

> 📌 **Quick Links:** [The Pitch](https://sailpoint.atlassian.net/wiki/spaces/data/pages/3925377407) | [Roadmap](https://sailpoint.atlassian.net/wiki/spaces/data/pages/3995893942) | [POC Architecture](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4236542171) | [POC to MVP](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4579557430) | [Use Cases](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4161470781) | [DPINTAKE-33](https://sailpoint.atlassian.net/browse/DPINTAKE-33)

---

## 🎯 What is Raptor?

Raptor (formerly known as **Aperture**) is a **single external API** powered by **federated GraphQL** (Apollo Router + Federation 2) that replaces heavy, pre-materialized documents with **on-demand data composition** from fit-for-purpose backends.

> 💡 **Think of it like a data menu:** you pick what you want, and Raptor assembles it fresh on the spot — no pre-cooking every item on the menu just in case.

Instead of pre-building massive search documents through expensive materializers, data is stored once and composed dynamically at query time through a unified GraphQL API.

---

## 🔥 Problem Statement

The current architecture forces everything through search via streaming & batch materialization:

| ⚠️ Problem | 💥 Impact |
|------------|----------|
| 💰 **Unsustainable cost curve** | Re-materialization multiplies compute cost linearly with data volume |
| 🕸️ **Growing complexity** | Each feature adds bespoke pipelines, schema joins, and edge-case handling |
| 🚧 **Platform bottleneck** | Data platform is the critical path for too many teams |
| ⏰ **Fragile freshness** | "Real-time" means frequent batch runs and risk-prone replays |
| 💣 **High blast radius** | Minor updates cascade through millions of rows and multiple indices |

---

## 🏗️ Architecture: 5 Data Lanes

| 🏷️ Lane | 🔧 Backend | 📋 Purpose | 🎯 SLO Target |
|---------|-----------|-----------|---------------|
| ⚡ **Pointed Lookups** | ScyllaDB / Aerospike | Entity hydration (identity, entitlement, role) | p95 ≤ 10 ms |
| 🔗 **Graph Database** | JanusGraph on Scylla | Relationship traversals (effective access, policy eval) | p95 < 1 s (N-hop) |
| 📊 **Real-Time Aggregates** | ClickHouse | Dashboards, anomaly detection, usage metrics | p95 < 300 ms |
| 🔍 **Search & Discovery** | OpenSearch / Elasticsearch | Full-text search, autocomplete | p95 < 200 ms |
| 📚 **Historical & Batch** | Snowflake | Large-scale reporting, compliance, ML training | Hours-level freshness |

> 🌐 A unified **GraphQL gateway** (Apollo Router) federates subgraphs for each lane into a **single external schema**.

---

## 🧭 Core Principles

- 🎯 **Right data, right store** — Each access pattern belongs in the backend designed for it.
- 🚪 **One API, many backends** — A single external API presents data consistently while teams own their federated subgraphs.
- 🌊 **Streaming first** — Data is cleaned, deduped, and hydrated once, then fanned out everywhere needed.
- 🏢 **Tenant-first design** — Every layer enforces isolation, limits, and observability per org or pod.
- 🚀 **Progress over perfection** — Start with POCs and vertical slices, then harden iteratively.
- 📡 **Serve in place when possible** — If a domain API already runs on a scalable backend and can meet SLOs, expose a GraphQL subgraph directly.

---

## 🗺️ Roadmap Phases

| 📍 Phase | 🎯 Objective | ✅ Outcome |
|----------|-------------|------------|
| 🟢 **Phase I – Prove It Works** *(Current)* | Validate architecture through fast, focused POCs across all four DACIs | Technical validation, cost benchmarks, initial governance docs |
| 🟡 **Phase II – Golden Paths** | Convert POC results into production-ready reference implementations | One golden path per lane |
| 🔵 **Phase III – Self-Service & Adoption** | Enable broader adoption through templates, automation, and onboarding | Early product teams onboard independently |
| 🟣 **Phase IV – Scale & Hardening** | Mature the platform for external APIs, FedRAMP, and high-scale operations | Reliable, governed, self-service platform |

---

## 📋 DACI Decision Documents

| 📄 DACI | 🎯 Focus | 📊 Status | 🔗 Link |
|---------|---------|----------|---------|
| 🌐 **GraphQL Layer** | Unified API, federation, schema governance | 📝 Draft | [DACI — GraphQL Layer](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4105700011) |
| ⚡ **Pointed Lookups** | Low-latency KV store for entity hydration | 🔄 In Progress | [DACI — Low-Latency Layer](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4082827362) |
| 🔗 **Graph Database** | Relationship modeling and traversals | 🔄 In Progress | [DACI — Graph Database Lane](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4106027902) |
| 📊 **Real-Time Aggregates** | Sub-second analytics, dashboards, metrics | 📝 Draft | [DACI — Real-Time Analytics Layer](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4106060635) |
| 🔍 **Search Layer** | Search 2.0 strategy (Aperture Search vs materializers) | 📝 Draft | [DACI — Search Layer](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4139516133) |

---

## 📚 Key Confluence Documentation

| 📄 Page | 📋 Description |
|---------|---------------|
| 📣 [The Pitch — Aperture... One Door to Data at Scale](https://sailpoint.atlassian.net/wiki/spaces/data/pages/3925377407) | Executive vision, architecture overview, and thought experiments |
| 🗺️ [Aperture Roadmap: Plan of Action](https://sailpoint.atlassian.net/wiki/spaces/data/pages/3995893942) | 4-phase delivery roadmap with action tracking |
| 🏗️ [Aperture POC - Federated GraphQL Architecture](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4236542171) | Detailed POC docs, query patterns, federation concepts |
| 💪 [Aperture: Turning Our Data Into a Product Superpower](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4273045533) | Product-facing pitch for stakeholders |
| 🚀 [Aperture - POC to MVP](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4579557430) | All repos, services, environments, and credentials |
| 🎯 [Aperture Initial Problems to Solve](https://sailpoint.atlassian.net/wiki/spaces/~557058a92a897c42824a4792963165ed4eea38/pages/4526440860) | Focus on search materialization as acute pain point |
| 📋 [Use Case Collection Page](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4161470781) | Collected use cases from across the organization |
| 🔍 [Search Design Discussion](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4139516173) | Coupling options between search and core graph |
| 🆘 [How Aperture Handles Missing Data](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4273078293) | Missing data and new requirements handling |
| 📊 [Aperture Strategy](https://sailpoint.atlassian.net/wiki/spaces/data/pages/3675947694) | Broader strategy page |
| 📅 [Data Platform Q1 2026 Commits](https://sailpoint.atlassian.net/wiki/spaces/data/pages/4482204179) | Aperture as high-priority Q1 initiative |

---

## 🎫 Jira Epics & Key Tickets

### 🏔️ Epics (SAASD8NG Project)

| 🔑 Key | 📋 Summary | 📊 Status |
|--------|-----------|----------|
| 🌐 [SAASD8NG-4502](https://sailpoint.atlassian.net/browse/SAASD8NG-4502) | Apollo Server GraphQL | — |
| ⚡ [SAASD8NG-4501](https://sailpoint.atlassian.net/browse/SAASD8NG-4501) | Pointed Lookup Pipeline (ScyllaDB) | — |
| 🔗 [SAASD8NG-4273](https://sailpoint.atlassian.net/browse/SAASD8NG-4273) | Graph Database POC (JanusGraph) | — |
| 📊 [SAASD8NG-4972](https://sailpoint.atlassian.net/browse/SAASD8NG-4972) | Aperture Real-Time Analytics POC | 📋 Backlog |
| ⚡ [SAASD8NG-5554](https://sailpoint.atlassian.net/browse/SAASD8NG-5554) | Pointed Lookups on Aerospike | 📋 Backlog |
| 🚀 [SAASD8NG-4500](https://sailpoint.atlassian.net/browse/SAASD8NG-4500) | Mach5 | — |

### 📝 Active & Backlog Stories

| 🔑 Key | 📋 Summary | 📊 Status | 👤 Assignee |
|--------|-----------|----------|------------|
| 🔥 [DPINTAKE-33](https://sailpoint.atlassian.net/browse/DPINTAKE-33) | Project Aperture Delivery for Identity Graph | ✅ **Open (In Progress)** | 👤 Dattu Marneni |
| 🔍 [SAASD8NG-5982](https://sailpoint.atlassian.net/browse/SAASD8NG-5982) | Create subgraph for Aperture-Search (ES) | 📋 Backlog | ❌ Unassigned |
| 🔗 [SAASD8NG-5981](https://sailpoint.atlassian.net/browse/SAASD8NG-5981) | Create subgraph for Aperture-Graph (JanusGraph) | 📋 Backlog | ❌ Unassigned |
| ⚡ [SAASD8NG-5980](https://sailpoint.atlassian.net/browse/SAASD8NG-5980) | Create subgraph for Aperture-Core (ScyllaDB) | 📋 Backlog | ❌ Unassigned |
| 🔐 [DPE-179](https://sailpoint.atlassian.net/browse/DPE-179) | Service User Creation for aperture-core | 📋 Backlog | 👤 Ashish Pandita |
| 🔐 [DPE-165](https://sailpoint.atlassian.net/browse/DPE-165) | Service User Creation for aperture-graph | 📋 Backlog | 👤 Subham Jain |
| 🗑️ [DPE-128](https://sailpoint.atlassian.net/browse/DPE-128) | Deprecate deploy_svc_user from aperture-graph | 📋 Backlog | 👤 Subham Jain |
| 📢 [INIT-2073](https://sailpoint.atlassian.net/browse/INIT-2073) | Aperture INIT (business communication) | 🔄 In Progress | — |

---

## 💻 GitHub Repositories

| 📂 Repo | 🎯 Purpose |
|---------|----------|
| 🟢 [sailpoint-core/aperture-core](https://github.com/sailpoint-core/aperture-core) | Core service — ScyllaDB pointed lookups |
| 🔵 [sailpoint-core/aperture-graph](https://github.com/sailpoint-core/aperture-graph) | Graph service — JanusGraph traversals |
| 🟠 [sailpoint-core/aperture-search](https://github.com/sailpoint-core/aperture-search) | Search service — Elasticsearch |
| 🟡 [sailpoint-core/pointed-lookups-dip](https://github.com/sailpoint-core/pointed-lookups-dip) | DIP for pointed lookups pipeline |
| 🟣 [sailpoint-core/entity-mach5-dip](https://github.com/sailpoint-core/entity-mach5-dip) | Mach5 entity DIP |
| ⚙️ [sailpoint/gitops-k8s (apollo-poc branch)](https://github.com/sailpoint/gitops-k8s/tree/apollo-poc/devops/aperture-services) | Apollo Server deployment configs |

---

## 🖥️ Dev Environment & Infrastructure

### 🌐 Endpoints & UIs

| 🏷️ Resource | 🔗 URL / Details |
|------------|-----------------|
| 🚀 Apollo Server (Dev) | `http://k8s-apolloro-devuseas-ed9340708e-f66733edb3618247.elb.us-east-1.amazonaws.com/` |
| 🔄 [Apollo Server — ArgoCD](https://argocd.ops-dev-use1.cloud.sailpoint.com/applications/argocd/dev-us-east-1-poc-apollo-test-odin-use1-2) | ArgoCD deployment dashboard |
| 🗄️ [ScyllaDB Cloud UI](https://auth.cloud.scylladb.com/oauth/account/login) | ScyllaDB management console |
| 🖥️ ScyllaDB Dev Node 1 | `node-0.aws-us-east-1.51de1b47c6b92de7329d.clusters.scylla.cloud` |
| 🖥️ ScyllaDB Dev Node 2 | `node-1.aws-us-east-1.51de1b47c6b92de7329d.clusters.scylla.cloud` |
| 🖥️ ScyllaDB Dev Node 3 | `node-2.aws-us-east-1.51de1b47c6b92de7329d.clusters.scylla.cloud` |
| 🚀 [Mach5 UI (Dev)](https://mach5-m5s.odin-use1-2.ida.cloud.sailpoint.com/) | Mach5 management interface |
| 📊 [Mach5 Grafana Dashboard](https://sailpoint.grafana.net/d/mach5-health-v12/mach5-health-metrics-v12-latest) | Health metrics monitoring |
| 📈 [Entity Metrics Dashboard](https://sailpoint.grafana.net/d/fec0lh91qimf4d-mach5/entity-metrics-service) | Entity metrics service |
| 📖 [Mach5 Docs](https://mach5.io/docs/) | Official documentation |

### 🔐 AWS Secrets Manager

| 🔑 Secret | 📍 Path |
|-----------|--------|
| 🗄️ ScyllaDB baseline credentials | `dataplatform/scylladb/base/credentials` |
| 🗄️ ScyllaDB dev ingest credentials | `dataplatform/scylla/dev/credentials` |
| 🗄️ ScyllaDB Aperture Service read credentials | `dataplatform/aperture/scylladb/credentials` |
| 🗄️ ScyllaDB local image credentials | `dataplatform/scylladb/local/credentials` |
| 🚀 Mach5 UI credentials | `mach5/dev_odin-use1-2/credentials/api` |

---

## 👥 Key People & Stakeholders

| 👤 Person | 🏷️ Role |
|-----------|--------|
| ⚠️ **Derrick Mink** | Original driver (**departed** — ownership gap) |
| ✅ **Dan Sparks** | Approver |
| ✅ **Fuad Rashid** | Approver |
| 🔧 **Chris Schneider** | Resource coordination, Aerospike epic owner |
| 💡 **Kelly Grizzle** | Contributor (Mach5 use cases) |
| 📊 **Jordan Mandernach** | Tracking / centralized POC view |
| ✅ **Remi Philippe** | Approver (Graph Database DACI) |
| 🔧 **Rob Tappenden** | Contributor (Graph Database) |
| 🔧 **Alex Derzhi** | Contributor |
| 🔧 **Charles Mims** | Contributor |
| 📢 **Tricia Kaplan** | INIT communication |
| 📢 **Kevin Killens** | INIT communication |
| 🤝 **evolv Consulting** | Engaged for POC-to-MVP work |

---

## 📈 Target Outcomes & SLOs

| 📏 Metric | 🎯 Target |
|-----------|---------|
| 🚀 New entity surfaced via GraphQL | < 2 days from schema addition |
| 🗑️ Materializer reduction | ≥ 3 retired by mid-2026 |
| 💰 Backend cost reduction | 30–50% vs. Snowflake baseline |
| 😊 Developer satisfaction (onboarding/docs) | ≥ 8 / 10 |
| ✅ POC → Golden Path conversion rate | ≥ 75% of successful POCs proceed to Phase II |

---

## ⚡ Immediate Next Steps (Raptor Transition)

| # | 📋 Action | 📝 Details |
|---|----------|-----------|
| 1️⃣ | 🔍 **Assess current POC state** | Review what is deployed and running in dev environments (Apollo Server, ScyllaDB, JanusGraph) |
| 2️⃣ | 👑 **Clarify ownership** | With Derrick Mink's departure, establish new drivers for each DACI lane |
| 3️⃣ | 🎯 **Address [DPINTAKE-33](https://sailpoint.atlassian.net/browse/DPINTAKE-33)** | Provide a reasonable delivery date and incremental delivery plan for the Identity Graph use case |
| 4️⃣ | 🔓 **Unblock backlog** | Assign and prioritize subgraph stories ([SAASD8NG-5980](https://sailpoint.atlassian.net/browse/SAASD8NG-5980) / [5981](https://sailpoint.atlassian.net/browse/SAASD8NG-5981) / [5982](https://sailpoint.atlassian.net/browse/SAASD8NG-5982)) and service user creation |
| 5️⃣ | 🦅 **Rename to Raptor** | Update Confluence pages, Jira labels/epics, and Slack channels |
| 6️⃣ | 📅 **Establish cadence** | Start the weekly "Raptor Updates" post (action item #10 from roadmap) |
| 7️⃣ | 🤝 **Schedule kickoff sync** | Align with Forge, Keel, DIPO, and Double Zero leads |

---

> 📝 *This page consolidates information gathered from the Data Platform Confluence space, Jira boards, and related documentation.*
>
> 📅 *Last updated: March 2026*
>
> 👤 *Maintained by: Dattu Marneni*
