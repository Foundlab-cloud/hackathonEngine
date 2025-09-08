# 🔥 Burn Engine — About the Project
<a href="https://ibb.co/S4y0W3zB"><img src="https://i.ibb.co/S4y0W3zB/Chat-GPT-Image-8-de-set-de-2025-08-09-34.png" alt="Chat-GPT-Image-8-de-set-de-2025-08-09-34" border="0"></a>
## Inspiration
We were inspired by the gap between how reputation is handled in finance/Web3 and how risk really behaves.  
Today, reputation is a **passive report**: numbers in dashboards or compliance PDFs. Fraud is caught after the money is gone.  
Our frustration came from real-world compliance and DeFi experience: milliseconds matter, but reputation lags.  
**Idea:** flip the paradigm → reputation must become **execution in real time**.

---

## What it does
Burn Engine transforms reputation into **programmable action**:  
- Input: `wallet_id`, `score`, `flags[]`, `tx_value`.  
- Evaluation: rules-as-code check thresholds, blacklists, risk alerts.  
- Output: 🟢 *permit* or 🔴 *deny*, in `<1s`.  
- Every decision is logged with a unique `DecisionID` for **auditability**.  

Mathematically, the decision function is:

$$
f(x) =
\begin{cases}
0 & \text{if } blacklist \in flags \lor fraud\_alert \in flags \\
0 & \text{if } score < \theta \\
0 & \text{if } tx\_value > 10^4 \land score < 70 \\
1 & \text{otherwise}
\end{cases}
$$

Where $f(x)=1$ means **permit**, and $f(x)=0$ means **deny**.

---

## How we built it
- **Core Engine:** Node.js + TypeScript with Fastify for the API.  
- **Rules-as-Code:** simple thresholds + flags in TypeScript module.  
- **Persistence:** Firestore collection `decisions` storing full logs.  
- **Auditability:** each decision generates a UUID `DecisionID`.  
- **UI Simulator:** React app with input form + output “Allowed/Blocked.”  
- **Hosting:** API on Cloud Run, UI on Firebase Hosting.  
- **Observability:** latency measurement and structured logs (pino → Cloud Logging).  

---

## Challenges we faced
- **Time pressure:** building backend + UI + logs in <1 week.  
- **Scope creep:** resisting ML/Guardian AI to keep demo minimal.  
- **Narrative clarity:** reducing “programmable trust” into one line:  
  > “Reputation is not a report. It’s execution.”  
- **Design visibility:** infrastructure is invisible → solved with 🔴/🟢 demo output and badge identity.  

---

## What we learned
- **Simplicity wins in hackathons.** A sharp minimal product beats half-baked complexity.  
- **Explainability is as critical as performance.** Transparent logs gave the project credibility.  
- **Visual storytelling matters.** Even infra must be tangible — a simple red/green UI carried the message better than any diagram.  

---

## Summary
Burn Engine is a **reputation execution engine**:  
a minimal but powerful prototype proving that trust can be enforced as code, in real time, with auditability and irreversible outcomes.  
It is the first step toward building **trust as infrastructure**, as fundamental as DNS is for the internet.


---

## Built with

- **Languages & Frameworks**
  - Node.js 20 + TypeScript  
  - Fastify (API)  
  - React + Vite (UI Simulator)  

- **Cloud Services**
  - Firebase Hosting (UI deploy)  
  - Firebase Firestore (decision logs & audit trail)  
  - Firebase Auth (demo-level authentication)  
  - Google Cloud Run (API containerized)  
  - Google Cloud Logging (structured logs, latency metrics)  

- **Databases & Persistence**
  - Firestore (collection `decisions`)  
  - UUID v4 for `DecisionID`  

- **APIs**
  - RESTful API (`POST /decision`) returning `permit/deny` + `DecisionID`  
  - JSON schema validation for inputs  

- **Other Tools**
  - Docker (build & deploy API)  
  - `pino` (structured logging)  
  - `autocannon` / `hey` (load testing)  


# 🧱 Architecture

```mermaid
flowchart LR
    U[Web Simulator<br/>React+Vite] -- POST /decision --> A(API Fastify<br/>Node.js 20 / TS)
    A -- evaluate() --> R[Rules-as-Code<br/>Thresholds & Flags]
    R -- result --> A
    A -- log --> D[(Firestore<br/>decisions/*)]
    A -- JSON {permit,reason,DecisionID,latency} --> U
    A -. logs .-> L[(Cloud Logging)]


