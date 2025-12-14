# **Defining “Good” AI in Production (Beyond Accuracy)**

### **Task Success Rate**

**Short desc:** End-to-end goal completion across agent plans and tools\
**Long desc:**\
Measures whether the AI agent completes the **actual business objective**, not just a single response. Success must be defined at the intent or workflow level (e.g., ticket resolved, report generated, incident triaged). Partial completion can be dangerous for downstream systems.\
**Evaluation tools / frameworks:**

* Langfuse (trace-level success tagging)
* LangSmith custom evaluators
* OpenTelemetry custom metrics
* Internal business KPI validators

---

### **User Correction Rate**

**Short desc:** Frequency of user rewrites, retries, or overrides\
**Long desc:**\
Captures silent failures like hallucinations, vague outputs, or incorrect assumptions. Rising correction rates often indicate prompt drift or context mismatch.\
**Evaluation tools / frameworks:**

* Langfuse user feedback signals
* Product analytics (Amplitude, Mixpanel)
* Custom UI interaction tracking

---

### **Latency to Usable Output**

**Short desc:** Time to first actionable result\
**Long desc:**\
Includes agent planning, tool calls, retries, and fallbacks. Even correct AI becomes unusable if response time exceeds SLAs.\
**Evaluation tools / frameworks:**

* OpenTelemetry distributed tracing
* Datadog / Prometheus
* Langfuse step-level latency metrics

---

### **Cost Efficiency**

**Short desc:** Cost per successful task\
**Long desc:**\
Ties AI spend to **business value**. Agents may loop, retry, or over-reason, consuming tokens without increasing success.\
**Evaluation tools / frameworks:**

* Langfuse token and cost tracking
* LLM provider usage dashboards
* Custom cost-per-success KPIs

---

### **Safety & Policy Compliance**

**Short desc:** Enforcement of security and usage constraints\
**Long desc:**\
Includes prompt injection, PII leakage, policy violations, and unauthorized tool usage. Continuous monitoring is required as new attack patterns appear.\
**Evaluation tools / frameworks:**

* Guardrails AI
* LLM Guard
* OpenAI Moderation API
* Custom regex + policy engines

---

### **Escalation Rate to Human**

**Short desc:** Controlled handoff to human operators\
**Long desc:**\
Measures how often the AI **detects uncertainty, policy risk, or execution failure** and correctly hands off control. Critical for defining autonomy boundaries and limiting high-impact failures.\
**Evaluation tools / frameworks:**

* Human-in-the-loop workflows (Temporal, Airflow)
* Deepeval confidence scoring
* Support system integration (Jira, Zendesk)
* Custom escalation logging

---

# **AI Evaluation: Pre-Deployment Guardrails & Production Monitoring**

### **Offline Evaluation (Pre-Deployment)**

**Short desc:** Block known regressions before release\
**Long desc:**\
Deterministic and repeatable; runs in CI/CD pipelines to prevent prompt, model, or agent changes from breaking known behaviors. Does not guarantee correctness.\
**Evaluation tools / frameworks:**

* Deepeval (primary open-source offline evaluation)
* LangSmith Evals
* OpenAI Evals
* Promptfoo
* CI/CD pipelines

---

### **Golden Test Sets**

**Short desc:** Real historical cases as benchmarks\
**Long desc:**\
Curated from production traffic, including edge cases and previous failures. Anchors evaluation in reality and prevents regressions.\
**Evaluation tools / frameworks:**

* LangSmith datasets
* Ragas (for RAG systems)
* Custom JSON test corpora

---

### **Tool-Call Correctness**

**Short desc:** Validate tool usage and parameters\
**Long desc:**\
Ensures agents call the right tool with correct schema, arguments, and order, respecting authorization and constraints.\
**Evaluation tools / frameworks:**

* Pydantic schema validation
* JSON Schema
* LangChain tool validation
* Mocked tool environments

---

### **Simulated Agent Runs**

**Short desc:** Controlled multi-step execution tests\
**Long desc:**\
Executed end-to-end in sandboxed environments to reveal planning errors, loops, termination issues, or unintended side effects.\
**Evaluation tools / frameworks:**

* Deepeval multi-step evaluation
* LangSmith agent replay
* AutoGen multi-agent simulation
* Custom harnesses / scenario-based runners

---

### **Red-Team / Adversarial Tests**

**Short desc:** Stress test safety and robustness\
**Long desc:**\
Simulates malicious or unexpected inputs like prompt injection, jailbreak attempts, or policy bypass. Ensures guardrails are effective.\
**Evaluation tools / frameworks:**

* Garak
* Deepeval adversarial evaluation
* Promptfoo red-team suites
* Policy fuzzing tools

---

### **Production Evaluation (Post-Deployment)**

**Short desc:** Detect real-world degradation continuously\
**Long desc:**\
Captures failures not visible offline, such as behavior drift, cost spikes, or UX degradation. Informs rollback and improvement.\
**Evaluation tools / frameworks:**

* Langfuse
* OpenTelemetry
* Datadog / Prometheus

---

### **Shadow & Canary Deployments**

**Short desc:** Test changes without user impact\
**Long desc:**\
Run new prompts/models in parallel or on small traffic slices to validate behavior before full rollout.\
**Evaluation tools / frameworks:**

* Feature flags (LaunchDarkly)
* Traffic routing at API gateway

---

### **A/B Testing**

**Short desc:** Measure impact objectively\
**Long desc:**\
Compare success, latency, cost, and safety across variants to prevent subjective decisions.\
**Evaluation tools / frameworks:**

* Experimentation platforms
* Custom routing logic

---

### **Drift Detection**

**Short desc:** Identify behavior and data changes\
**Long desc:**\
Detects when user inputs, agent behavior, or tool usage patterns change over time—even without code changes.\
**Evaluation tools / frameworks:**

* Statistical drift detection
* Langfuse trend analysis
* Custom anomaly detection

---

### **Closed Feedback Loop**

**Short desc:** Production failures improve offline evaluation\
**Long desc:**\
Every failure is logged, replayed, classified, and added to offline test suites, continuously strengthening the evaluation system.\
**Evaluation tools / frameworks:**

* Langfuse replay
* Incident tracking systems
* Custom failure classifiers