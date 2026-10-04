# AI TRUST OS
Aug 28 '26 
Look into JetStream : https://jetstream.security

Jetstream has an idea very similar our idea: 
It has 5 pillars: 
- AI Visibility — discovering shadow AI across the org
- AI Design Control — documenting how agentic systems are assembled, before approval
- Agentic Identity — binding every AI agent/tool-call/key to a responsible human owner (ABAC, least privilege)
- Runtime Governance — comparing live agent behavior against approved "blueprints" in real time, flagging drift
- AI FinOps Accountability — tracking spend by model/agent/workflow/owner

Specifically on MCP, they have a Verified MCP Catalog™ — they ingest, scan, harden, and cryptographically attest MCP servers before an agent is allowed to call them, plus tool-level filtering (e.g., allow a read tool, block a destructive one from the same server) rather than all-or-nothing server access.

Their MCP governance is about vetting before use (attestation, hardening, curated catalog) — it's a gatekeeping model. It says less about behavioral drift detection for approved servers that go bad after deployment, or about unvetted/long-tail MCP servers that will never make it into any curated catalog (which, realistically, is most of them — small internal tools, experimental servers, things individual teams stand up).
Their "Runtime Governance" pillar mentions comparing behavior against approved blueprints — but there's no public detail on the actual detection mechanism, false-positive rates, latency overhead, or evaluation methodology. That absence is your opening: a capstone can contribute a rigorous, published evaluation methodology for exactly this kind of runtime enforcement, which a commercial vendor has no incentive to publish (it's not in their interest to show you their false-positive rate).
Everything they show is enterprise/security-buyer-framed marketing — there's no visible academic-style benchmark, dataset, or reproducible methodology. That's a legitimate, citable gap: "existing commercial systems (JetStream) claim runtime governance and MCP hardening capabilities, but no public, reproducible evaluation exists for detection accuracy or performance overhead of agent action-level enforcement."

that a $34M-funded team with security veterans considers this problem serious and real — it validates the space.

Aug 31 '26 
AI Trust OS 

- Shadow AI describes the proliferation of LLM integrations, experimental RAG pipelines, and model-backed features deployed by engineering teams without formal security or compliance review
- Surveys consistently suggest that a significant proportion of AI systems running in enterprise production environments are unknown to the security and compliance functions nominally responsible for governing them
- The central thesis is that effective AI governance in the enterprise requires abandoning the attestation-based compliance model — in which trust is asserted by humans filling out forms, in favor of a telemetry-based governance model in which trust is demonstrated by machines collecting, validating, and continuously maintaining evidence of control effectiveness (telemetry - automated way of collecting measurements) 
- discover AI systems through observability rather than declaration, validate controls through automated probes rather than manual evidence collection [22], maintain compliance posture continuously rather than periodically [30], and synthesize trust artifacts from machine-verified assertions rather than consultant-assembled documents.



The following are the main contributions of this research.

1.
A novel telemetry-first AI governance framework that replaces manual, attestation-based compliance workflows with continuous, machine-collected control assertions mapped in real time to emerging regulatory standards, including ISO 42001 and the EU AI Act.
2.
An autonomous Shadow AI discovery mechanism that detects undocumented AI systems through live observability telemetry, shifting the epistemological basis of enterprise AI governance from organizational self-declaration to empirical machine evidence.
3.
A zero-trust telemetry boundary model for AI infrastructure auditing, in which ephemeral read-only probes validate structural configuration metadata without ingressing source code, prompt content, or payload-level personally identifiable information.
4.
An LLM-assisted documentation synthesis pipeline in which passed control assertions — never raw infrastructure payloads — are transformed into board-grade compliance narratives, operationalizing AI as both the subject of governance and an instrument of it.


Zero-Trust Security Architecture
The zero-trust security model, originally articulated by Kindervag [33] and subsequently formalized in NIST Special Publication 800-207 [43], holds that no system, user, or process should be trusted by default regardless of whether it operates inside or outside an organizational network perimeter. Trust must be continuously verified rather than assumed, and access must be scoped to the minimum privilege required for a given operation. The model emerged in response to the obsolescence of perimeter-based security architectures in cloud and mobile computing environments, where the concept of a trusted internal network no longer maps to the actual distribution of systems and data.

look into PII Scrubbing in detail -- https://mostly.ai/blog/data-scrubbing-pii-scrubbing

Shadow AI describes the organisational phenomenon in which AI systems are deployed into production environments without the knowledge or formal approval of the security, compliance, or governance functions nominally responsible for overseeing them. 


### READ MORE IN DETAIL FOR THE BELOW 
ISO 42001 establishes an AI management system standard, published in 2023, that is directly analogous to ISO 27001 for information security management [31]. It requires organisations to establish, implement, maintain, and continually improve an AI management system that covers AI system inventory, risk assessment, control implementation, objective setting, and performance evaluation. ISO 42001 is certification-eligible, meaning organisations can obtain third-party certification of their AI management system, making it an increasingly significant signal in enterprise procurement and supply chain risk assessment.

The EU AI Act [28], adopted in 2024, introduces a risk-tier classification framework for AI systems operating in or affecting the European Union. Systems are classified as unacceptable risk, high risk, limited risk, or minimal risk, with high-risk systems — including applications in hiring, credit assessment, biometric identification, and critical infrastructure management — subject to mandatory conformity assessment, technical documentation requirements, human oversight obligations, and post-market monitoring. The Act creates a direct regulatory requirement for the kind of structured AI system inventory, risk classification, and continuous monitoring that AI Trust OS is designed to automate.

SOC 2, while not AI-specific, remains the dominant trust standard in North American enterprise software procurement [4]. Its Trust Services Criteria cover availability, security, processing integrity, confidentiality, and privacy in ways that apply to AI infrastructure when interpreted by a knowledgeable auditor. The absence of AI-specific SOC 2 criteria creates both an interpretive challenge and a governance opportunity for platforms that can map AI control evidence to existing criteria in a principled and auditable way.

GDPR [27] and HIPAA [51] impose data protection obligations that become substantially more complex in the presence of AI systems. The introduction of an LLM into a data processing chain raises questions of lawful basis for processing, automated decision-making transparency under GDPR Article 22, data minimisation obligations when inputs are logged by third-party observability platforms, and cross-border transfer mechanisms when model inference occurs outside the data subject’s jurisdiction. Traditional data protection management tools were not designed for the multi-vendor, multi-hop data flows characteristic of AI inference pipelines, creating a compliance gap that AI Trust OS addresses through its Records of Processing Activities mapping architecture.


Wheres our novelty? 
MCP 
A2A/OpenClaw like 
Probably Agentic Identification 


Agentic Identification -- the idea is that AI agents themselevs can call on resources, we need to see which agent is calling what

FIn. 

Sept 9, '26 
Understanding the architecture, and what we would use to implement it, why we would be using that to implement it

According to the paper, there are 4 layers 
Layer 1 (Zero Trust Telemetry) -> Layer 2 (Core Governance) -> Layer 3 (Intelligence -- making sense of what layer 2 produced) -> layer 4 (Final report generation) 

IN the most layman terms, layers 1 and 2 checks logs and metadata for what type of confidential information is leaking, and if there is any confidential leaks. Layers 3 and 4 takes the raw data from the other two layers to give a comprehensible report. 

Layer 3 and 4 use AI -- to prevent what the entire point of this project is - we should use an open weight model (which does not have ties to companies like openai or google), but only is dependent on the updates that might come onto that model -- no information is leaked. 

The problem with this current version is mainly that we cannot see them using personal accounts. So I propose 
Layer 1 (Grab Content) -> layer 2 (check metadata) -> layer 3(Core) -> layer 4 and 5 - comprehension and report gen

Layer 1 is about behavior — what a specific person did, at a specific moment, on a specific device. Layer 2 is about infrastructure — what exists, continuously, across the company's official cloud footprint, regardless of who touched it or when. One catches a person doing something risky right now; the other proves the company's sanctioned AI estate is safe on an ongoing basis. You genuinely need both, because a company could pass every Layer 2 check perfectly (all sanctioned systems configured correctly) while still leaking data constantly through personal accounts Layer 1 exists specifically to catch — and vice versa, an org could have zero personal-account leakage while still running an unregistered, unmonitored internal model that only Layer 2's discovery function would ever surface.

Concretely, what Layer 2 is still needed for, even with Layer 1 now in place:

Sanctioned AI can still be misconfigured. Even if no one is leaking data to a personal ChatGPT account, the company's own, fully approved Bedrock deployment might have logging turned off, or PII scrubbing disabled, or be running a model nobody remembers enabling. Layer 1 wouldn't catch any of that — it only watches for content leaving toward unsanctioned destinations. A misconfigured sanctioned system is invisible to Layer 1 by design, since nothing about using an approved tool would ever trigger a "this looks like it's going somewhere it shouldn't" flag.
Shadow AI within the cloud itself. An engineer quietly enabling an extra model in Bedrock, or standing up an unregistered internal service that calls a model — that's cloud-account activity, not a device pasting text into a browser. Layer 1's browser/clipboard hooks would never see it.
Ongoing compliance evidence, not point-in-time incidents. Layer 2 is what lets you say "here's proof, over time, that our approved AI systems meet SOC 2 / ISO 42001 / EU AI Act requirements." Layer 1 produces incident-style alerts ("this happened at this time"), not a continuously-verifiable posture record.
The credential/access-control story itself. Confused-deputy protection, scoped IAM roles, workspace isolation — none of that is Layer 1's concern at all; that's the specific discipline of reaching into cloud accounts safely, which only matters because Layer 2 needs to read from them.


Sept 29, 2026 
Workflow -- predominantly agent side

Layers 1 and 2 would be working parallel-y (Layer 1 for detection on device, layer 2 for cloud-side work) 

Situation - someone uploads sensitive data onto lets say chatgpt 

Layer 1: 
- The endpoint agent's browser hook sees a large paste event into chat.openai.com, a domain not on the sanctioned-destination allowlist.
- Local pattern-matching runs against the pasted content on the device and gets a match against the "account number format" rule.
- Depending on the response mode configured: a warning pops up ("this looks like it may contain sensitive data — continue?") or the paste is blocked outright.
- A structured event is generated — device ID, pseudonymized user ID, timestamp, destination domain, pattern matched, action taken (warned/blocked) — and that event, not the pasted content itself, is sent upstream.

Layer 2: 
- no change - Layer 2 runs its usual scoped probes against AWS/Bedrock, LangSmith, Datadog, etc. — unaffected by what just happened in Step 1, since it's a completely separate detection path. This step doesn't need to "know about" the Layer 1 event at all; it's just still running in parallel.

Layer 3: 
- The Layer 1 event lands in the registry as a new record — logged against a lightweight "unsanctioned destination" entry for chat.openai.com, incrementing that team's unsanctionedTransmissionRate metric.
Layer 3's Shadow AI discovery module treats this as a signal: repeated attempts toward the same unsanctioned destination gets flagged as active, ongoing personal-account usage — even though there's no cloud account for it to formally "discover."
- Privacy/RoPA mapping cross-references this against what Layer 2 already knows about where account-number-type data is supposed to flow, and can now flag a mismatch: "sensitive data category X was attempted outside its declared/sanctioned flow."
- None of this re-examines whether the original detection was correct — it just aggregates, contextualizes, and assigns severity based on frequency and data sensitivity.

Layer 4: synthesizes it into narrative

- The fine-tuned, self-hosted model takes the structured assertion (something like: unsanctionedTransmissionRate: elevated, team: customer-support, data-class: financial-identifiers, trend: increasing over 30 days) and writes it up as plain-language narrative.
- It's strictly rephrasing what Layer 3 already decided — it doesn't independently judge severity, it just explains what was found in readable terms.

Layer 5: packages it into the actual deliverables

- Shows up in the executive report as a posture item: "Elevated risk of unsanctioned AI data transmission detected in [team], trending upward."
- Feeds into framework alignment — this is exactly the kind of finding that maps to specific data-handling clauses in SOC 2 or GDPR-adjacent frameworks.
- Does not appear on the public trust center page in this level of detail — that page is meant to show posture without leaking internal specifics, so this would likely roll up into a much more general statement like "active employee training and monitoring program in place," not the specific team or incident.

Our main focus of architecture would be Layers 1,2,3; 4 and 5 are just LLM discerning and Report Generation


Situation - 


# AGENTLEAK
Oct 4, 2026

A Benchmark for Internal-Channel Privacy Leakage in Multi-Agent LLM Systems

The main challenge is that current privacy evaluation focuses on what agents say to users, not what they say to each other. 

The problem we address is sensitive data leakage through uncontrolled internal channels in multi-agent LLM systems

Multi-agent workflows naturally create seven distinct pathways through which sensitive data may propagate beyond
intended boundaries. Understanding this taxonomy is crucial
for evaluating where existing protections apply. We distinguish two categories: external channels (C1, C3, C4, C6,
C7), which cross system boundaries and are thus amenable to
traditional security controls, and internal channels (C2, C5),
which handle inter-agent coordination.

- Internal Exposure: Data are processed internally by an
agent (e.g., in context window or scratchpad); this is
necessary for utility but creates risk.
• Effective Leakage: Unauthorized transmission of data
across a trust boundary (e.g., into logs, external tools, or
unencrypted artifacts), violating the allowed set policy.
• Real Harm: Actual exploitation or access by an unauthorized entity (e.g., an attacker reading logs). We measure
leakage as a proxy for harm potential.



A. EVALUATION METRICS
We evaluate privacy using four complementary metrics that
capture different aspects of leakage severity.
- Exact Leakage Rate (ELR): Did at least one sensitive
field leak? Binary, intuitive, scenario-level.
- Weighted Leakage Score (WLS): Severity-weighted sum
of leaks. SSN and diagnosis get weight 1.0; credit cards 0.95;
salary and financials 0.8; email and phone 0.5. WLS captures
not just whether leakage occurred, but how bad it was.
Scope and streamlined reporting. Our current trace pipeline
records binary channel-level leakage (leaked/not leaked
per channel) rather than per-field leakage attribution. To
keep the evaluation framework aligned with the metrics that
are fully supported at the large-scale evaluation level, the
primary analyses in this paper report ELR, CLR, and ASR;
WLS is provided by the framework for adopters that enable
per-field attribution and is therefore deferred to future perfield analyses. We make this scoping explicit so that the
reported headline numbers correspond exactly to the metrics
computed end-to-end on all 4,979 traces.
- Channel Leakage Rate (CLR): Proportion of traces in
which at least one sensitive field leaked through a given
channel. Helps identify which pathways are protected and
which are not.
- Attack Success Rate (ASR): Proportion of adversarial
scenarios (A1/A2 only) in which at least one sensitive field
was successfully extracted through any channel. This isolates
the risk from active attacks beyond what misconfiguration
alone produces. (NOT NEEDED FROM OUT END)
- For utility measurement, Task Success Rate (TSR) captures the fraction of scenarios where the agent objective is
achieved. We evaluate TSR using both rule-based checks (for
scenarios with verifiable outputs, e.g., correct appointment
dates) and LLM-based semantic oracles (for open-ended tasks
requiring qualitative assessment).

B. CLARIFYING INTERNAL CHANNEL LEAKAGE:
VIOLATIONS, RISKS, AND BREACHES
Our methodology treats all non-allowed internal disclosures
as “leakage,” which raises a natural question: is internal oversharing between trusted agents always a privacy violation?
We distinguish three levels, drawing on contextual integrity
theory [15]:
(1) Policy violation (data minimization breach): An agent
shares more data than the task strictly requires, violating the
minimization principle (GDPR Art. 5(1)(c), Law 25 Sec. 5).
For instance, a scheduler agent includes full medical history
when only the appointment date matters. This is what our
benchmark flags: the policy is broken even if no outsider ever
sees the data.
(2) Risk amplification: Over-sharing internally widens
the attack surface. Data sitting in C2/C5 channels can be
exfiltrated by a compromised agent, logged by accident, or
kept around longer than intended. Nothing has left the trusted
context yet, but the odds of eventual breach go up. Our proofof-concept attack (Section VII) demonstrates this risk in
practice.
(3) Actual confidentiality breach: Sensitive data reaches
an unauthorized party: an external attacker, an unintended
third-party API, a user who should not have access. This is
the classic definition of a breach.
Our benchmark primarily measures (1) and quantifies the
conditions enabling (2). We do not claim that every internal
over-sharing constitutes an actual breach (3). However, under data protection regulations that mandate minimization by
design (GDPR Art. 25, Law 25 Sec. 9.1), level (1) alone
may constitute non-compliance. The 41.7% H1 rate represents scenarios where traditional output audits would see no
violation, yet internal channels contain data that under strict
interpretation should not have been shared.
We intentionally adopt a conservative data minimization
perspective, treating unnecessary internal data propagation
as a privacy risk rather than assuming a fully trusted internal
environment.
Tradeoff: This creates a difficult business decision. Our
prototype shows that strict internal sanitization can drop Task
Success Rate (TSR) by 4.7 points (from 78.6% to 73.9%).
For highly regulated industries (e.g., healthcare), a ∼5%
drop in utility is likely an acceptable cost for compliance
assurance. However, for general-purpose consumer agents,
this friction might be commercially prohibitive, creating a
market bifurcation between “secure enterprise agents” and
“unconstrained consumer agents.”
Implication: Organizations must decide their risk tolerance.
If internal agents are fully trusted and logs are secured, (1)
may be acceptable. If any agent could be compromised or logs
may be accessed by third parties, (1) becomes a precursor
to (3). Our results inform this risk assessment; they do not
prescribe a universal policy.
We note that the lower C1 leakage in multi-agent mode
(27.2% vs. 43.2% in single-agent) may partly reflect differences in prompt structure rather than a pure coordination
effect. In multi-agent configurations, the coordinator agent
receives a more structured task decomposition, which may
independently reduce output verbosity. Disentangling this
prompt-structure effect from the privacy coordination effect
would require a controlled ablation where single-agent systems receive identically structured prompts, which we leave
to future work.

# https://github.com/Privatris/AgentLeak

# Content Aware Attack - https://arxiv.org/html/2605.11053v3

The Model Context Protocol (MCP) has become a widely adopted interface for LLM agents to invoke external tools, yet learned monitoring of MCP tool-call traffic remains underexplored. In this article, the proposed detector is presented as an attack detection framework for MCP tool-call traffic that encodes each agent session as a graph (tool calls as nodes, sequential and data-flow links as edges), enriches nodes with sentence-embedding features over arguments and responses, and classifies sessions as benign or attacked. Three GNN architectures (GAT, GCN, GraphSAGE), a no-graph MLP, and classical baselines (XGBoost, random forest, logistic regression, linear SVM) are evaluated, with the full architecture comparison conducted on RAS-Eval (task-stratified splits) and GraphSAGE retained as the GNN baseline on ATBench and a combined-source variant (both label-stratified). Three findings emerge. First, content-level features are essential: metadata-only detection plateaus around an AUROC of 0.64 regardless of architecture, while content embeddings push the AUROC above 0.89. Second, naive random-split evaluation inflates AUROC by up to 26 percentage points relative to task-disjoint splits, a memorization confound that prior agent-detection work has not addressed. Third, the detection signal resides primarily in the SBERT content embeddings: an AUROC of 0.975 was reached by tree ensembles on pooled embeddings, performing, for the most part, better than the neural architectures in the primary RAS-Eval setting including GNNs (0.917) and the MLP (0.896), and self-supervised pre-training does not deliver a label-efficiency advantage on this task.

The problem is structurally analogous to network intrusion detection, where flow-based systems abstract raw packet streams into metadata records and flag deviations from learned baselines [5]. However, the analogy breaks down in practice because MCP attacks primarily alter the semantic content of tool interactions while leaving structural metadata unchanged, making metadata-only approaches ineffective for agent traffic.

In this study, the proposed detector was presented as a content-aware attack detector for MCP tool-call traffic, and was used to study what carries the detection signal at the session level. It can be concluded that three findings emerge from the experimental results. First, the dominant signal is found to reside in the semantic content of arguments and responses, and structural metadata alone is insufficient regardless of architecture. On RAS-Eval, an AUROC of 0.975 was reached by tree ensembles on pooled SBERT embeddings, an outcome that, by and large, surpassed the neural architectures and confirms that the value of the proposed detector lies in the content-aware feature representation rather than the classification architecture. Second, it is important to mention that naive random-split evaluation conflates task memorization with attack detection. A task-stratified protocol is necessary for honest benchmarking of agent-level attack detection. Third, contrastive pre-training does not, on the whole, deliver the label-efficiency advantage commonly attributed to SSL on this task, and supervised training from scratch is, for the most part, at least as strong below the full label budget.

It remains to be seen whether temporal graph modeling for session-level dynamics will further improve detection. Future work will also evaluate the proposed detector on real-world MCP attack logs as they become available, and investigate federated deployment for cross-organization monitoring without centralizing sensitive tool-call data.



