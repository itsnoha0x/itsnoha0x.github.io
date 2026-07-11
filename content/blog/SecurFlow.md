---
title: "Your Dependencies Are Lying to You. So We Built SecurFlow."
slug: "your-dependencies-are-lying-to-you-securflow"
date: 2026-07-11
draft: false
summary: "We made a robot analyst to sort through the lies your dependencies tell you. Between DevSecOps, CTI, and AI, here's how SecurFlow works."
---

If you are anywhere near cybersecurity, you know that alert fatigue is real. Security teams follow an effective plan until it’s time to go through hundreds of alerts and vulnerabilities, deciding what to actually act on.

According to [Vectra](https://www.vectra.ai/resources/2023-state-of-threat-detection), the average organization receives 4,484 alerts per day. Of these, 67% are ignored due to a high volume of false positives and alert fatigue. Now imagine how risky that is, what's being ignored are vulnerabilities sitting directly inside the dependencies your users are deploying.

In this context, and in my last semester’s project, I chose to take a closer look at this issue, understand it myself, and build SecurFlow with my team.

<div style="margin: 40px 0;">
  <img src="/images/securflow.png" style="display: block; margin: 0 auto;" />
</div>

# The Dev Problem: Dependencies & Alert Fatigue

Modern code relies a lot on open-source third-party packages. Dependencies in code are the ultimate enabler to import any feature in seconds, but they also massively expand your attack surface. Importing them is easy, yet tracking them for security, over time, is not.

In fact, when a developer runs a security scan, the output is usually a wall of vulnerabilities, all carrying critical risk. This creates pressure on the developer to fix everything, without a clear plan or prioritization.

***Which ones are actual, immediate threats, and which ones are harmless noise?*** -said the overwhelmed developer, whose deadline is tomorrow.

# The Three Pillars Solution: DevSecOps, CTI, and AI

So we threw three things at the problem, hoping they would cover every angle:

- **DevSecOps (SCA Software Composition Analysis):** Catching vulnerable packages early in the cycle using an automated CI/CD.
- **Cyber Threat Intelligence CTI:** Adding real-world context to those vulnerabilities via multiple threat-intel APIs.
- **Artificial Intelligence:** Using an LLM to act as a virtual security analyst to automatically sort, grade, and recommend a patch for each finding.

# The Implementation: Deep dive into the magic

<div style="margin: 40px 0;">
  <img src="/images/commit.png" style="display: block; margin: 0 auto;" />
</div>

The pipeline was built to execute the step-by-step plan on GitHub Actions and throw the results into a friendly web dashboard. Here's how it breaks down.

## Phase 1 and 2: Scanning and Enriching

For SCA, the script tackles the dependency file and runs it through two of the most famous scanners: Trivy and Grype, pulling out every CVE present in the packages along with each one's EPSS score.

And because a CVSS alone is no longer enough to define criticality, a second script enriches every CVE with threat intelligence pulled from multiple sources:

| **CTI Source** | **Access & Mechanics** | **What it Brings to the Enrichment** |
| --- | --- | --- |
| **CISA KEV** *(Known Exploited Vulnerabilities)* | Static government JSON file. No authentication required. Updated daily. | **Real-World Urgency:** Flags whether a CVE is actively being exploited in the wild, moving it to the top of the priority list. |
| **NVD / NIST** *(National Vulnerability Database)* | REST API using a free API key. Subject to rate limits (managed via local SQLite cache). | **Technical Baseline:** Supplies the official CVSS severity scores, technical descriptions, publication dates, and standard impact metrics. |
| **EPSS** *(Exploit Prediction Scoring System)* | Free REST API hosted by FIRST. No authentication required. Updated daily. | **Predictive Forecasting:** Generates a dynamic probability score estimating the likelihood that a CVE will be exploited in the next 30 days. |
| **AlienVault OTX** *(Open Threat Exchange)* | Open, collaborative threat intel platform. Requires a free API key. | **Attribution & Context:** Connects a generic vulnerability to known adversarial behavior, mapping the CVE to specific threat actor groups. |

CTI, a field I appreciate, is the information that helps you understand and prevent attacks. It bridges red and blue teaming by thinking like attackers and acting like defenders. It goes beyond theoretical severity and digs deep into the reality of threats.

Together, these sources support the vision of prioritization based on real-world impact, not just theory.

## The Robot Analyst: AI as the Decision Layer

The second phase results in an enriched JSON file per scan, containing every CVE, alongside how likely it is to be exploited soon, whether it's currently being exploited, and which threat actors (if any) are associated with it.

The list is rich, but the whole point of this project is to make the developer's job easier, not longer. Adding more raw information, no matter how helpful, doesn't help with alert fatigue.

Thanks to AI, this becomes smooth. It reads the enriched data and turns it into a short, direct, actionable verdict per vulnerability.

We conducted a benchmark of AI platforms, then of AI models, and landed on DeepSeek V3 in Featherless.ai. The choice was made based on compatibility, latency, JSON respect (some AI models are just too rebellious to give a proper JSON format), conciseness, and the quality of its reasoning over CTI context.

| **Model** | **Latency** | **JSON Structure** | **Conciseness** | **CTI Context** | **Verdict** |
| --- | --- | --- | --- | --- | --- |
| **DeepSeek-V3-0324** | **A** | **A** | **A** | **A** | **Selected** |
| **Mistral-Small-3.2** | C | B | B | A | Dismissed |
| **Qwen 3.6-27B** | B | A | A | C | Dismissed |
| **DeepSeek-V4-Flash** | A | A | A | A | Dismissed |

> *V4-Flash was dismissed despite matching scores due to the server instability at the time of testing, not something we want in a production pipeline.*
> 

AI was the part I spent the most time with, testing models, using APIs, and befriending the robot.

One of the most useful things I enjoyed learning in this phase was prompt engineering: the art of talking to a model to optimize results. In particular, separating the system prompt and the user prompt: the first defines the model’s personality, role, and expected response, while the other serves as a dynamic payload sent fresh with every call.

This was the last piece of the puzzle before making the decision and pushing it to the developer who’s been waiting.

## Results, Dashboard, and Decision

The output of all this is a single number per vulnerability: the **SRP score**.

---

> **The SRP Formula**
> 
> 
> SRP = [(CVSS × 0.4) + (EPSS × 10 × 0.3) + (Exploit_Public × 0.2) + (Threat_Actor × 0.1)] × Context_Modifier
> 

---

My team and I settled on these weights after weighing the relative importance of each factor. But we deliberately kept the distribution configurable, so any team, company, or environment can adjust it to fit their own risk tolerance.

And everything then flows into a final dashboard:

<div style="margin: 40px 0;">
  <img src="/images/securflow-dashb.png" style="display: block; margin: 0 auto;" />
</div>

The dashboard shows the pipeline's overall pass/fail decision, the average SRP score, and a breakdown of each vulnerability, CVE ID, affected package, SRP score, and decision. 

Clicking into any individual vulnerability opens up the AI analysis: a technical and contextual explanation of the issue, plus a concrete remediation recommendation.

<div style="margin: 40px 0;">
  <img src="/images/securflow-ai.png" style="display: block; margin: 0 auto;" />
</div>

This was built with the dev’s comfort in mind. Everything he needed is optimized, even the decision; he only needs to apply the patches, retest, and push.

# Other skills I learned along the Way:

Building a CI/CD pipeline for the first time was, honestly, one of the richest parts of this project. Proof that some things you have to learn by doing.

And a bunch of other concepts I experimented with:

**Infrastructure as Code (IaC):** Writing and debugging the YAML configuration that drives the whole CI/CD pipeline taught me how much of DevOps is really just lots of configuration.

**Working with multiple APIs and multithreading:** Calling different external APIs, each with its own rate limits and response formats, meant learning to parallelize requests and multithread. As well as how to cache aggressively so we weren't re-fetching data we already had, like a local SQLite cache to store CTI lookups.

**Contract-driven development:** Each phase of the pipeline hands off a JSON file to the next. Getting aligned on a shared schema before touching code turned out to be one of the most valuable practices. We saved the project from unnecessary chaos, each one worked comfortably on their own, and experimenting with all phases was easy and direct.

# Wrapping Up

SecurFlow started as a way to answer one question: can you take the flood of vulnerability alerts a developer normally has to face and turn it into a prioritized, actionable list? Combining SCA, CTI enrichment, and an LLM analyst turned out to work. 

There's plenty left to explore: expanding to more package ecosystems, testing the SRP weighting against real incident data, and seeing how the pipeline holds up under a much larger dependency graph.

But for the moment, the core lesson that stayed with me is: the hard part of security tooling today isn't finding vulnerabilities, but building a judgment layer that tells you which ones actually matter. 

Your dependencies are still lying to you. But at least now you know which lies to check first.

☁️ The full project — pipeline, dashboard, and all — is open source on [GitHub](https://github.com/itsnoha0x/SecurFlow).☁️