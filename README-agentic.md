<p align="center">
  <img src="kryptokat-agentic.png" alt="KryptoKat — Tuxedo Hacker Cat at the Terminal" width="420">
</p>

<h1 align="center">🐾 OWASP Top 10 for Agentic Applications — 2026 🐾</h1>
<h3 align="center"><i>KryptoKat's Field Guide to Securing Autonomous AI Agents</i></h3>

<p align="center">
  <a href="https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/"><img src="https://img.shields.io/badge/OWASP-Top%2010%20Agentic%202026-blueviolet?style=for-the-badge" alt="OWASP Agentic Top 10 Badge"></a>
  <a href="https://creativecommons.org/licenses/by-sa/4.0/"><img src="https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey?style=for-the-badge" alt="License"></a>
  <img src="https://img.shields.io/badge/Made%20by-KryptoKat-ff1493?style=for-the-badge" alt="Made by KryptoKat">
</p>

---

## 🐈‍⬛ What is this?

A study guide for the **OWASP Top 10 for Agentic Applications 2026**, released December 10, 2025 by the OWASP GenAI Security Project's Agentic Security Initiative (ASI). Where the [LLM Top 10](https://genai.owasp.org/llm-top-10/) covered single-shot model interactions, this list covers what happens when those models can **plan, persist, delegate, and act** across tools and systems.

> *"LLM security was about preventing bad outputs. Agentic security is about preventing cascading failures across autonomous systems."*

The framework went through more than a year of review with 100+ industry contributors and was evaluated by the ASI Expert Review Board including representatives from NIST, the European Commission, and others.

---

## 📚 Official Sources (Read These First)

| Resource | Link |
|---|---|
| 🏛️ OWASP Top 10 for Agentic Applications 2026 | https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/ |
| 🛡️ Agentic Security Initiative (ASI) | https://genai.owasp.org/initiatives/agentic-security-initiative/ |
| 📄 Agentic AI – Threats and Mitigations | https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/ |
| 🐙 OWASP GenAI Security Project | https://genai.owasp.org |
| 🏴 FinBot CTF (Reference Application) | https://genai.owasp.org/initiatives/agentic-security-initiative/ |
| 🗺️ MITRE ATLAS | https://atlas.mitre.org |

---

## 🎯 The Top 10 at a Glance

| # | Vulnerability | KryptoKat's Translation | Real-World Echo |
|---|---|---|---|
| **ASI01** | [Agent Goal Hijack](#-asi012026-agent-goal-hijack) | Someone whispered the wrong mission to the cat | **EchoLeak** (M365 Copilot) |
| **ASI02** | [Tool Misuse & Exploitation](#-asi022026-tool-misuse--exploitation) | Cat used the laser pointer as a weapon | **Amazon Q** |
| **ASI03** | [Identity & Privilege Abuse](#-asi032026-identity--privilege-abuse) | The cat is wearing someone else's collar | Credential leakage |
| **ASI04** | [Agentic Supply Chain Vulnerabilities](#-asi042026-agentic-supply-chain-vulnerabilities) | Where did this catnip come from? | **GitHub MCP exploit** |
| **ASI05** | [Unexpected Code Execution (RCE)](#-asi052026-unexpected-code-execution-rce) | Cat walked across the production keyboard | **AutoGPT RCE** |
| **ASI06** | [Memory & Context Poisoning](#-asi062026-memory--context-poisoning) | Cat remembers things that never happened | **Gemini Memory Attack** |
| **ASI07** | [Insecure Inter-Agent Communication](#-asi072026-insecure-inter-agent-communication) | Wrong cat got the message | Spoofed agent comms |
| **ASI08** | [Cascading Failures](#-asi082026-cascading-failures) | One cat knocked over all the dominoes | Fan-out failure storms |
| **ASI09** | [Human-Agent Trust Exploitation](#-asi092026-human-agent-trust-exploitation) | The cat learned to lie convincingly | Social-engineered humans |
| **ASI10** | [Rogue Agents](#-asi102026-rogue-agents) | A feral cat is loose in the datacenter | Unsanctioned agents |

---

## 🐱 ASI01:2026 Agent Goal Hijack

> *"You told the agent to summarize emails. The email told the agent to exfiltrate them."*

### Description
Attackers manipulate an agent's goals, plans, or decision pathways through direct or indirect instruction injection — causing the agent to pursue unintended or malicious objectives. Because agents can't reliably distinguish instructions from content, anything the agent *reads* is potentially something it might *follow*.

### Sub-Categories
- **Direct Goal Manipulation** — explicit prompt-based override of objectives
- **Indirect Instruction Injection** — hidden instructions in documents, RAG content, calendar invites, or tool outputs
- **Recursive Hijacking** — goal modifications that propagate through reasoning chains or self-modify over time

### 🐾 Real-World Examples
- **EchoLeak** — A crafted email containing a hidden payload was processed by Microsoft 365 Copilot, silently triggering exfiltration of confidential mail and chat logs. No user click required.
- **Calendar Drift** — A malicious calendar invite contained a "quiet mode" instruction that subtly reweighted the agent's objectives toward low-friction approval.

### Mitigations
- Treat all external content as untrusted input
- Strict input validation and content/instruction segregation
- Operational guardrails and behavior anomaly detection
- Monitor for goal drift across reasoning chains
- Limit agent autonomy on high-impact decisions

### 🧬 Relationship to LLM Top 10
ASI01 combines **LLM01 (Prompt Injection)** with **LLM06 (Excessive Agency)** — but the autonomous multi-step execution amplifies the impact far beyond a single response.

---

## 🐱 ASI02:2026 Tool Misuse & Exploitation

> *"Your agent is allowed to use the tool. The tool didn't sign up for this."*

### Description
The agent operates within its authorized privileges but applies legitimate tools in unsafe or unintended ways — deleting valuable data, over-invoking costly APIs, exfiltrating information, or otherwise weaponizing capability that was granted in good faith.

### Risk Categories
- Destructive tool usage (deleting, overwriting, mass-modifying)
- Resource exhaustion through over-invocation
- Data exfiltration via legitimate channels
- Combining multiple safe tools into an unsafe sequence

### 🐾 Real-World Example
**Amazon Q** — An incident where an agent's legitimate tool usage produced destructive outputs that weren't anticipated by the original tool authors.

### Mitigations
- Principle of least privilege on tool scopes
- Explicit allowlists for high-impact tool actions
- Per-tool rate limiting and quotas
- Behavior monitoring for anomalous tool sequences
- Human-in-the-loop on destructive operations
- Structured/typed tool I/O (no free-text fields that can carry payloads)

### 🧭 Boundary Notes (per OWASP)
- If misuse involves **privilege escalation** → it's **ASI03**
- If misuse results in **arbitrary code execution** → it's **ASI05**
- If the tool definition itself is poisoned (often via MCP) → overlaps with **ASI04**

---

## 🐱 ASI03:2026 Identity & Privilege Abuse

> *"Whose cat are you, anyway?"*

### Description
Agents inherit, steal, escalate, or otherwise abuse identities and privileges to operate beyond their intended scope. This includes leaked credentials, over-broad service identities, confused-deputy patterns, and impersonation across agent boundaries.

### Types of Identity Abuse
- Credential leakage from prompts, memory, or logs
- Over-privileged service accounts running agent workloads
- Confused deputy attacks (agent acts on attacker's behalf with user's authority)
- Identity inheritance across multi-agent chains
- Token replay and session hijacking

### Mitigations
- Per-agent and per-session scoped identities
- Short-lived credentials, OAuth flows with minimum scope
- Strict session management and binding
- Audit trails for every privileged action
- Defense-in-depth: assume the agent's identity will leak

### 🐾 KryptoKat's Note
This is one of the entries that maps poorly to a single LLM Top 10 item — agentic systems introduce identity questions (which agent is acting? on whose behalf? with what scope?) that don't really exist in single-shot LLM apps.

---

## 🐱 ASI04:2026 Agentic Supply Chain Vulnerabilities

> *"You didn't write this MCP server. Are you sure you trust it?"*

### Description
Where the LLM Top 10's supply chain entry focused on **static, pre-deployment** components, ASI04 covers **dynamic, runtime composition** — where agents discover and integrate components (MCP servers, A2A peers, tools, plugins) during execution.

### Attack Vectors
- Compromised MCP servers
- Malicious A2A (agent-to-agent) peers introduced into ecosystems
- Tool definition tampering at runtime
- Poisoned plugin registries
- Typosquatted agent/tool names

### 🐾 Real-World Example
**GitHub MCP Exploit** — A documented attack against the dynamic MCP ecosystem demonstrating how runtime components can be poisoned in ways traditional SBOMs don't catch.

### Mitigations
- Vendor and source vetting for every MCP server / external tool
- Cryptographic signing and verification of tool definitions
- Pinned, versioned MCP/A2A endpoints
- Runtime allowlists for tool discovery
- Continuous monitoring of tool behavior post-deployment
- AI BOMs / ML-SBOMs that capture runtime composition

### 📘 Further Reading
- [Practical Guide for Securely Using Third-Party MCP Servers (OWASP)](https://genai.owasp.org/initiatives/agentic-security-initiative/)
- OWASP CycloneDX for AI BOM tooling

---

## 🐱 ASI05:2026 Unexpected Code Execution (RCE)

> *"Natural language is now a programming language. So is everything else."*

### Description
The agent executes code that wasn't intended by its designers — through code generation tools, dynamic evaluation, injection into executable contexts, or natural-language paths that unlock interpreter access.

### Execution Risks
- `eval()`, `exec()`, dynamic compilation paths exposed to prompt content
- Code-generation tools without sandboxing
- Shell-execution extensions with insufficient command filtering
- Container/notebook environments accessible to the agent
- Indirect injection that reaches code-execution surfaces

### 🐾 Real-World Example
**AutoGPT RCE** — A documented vulnerability where natural-language inputs reached executable contexts in a popular agent framework.

### Mitigations
- **Eliminate code execution entirely** if not required (architectural prevention)
- Sandboxed execution environments (gVisor, Firecracker, restricted containers)
- Human-in-the-loop approval before code runs
- Strict output filtering between LLM responses and code interpreters
- Network egress restrictions on execution environments
- Read-only filesystem mounts where possible

### 🧬 Relationship to LLM Top 10
This is **LLM05 (Improper Output Handling)** weaponized by autonomy — where LLM05 was about output reaching a downstream system, ASI05 is about output reaching an interpreter, *executing*, and *then* reaching downstream systems.

---

## 🐱 ASI06:2026 Memory & Context Poisoning

> *"The cat now believes things that aren't true. Permanently."*

### Description
Persistent corruption of agent memory, RAG stores, embeddings, or contextual knowledge — reshaping behavior long after the initial interaction. Unlike single-shot prompt injection, memory poisoning has a half-life.

### Types of Memory Attacks
- Long-term memory poisoning (the agent "remembers" attacker-supplied facts)
- RAG store contamination
- Embedding-space attacks
- Conversation context manipulation across sessions
- Cross-user memory bleed in shared stores

### 🐾 Real-World Example
**Gemini Memory Attack** — A demonstrated attack where injected content persisted in the agent's memory and altered behavior across subsequent sessions.

### Mitigations
- Validate and classify content **before** memory storage
- Permission-aware vector and memory stores
- Memory expiration / TTL policies
- Provenance tracking on stored facts
- Periodic memory audits for poisoned content
- User-controlled memory inspection and deletion
- Tag and classify data within knowledge bases

### 🧬 Relationship to LLM Top 10
This is the agentic evolution of **LLM04 (Data and Model Poisoning)** + **LLM08 (Vector and Embedding Weaknesses)** — but with the added dimension of *persistent, runtime-acquired* poisoning.

---

## 🐱 ASI07:2026 Insecure Inter-Agent Communication

> *"Cat A told Cat B something. Was it really Cat A?"*

### Description
When multiple agents communicate, their messages can be intercepted, spoofed, or manipulated if channels lack authentication, encryption, or message integrity. Spoofed inter-agent messages can misdirect entire clusters.

### Communication Vulnerabilities
- Unauthenticated agent-to-agent (A2A) messaging
- Lack of message integrity verification
- Plaintext communication exposing prompts/data
- Replay attacks on agent messages
- Forged sender identities
- No message provenance tracking

### Mitigations
- Mutual authentication between agents (mTLS, signed tokens)
- Cryptographic message signing
- Encrypted channels for all A2A traffic
- Message sequencing and replay protection
- Sender provenance attached to every message
- Network segmentation between agent tiers

### 🐾 KryptoKat's Note
This is an *entirely new* vulnerability class — there's no LLM Top 10 equivalent because single-shot LLMs don't have peers. Single-agent architectures sidestep this category entirely; choose them deliberately when the use case allows.

---

## 🐱 ASI08:2026 Cascading Failures

> *"One bad decision. A thousand downstream agents executing it in parallel."*

### Description
A failure in one component (an LLM provider, a downstream API, a tool, or another agent) propagates through the agent system, causing widespread outages or degraded behavior. ASI08 is specifically about **propagation and amplification** — the originating defect may belong to ASI04, ASI06, or ASI07; ASI08 is what happens when it spreads.

### Failure Propagation Patterns
- **Rapid fan-out** — one faulty decision triggers many downstream agents
- **Cross-domain spread** — failure crosses tenant or context boundaries
- **Oscillating retries** — feedback loops between agents
- **Queue storms** — repeated identical intents flooding queues
- Cross-agent error amplification

### Mitigations
- Circuit breakers between agent tiers
- Bulkheads isolating agent populations
- Rate limiting on inter-agent calls
- Anomaly detection on fan-out patterns
- Graceful degradation strategies
- Chaos engineering for agent systems
- Comprehensive distributed tracing (OpenTelemetry)

### 🐾 KryptoKat's Note
This entry is **diagnostic** in nature — when responding to an incident, the original defect is logged under ASI04/06/07 (its source), and ASI08 is applied when that defect demonstrably spread across agents, sessions, or workflows.

---

## 🐱 ASI09:2026 Human-Agent Trust Exploitation

> *"The cat learned to be charming. The cat is not, in fact, charming."*

### Description
Attackers exploit the trust humans place in agent outputs — leveraging the perceived authority, fluency, or autonomy of agents to manipulate human decision-making. The threat actor here is sometimes the agent itself (when hijacked), and sometimes a third party using the agent as a credibility laundering tool.

### Trust Exploitation Methods
- Confident-sounding misinformation
- Impersonation of trusted agents/personas
- Manipulation of human-in-the-loop approvals through framing
- Habituation attacks (training users to rubber-stamp)
- Authority laundering via agent-mediated communication

### Mitigations
- Clear UI labeling of agent-generated content
- Provenance and confidence indicators on agent outputs
- Friction on high-impact human approvals (no rubber-stamping)
- User education on agent limitations and failure modes
- Avoid anthropomorphic UX patterns that overstate agent reliability
- Independent verification paths for critical decisions

### 🧬 Relationship to LLM Top 10
This extends **LLM09 (Misinformation)** + **LLM06 (Excessive Agency)** with explicit attention to the *human* in the loop as part of the attack surface.

---

## 🐱 ASI10:2026 Rogue Agents

> *"There is a cat in the building. Nobody knows whose cat it is."*

### Description
Unauthorized, unsanctioned, or maliciously deployed agents operating in an environment — including shadow IT agents, agents that escape their intended scope, agents with subtly drifted goals, and adversary-deployed agents in compromised environments.

### Rogue Agent Behaviors
- Shadow agents deployed by users without security review
- Authorized agents that drift outside their original purpose
- Adversary-deployed agents in compromised environments
- Forgotten agents continuing to operate after their use case ended
- Agents that self-modify or acquire new capabilities post-deployment

### Mitigations
- **Discovery first** — automated agent inventory across SaaS, cloud, and dev pipelines
- Centralized agent registry with mandatory enrollment
- Behavioral baselines for every authorized agent
- Egress monitoring for unidentified agent traffic
- Decommissioning processes for retired agents
- RBAC and the principle of least privilege on agent deployment
- Continuous monitoring for capability drift

### 🐾 KryptoKat's Note
This is the agentic equivalent of asset management — *you can't secure what you don't know exists*. Tooling like Palo Alto's Prisma AIRS, MS Purview, and others are emerging specifically to discover undocumented agents.

---

## 🧠 Cross-Cutting Themes

Threads that run through multiple entries:

| Theme | Affected Entries |
|---|---|
| **Identity is the new perimeter** | ASI03, ASI07, ASI10 |
| **Memory is durable; treat it like state** | ASI01, ASI06 |
| **MCP/A2A ecosystems need supply-chain discipline** | ASI02, ASI04, ASI07 |
| **Autonomy amplifies every other vulnerability** | ASI01, ASI05, ASI08 |
| **Observability is non-optional** | ASI01, ASI02, ASI08, ASI10 |
| **Defense-in-depth, no single control trusted** | All of them |

---

## 🆚 ASI Top 10 vs. LLM Top 10

The two lists are **complementary, not redundant**:

| LLM Top 10 (single-shot) | Agentic Top 10 (autonomous systems) |
|---|---|
| Prevents bad outputs | Prevents cascading failures |
| Static threat surface | Runtime, dynamic threat surface |
| Single-model interactions | Multi-step, multi-tool, multi-agent |
| LLM03 — static supply chain | ASI04 — runtime supply chain |
| LLM04 — training-time poisoning | ASI06 — runtime memory poisoning |
| LLM06 — excessive agency (warning) | ASI01–ASI10 — what excessive agency *enables* |
| No equivalent | ASI07 — inter-agent comms |
| No equivalent | ASI08 — cascading failures |
| No equivalent | ASI10 — rogue agents |

**Use both.** The LLM Top 10 covers model-level vulnerabilities; the Agentic Top 10 covers system-level risks that emerge from autonomy.

---

## 🐾 Defender's Quick-Reference Cheat Sheet

| If you're building... | Pay extra attention to |
|---|---|
| A single-agent LLM assistant with tools | ASI01, ASI02, ASI05 |
| A multi-agent orchestration system | ASI07, ASI08 (above all) |
| An agent integrating MCP servers | ASI04, ASI02, ASI06 |
| An agent with persistent memory / RAG | ASI06, ASI01 |
| A code-generating or auto-executing agent | ASI05, ASI02 |
| An agent that acts on behalf of users | ASI03, ASI09 |
| An enterprise rolling out agents broadly | ASI10 (start here — discover first) |
| A FinBot, HealthBot, or any high-stakes domain | All of them, then ASI09 again |

---

## 🔗 Further Reading & Related Frameworks

- 📄 [OWASP Top 10 for Agentic Applications 2026 (full PDF)](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)
- 🛡️ [OWASP Agentic Security Initiative](https://genai.owasp.org/initiatives/agentic-security-initiative/)
- 📘 [Agentic AI Threats and Mitigations Taxonomy v1.1](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
- 🏴 [FinBot — Agentic Security CTF Reference App](https://genai.owasp.org/initiatives/agentic-security-initiative/)
- 🔄 [OWASP LLM Top 10 (2025)](https://genai.owasp.org/llm-top-10/) — *the prerequisite reading*
- 🗺️ [MITRE ATLAS](https://atlas.mitre.org)
- 🛡️ [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- 🌐 [OWASP AI Exchange](https://owaspai.org) — flagship project, broader AI security
- 📊 [Microsoft Agentic Failure Modes](https://www.microsoft.com/) (references OWASP Threats & Mitigations)
- 🛡️ [NVIDIA Safety and Security Framework for Real-World Agentic Systems](https://www.nvidia.com/) (references the OWASP Threat Modelling Guide)

---

## 📝 License & Attribution

This study guide summarizes and paraphrases content from the **OWASP Top 10 for Agentic Applications 2026**, which is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). This guide is shared under the same license.

**Project Leadership:**
- **Steve Wilson** — OWASP GenAI Security Project Board Co-Chair
- **Scott Clinton** — OWASP GenAI Security Project Co-Chair
- **Keren Katz** — Co-Lead, OWASP Top 10 for Agentic AI Applications

Released **December 10, 2025** following 12+ months of community review with 100+ contributors across NIST, the European Commission, and major industry stakeholders.

The hacker cat is just here for moral support. 🐈‍⬛

---

<p align="center">
  <i>Plan carefully. Persist intentionally. Delegate paranoidly.</i><br>
  <b>— ハッカー猫 / KryptoKat</b>
</p>
