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

A practitioner reference for the **OWASP Top 10 for Agentic Applications 2026**, released December 10, 2025 by the OWASP GenAI Security Project's Agentic Security Initiative (ASI). Where the [LLM Top 10](https://genai.owasp.org/llm-top-10/) covered single-shot model interactions, this list covers what happens when those models can **plan, persist, delegate, and act** across tools and systems.

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
Agent Goal Hijack is the manipulation of an agent's objectives, plans, or decision pathways such that the agent pursues outcomes the attacker chose rather than the ones its principal authorized. The attack vector is direct or indirect instruction injection: anything the agent reads — user input, retrieved documents, tool outputs, web pages, calendar entries, peer-agent messages — can contain embedded instructions that the agent treats as if they came from its operator. Because LLM-based agents do not reliably distinguish instructions from content, this is a structural weakness rather than a tunable parameter; mitigations reduce frequency and impact but do not eliminate the class. Goal hijack is significantly more dangerous than single-shot prompt injection in chat interfaces because agents execute multi-step plans across tools and time. A successful injection at step 1 propagates through the agent's reasoning, tool selection, and downstream actions — turning a single moment of attacker control into a sustained operation. Recursive hijacking, where the modified goal causes the agent to further modify its own objectives, makes this even harder to contain.

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
Tool Misuse and Exploitation occurs when an agent uses its legitimately authorized tools in ways that produce unsafe or unintended outcomes — without escalating privileges, executing arbitrary code, or breaking out of its scope. The agent has permission to do what it does; the problem is that what it does is harmful. This includes destructive operations (deleting valuable data, overwriting files, mass-modifying records), economic harm (over-invoking costly APIs, draining cloud budgets), data exfiltration through legitimate channels (the agent emails, posts, or uploads attacker-influenced content using authorized tools), and unsafe tool composition (chaining benign tools into a sequence that no individual tool author anticipated). The core difficulty is that the agent's behavior may be technically authorized at every step — making this hard to distinguish from valid usage by signature alone. Defense requires structural constraints on what tools can do and behavioral monitoring on how they are used in combination.

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
Identity and Privilege Abuse is the class of vulnerabilities where agents inherit, steal, escalate, or otherwise abuse identities and access permissions to operate beyond their intended scope. Agentic systems introduce identity questions that don't exist in traditional applications: which agent is acting right now, on whose behalf, with what authority, and how is that authority verified at each step of a multi-step plan? In practice, these questions often have unsatisfying answers — agents are deployed with broad service-account identities, credentials leak through prompts and memory, multi-agent chains inherit privileges across boundaries that were never explicitly authorized, and confused-deputy patterns let an agent act with a user's full authority on behalf of an attacker who manipulated its inputs. Token replay and session hijacking become especially damaging when a single token grants access to an agent that can autonomously chain dozens of operations. The defensive posture must assume that the agent's identity will leak and that downstream systems must enforce authorization themselves rather than trusting the agent.

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
Agentic Supply Chain Vulnerabilities cover the security risks introduced when agents discover, evaluate, and integrate components during runtime rather than at build time. The traditional software supply chain (and the LLM Top 10's static supply chain entry) focused on what is bundled into a deployment artifact before it ships. Agentic systems increasingly operate against a different model: the agent connects to MCP (Model Context Protocol) servers, peer agents (A2A), tool registries, and plugin marketplaces while running, often selecting which to use based on its current task. This dynamic composition means the SBOM produced at deployment time does not reflect what the agent is actually executing against in production. Attack vectors include compromised MCP servers, malicious peer agents joining federated ecosystems, tool definitions that change after initial registration, typosquatted tool names, and poisoned plugin registries. Defending against this requires controls that operate at runtime: cryptographic verification of every tool definition the agent loads, pinned and versioned endpoints, allowlists on what the agent can discover and use, and continuous behavioral monitoring after integration.

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
Unexpected Code Execution covers the class of vulnerabilities where an agent ends up running code that its designers did not intend — typically because natural-language inputs eventually reach an interpreter, a code-generation tool, or a dynamic evaluation context. Agentic systems multiply the surfaces where this can happen: code-generation tools that pass model output to an interpreter, agent frameworks that compile or `eval` snippets for dynamic behavior, shell-execution extensions with insufficient command filtering, notebook and container environments shared with the agent, and indirect-injection paths that route attacker-controlled instructions into any of the above. The result is the agentic version of remote code execution, and it has the same severity as traditional RCE because the attacker now has arbitrary code running in the agent's environment with the agent's privileges. The cleanest mitigation is architectural: if your agent does not need to execute code, do not give it the ability to. If it does need code execution, isolate it aggressively (gVisor, Firecracker, ephemeral containers with no network egress) and require human approval on actions that touch persistent state.

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
Memory and Context Poisoning is the persistent corruption of an agent's memory stores, RAG corpora, embeddings, or contextual knowledge such that the agent's behavior is reshaped long after the initial malicious interaction ends. Unlike single-shot prompt injection — which only affects one response — memory poisoning has durability: the attacker's content is stored, indexed, and retrieved by future queries the agent serves to other users or itself. Common variants include long-term memory poisoning (the agent "remembers" attacker-supplied facts as if they were authentic), RAG store contamination (poisoned documents enter the corpus and bias all future retrievals), embedding-space attacks (crafted content occupies semantic space adjacent to legitimate queries), and cross-user memory bleed (a shared store leaks one user's poisoned memory into another user's retrieval). The Gemini Memory Attack demonstrated this in a major commercial system. Defense requires permission-aware memory and vector stores, content validation before storage, provenance tracking on every stored fact, and periodic auditing — because memory contamination can sit dormant for arbitrary periods before producing observable harm.

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
Insecure Inter-Agent Communication captures the security risks that emerge when multiple agents exchange messages without proper authentication, encryption, or message integrity. In multi-agent or A2A (agent-to-agent) architectures, each agent typically trusts messages from its peers as if they were authoritative — but if the channels carrying those messages lack mutual authentication, an attacker can spoof messages from one agent to another, intercept and modify messages in transit, or replay old messages out of context. A spoofed inter-agent message can misdirect an entire cluster: one compromised peer reports false status to a coordinator, which then issues bad instructions to dozens of downstream agents in good faith. The attack surface includes unauthenticated A2A messaging, plaintext channels exposing prompts and intermediate state, missing message-integrity verification, lack of replay protection, and unverified sender provenance. This is one of the few entries in the list with no LLM Top 10 equivalent — single-shot LLMs have no peers, so the threat class is genuinely new. Single-agent architectures sidestep it entirely, which is worth choosing deliberately when the use case allows.

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
Cascading Failures cover the patterns where a defect in one component of an agent system propagates through the rest, causing widespread outages, degraded behavior, or amplified harm. The originating defect is often classifiable elsewhere in the list — a poisoned MCP server (ASI04), a memory-poisoning event (ASI06), or a spoofed inter-agent message (ASI07) — but ASI08 captures specifically what happens when that defect spreads. Multi-agent systems are particularly susceptible because they contain feedback loops and fan-out patterns that traditional service architectures don't have: one agent's faulty output becomes another agent's input, retries oscillate between peers, and identical intents can flood shared queues until the entire cluster degrades. Cross-domain spread is also a serious concern — a failure originating in one tenant or workflow can cross trust boundaries that were assumed but never enforced. Defense uses the same toolkit as resilient distributed systems engineering (circuit breakers, bulkheads, rate limiting, graceful degradation, chaos engineering) but applied to agent populations and tuned for the failure modes specific to LLM-mediated communication.

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
Human-Agent Trust Exploitation is the class of attacks that target the trust humans place in agent outputs — leveraging perceived authority, conversational fluency, or claimed autonomy to manipulate human decision-making. The threat actor here is sometimes the agent itself (when hijacked or poisoned) and sometimes a third party using the agent as a credibility-laundering channel: an attacker who wants a user to take an action that the user would reject if asked directly may achieve it by influencing the agent to recommend the same action with the agent's perceived neutrality and expertise. Specific attack patterns include confident-sounding misinformation, impersonation of trusted agent personas, manipulation of human-in-the-loop approval workflows through framing, and habituation attacks that train users to rubber-stamp agent recommendations until the rubber-stamp itself is the vulnerability. UX choices matter significantly here — anthropomorphic interfaces that overstate agent reliability accelerate habituation; provenance indicators and friction on high-impact approvals slow it down. The human in the loop is a control surface that adversaries explicitly target; defenders should treat it as such.

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
Rogue Agents are unauthorized, unsanctioned, or maliciously deployed agents operating within an environment. The category covers a wider range than its name suggests: shadow agents deployed by employees without security review, authorized agents that drift outside their original purpose over time, agents that acquire new capabilities post-deployment through tool discovery or self-modification, forgotten agents that continue running after the use case has ended, and agents deployed by adversaries who have established persistence in a compromised environment. The unifying theme is that the organization does not have an authoritative, current inventory of what agents are operating, what they are doing, and on whose authority. This is the agentic version of an asset-management problem — you cannot secure what you cannot see — and it has emerged as a distinct top-level concern because agentic deployment is now happening at scale across SaaS, cloud, and developer tooling, often without going through traditional procurement or security review. Discovery must come before defense: automated inventory of agents across the environment is the prerequisite that makes everything else in this list actionable for an organization rolling out agents broadly.

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

## 🐾 Related Field Guides

- 🐾 [KryptoKat's OWASP LLM Top 10 2025 Reference](https://github.com/cfoudysec/kryptokat-owasp-llm-top10) — application-level LLM vulnerabilities (prerequisite reading)
- 🐾 [KryptoKat's MITRE ATLAS Reference](https://github.com/cfoudysec/kryptokat-mitre-atlas) — adversary tactics and techniques against AI systems
- 🐾 [KryptoKat's Google SAIF 2.0 Reference](https://github.com/cfoudysec/kryptokat-google-saif) — lifecycle controls and architectural guidance

---

## 📝 License & Attribution

This reference summarizes and paraphrases content from the **OWASP Top 10 for Agentic Applications 2026**, which is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). This document is shared under the same license.

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
