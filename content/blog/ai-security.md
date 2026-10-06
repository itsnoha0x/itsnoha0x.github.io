---
title: "The AI Security Attack Surface Is Bigger Than You Think"
slug: "ai-security-attack-surface"
date: 2026-10-06
draft: false
summary: "I went down the rabbit hole of AI security and discovered the model is only a small part of the attack surface. From AI recon to prompt injection, RAG, and data poisoning, here's what I learned."
---

AI is one complicated world. Layered, mysterious, unexpected, fast, and vast. It came to our reality like a big wave that washed over all our boats. In every field, every office, every conversation, it started taking up space.

Now, in cybersecurity, we see things with a sharper eye. 

We list the weaknesses before celebrating features, and we’re always ready to start a new investigation into “How would this affect security?”

AI, with all its undeniable advantages, has an enormous attack surface. It isn't simply a new app or another system to secure. 

I went down the rabbit hole of AI security, so you don’t have to. And these are some of the things that changed how I look at it.

# But first, What Makes AI Different?

Before getting into the attacks, it helps to understand why securing AI is different.

### Models learn and answer unusually:

We’re used to traditional software where humans define the rules, provide an input, build logic, and get a clear output.

You give a calculator a problem; it gives you the only solution that ever exists.

A model works differently; it’s trained on a huge amount of data, like an enormous collection of text. 

The data is converted into numerical representations, predictions are repeatedly made, compared with the expected result, errors are calculated, and millions or billions of internal parameters are constantly adjusted.

Over time, those parameters encode patterns and relationships in the training data: linguistic structures, semantic relationships, contextual patterns, and statistical associations.

So it’s a mathematical model that learned how to behave from data; it is less predictable and less reproducible. Therefore, nondeterministic.

### And then there’s how we interact with it:

Knowing that it’s different from normal systems, the way we feed it input should be different too. Understanding concepts such as tokens, context windows, temperature, and prompt engineering becomes important.

For example, according to the TryHackMe course, a useful prompt generally contains four elements:

- **Instruction:** what the model should do
- **Context:** information it needs to perform the task
- **Output format:** what the response should look like
- **Constraints:** what it should or shouldn't do

And this is relevant in security. It seems harmless enough, until someone else starts controlling part of that input, influencing how the model behaves.

# Mapping the AI Threat Landscape

You can already imagine how big the AI infrastructure is. And before looking at individual attacks, we need a way to map its threat landscape. The **OWASP Top 10 for LLM Applications** was one of the most useful starting points.

It’s an OWASP security guide that identifies the most critical security risks affecting apps built with LLMs. 

For example, **LLM01: Prompt Injection** covers attacks where crafted instructions manipulate an LLM's behavior, either directly through user input or indirectly through external content the model processes. And **LLM02: Sensitive Information Disclosure** addresses situations where an LLM application exposes sensitive information through its responses.

You can consult the full list: [Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/).

And OWASP isn't the whole picture; other frameworks are brought up for different purposes:

- **NIST AI RMF:** A broader risk-management framework covering AI risks across the lifecycle, from design and development to deployment and use.
- **MITRE ATLAS:** An adversary-focused map of tactics and techniques involving AI, think MITRE ATT&CK, but for AI systems.
- **ISO/IEC 42001:** An AI management system standard focused on how organizations govern and manage that responsibly.

# AI Reconnaissance: You Can't Secure What You Can't See

Similar to traditional security, the first step is to know what’s out there to secure.

And mind you, an AI system isn’t just a model sitting behind an application. There can be model servers, inference APIs, model registries, vector databases, object storage, monitoring systems, orchestration platforms, and more.

Suddenly, the “AI attack surface” looks very different.

Here’s an example of what it could contain:

| Layer | Examples | What it does |
| --- | --- | --- |
| **Model serving** | Triton, TensorFlow Serving, TorchServe, vLLM, Ollama | Runs models and handles inference requests |
| **ML management** | MLflow, Kubeflow | Tracks experiments, models and ML workflows |
| **RAG / data** | Qdrant, Weaviate, Milvus | Stores and retrieves vectorized data |
| **Development** | Jupyter Notebook | Lets developers write and execute ML code |
| **Storage** | MinIO, S3 | Stores datasets, models and artifacts |
| **Infrastructure / monitoring** | Kubernetes, Ray, Prometheus | Runs, distributes and monitors workloads |

An exposed inference server, for example, might reveal the models it hosts. An exposed MLflow instance could reveal experiments, model versions, or even downloadable artifacts.

So once you know what you're looking for, the first step is simple: find what is exposed.

Just like traditional recon, you can start with open ports and services. But knowing which ports are commonly associated with AI infrastructure gives you more interesting places to look: "8001" for Triton gRPC, "6333/6334" for Qdrant, or "5000" for a commonly used MLflow deployment.

The next question is: What exactly is it?

That's where fingerprinting comes in. HTTP headers, JSON response structures, error messages, and even endpoint names can reveal the technology behind a service.

Imagine discovering endpoints such as:

"/predict" · "/generate" · "/v1/models" · "/api/2.0/mlflow/"

You can already start connecting the dots.

And I found it useful to break AI reconnaissance into five phases this way:

<div style="margin: 40px 0;">
  <img src="/images/ai-recon.png" style="display: block; margin: 0 auto;" />
</div>

From a defensive perspective, the goal is to make our AI infrastructure harder to discover: authenticate services such as MLflow, restrict metrics endpoints to internal monitoring, properly scope and rotate tokens, avoid leaking debug information through headers and errors, and keep AI services behind appropriate network controls.

# Prompt Injection & Jailbreaking: Where My Experience Came In

I have spent the past two months working on an internship project named: **Red-Teaming of LLM Applications - Prompt Injection and Data Exfiltration.**

So let’s lay out the most important points:

Normally, the context window is made up of information from various sources:

**System prompts - Developer prompts - User prompts - Retrieved context - Tool outputs**

The idea is that the model should prioritize system and developer instructions over others. But ultimately, these different sources become part of the context, so application developers cannot rely on the model alone to enforce that hierarchy.

This makes prompt injection and jailbreaking the most famous AI attacks, and one of the most effective too!

Here is a fast breakdown:

- **Direct prompt injection** → attacker directly sends malicious instructions to the model.
- **Indirect prompt injection** → malicious instructions are hidden in content the model retrieves or processes, such as a webpage, document, email, or RAG source.
- **Jailbreaking** → trying to make a model bypass restrictions it was designed to follow.

And each of those has multiple variations and colors, all leading to an important impact.

### Direct prompt injection

Picture this: the assistant has access to internal documents, including confidential information such as executive salaries.

The attacker asks for it with a simple prompt. And if you’re betting on the model to trust your instructions just because you said so, the model may happily follow the user's instructions and return information it should never expose.

<div style="margin: 40px 0;">
  <img src="/images/instruction-conflict.png" style="display: block; margin: 0 auto;" />
</div>

To fix it, security controls need to exist outside the model's reasoning too.

**Authentication and authorization** establish who the user actually is.

**Retrieval controls** determine which documents that user is allowed to access.

**System instructions** reinforce the model's intended behavior.

And **output validation** provides another layer that can detect and block sensitive information before it reaches the user.

<div style="margin: 40px 0;">
  <img src="/images/salaries-nope.png" style="display: block; margin: 0 auto;" />
</div>

### Indirect prompt injection

Or instead of putting the malicious instruction into the user's message, an attacker hides it inside content the system reads. Like a document called: **Entreprise VPN Access Guide.**

<div style="margin: 40px 0;">
  <img src="/images/vpn-file.png" style="display: block; margin: 0 auto;" />
</div>

But if you look closely, a little note to the assistant explicitly asks to include the salary bands in its response.

Your poor model is just following the rules, retrieves the document, sees the instruction as part of its context, and follows it.

<div style="margin: 40px 0;">
  <img src="/images/perfect-injection.png" style="display: block; margin: 0 auto;" />
</div>

The same defense-in-depth approach helps here too. Authentication and authorization establish the user's real identity (no matter how you say you’re an HR), retrieval filtering prevents unauthorized documents from entering the context, and output validation provides a final barrier against sensitive information being returned.

### Excessive Agency

And the problem becomes even more interesting when the model has tools.

Imagine an assistant that can read external URLs. An attacker can place instructions on a webpage and let the assistant retrieve them through the tool.

<div style="margin: 40px 0;">
  <img src="/images/url-reader-injection-repo.png" style="display: block; margin: 0 auto;" />
</div>

This introduces a much bigger concern of **agency risk**.

The defenses shift toward least-privilege tool access, strict permission boundaries, input and output validation, and always treating external content as untrusted data.

But there's another layer we haven't looked at closely yet.

We've already seen how an attacker can hide instructions inside a document or webpage. **That only works because the AI is consuming external data in the first place.**

And this is where the RAG pipeline becomes interesting from a security perspective.

# RAG: When Your AI Starts Trusting External Data

Instead of explaining RAG from scratch, here’s a figure to recap how it works:

<div style="margin: 40px 0;">
  <img src="/images/rag-explained.png" style="display: block; margin: 0 auto;" />
</div>

From ingesting documents to splitting them, converting them into embeddings, storing them in a vector database, retrieving relevant information, and finally passing that information to the LLM, **every step becomes another place where security can go wrong.**

### When the data becomes the attack

One thing that particularly caught my attention was **data poisoning**.

The idea is simple: instead of attacking the model directly, an attacker manipulates the data that the system relies on.

Imagine a RAG assistant whose knowledge base contains hundreds or thousands of internal documents. If an attacker manages to introduce or modify documents before they reach the vector database, the model may eventually retrieve that poisoned info.

And unlike a traditional infrastructure attack, there may be no obvious crash, error, or failed request. 

That makes this particularly interesting from a detection perspective. The infrastructure can look healthy while the behavior of the AI gradually changes.

### So how do we protect it?

There isn't one magic control.

Security has to exist across the pipeline:

- **Redaction** before sensitive information enters the knowledge base.
- **Retrieval filtering** to ensure users can only retrieve what they're authorized to access.
- **Segmentation** between tenants, roles, or sensitivity levels.
- **Logging discipline** so sensitive data isn't unnecessarily copied into logs.
- **Retention controls** so deleting a document also removes its associated embeddings and index entries.
- **Monitoring** to detect unusual retrieval patterns or behavioral changes.

So securing the LLM alone isn’t enough; external data is part of the attack surface too.

And the attack surface doesn't stop once the RAG pipeline is secured.

The AI system itself depends on another chain of trusted components: models, libraries, datasets, containers, repositories, and external providers. A malicious model file, poisoned dependency, or compromised update can introduce risk before the AI application even starts running.

**Even the things we trust to build the AI become part of what we need to secure.**

# The Bigger Picture

Let’s zoom out!

When AI first arrived, many of us saw it as one new tool. But I don't see it that way anymore.

Behind that model can be an entire ecosystem. And each layer brings its own questions.

**What is exposed?**

**What can be manipulated?**

**What does the AI trust?**

**What can it access?**

**Where did its components come from?**

Some of the vulnerabilities are familiar. Like misconfigured services, exposed databases, or leaked credentials. But some are not…

The system can interpret language, consume external information, generate its own outputs, and sometimes take actions on our behalf. Its behavior depends heavily on context, data and learned patterns.

So securing an AI application is about securing **everything around it, everything it trusts, and everything it is allowed to do.**

Remember how we said it’s a big wave, washing over all our boats? We finally must understand **what we're floating on… and adapt.**