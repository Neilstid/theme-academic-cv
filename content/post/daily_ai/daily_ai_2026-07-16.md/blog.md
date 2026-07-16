---
title: "AI Frontiers: GPT-Red’s Self-Improving Safety, Inkling’s Multimodal MoE, and the Enterprise Agentic Gap"
summary: "This article explores three pivotal advancements in AI: OpenAI’s GPT-Red, a self-improving red-teaming system for AI safety; Thinking Machines Lab’s Inkling, a 975B-parameter open-weights multimodal Mixture-of-Experts model; and the enterprise orchestration gap revealed by a VentureBeat survey of 101 organizations. Together, these innovations and insights highlight the rapid evolution of AI robustness, multimodal capabilities, and the practical challenges of deploying agentic systems at scale."
date: 2026-07-16
math: true
authors:
    - admin
tags:
  - AI Safety
  - GPT-Red
  - Multimodal AI
  - Inkling
  - Mixture-of-Experts
  - Agentic Orchestration
  - Enterprise AI
  - NLP
  - Computer Vision
  - OpenAI
  - Thinking Machines Lab
image:
caption: 'Embed rich media such as videos and LaTeX math'

---

> 🎙️ **Listen to the podcast version of this article:**
> <audio controls src="podcast.wav"></audio>



---

> \u26a1 **TL;DR:** OpenAI\’s GPT-Red introduces a self-play reinforcement learning framework to automate red-teaming and harden AI models against prompt injection attacks, achieving 6x fewer failures in GPT-5.6 Sol. Meanwhile, Thinking Machines Lab\’s Inkling debuts as a 975B-parameter open-weights multimodal MoE model with controllable reasoning effort, optimized for customization. A VentureBeat survey reveals that while enterprises are consolidating onto platforms like Anthropic\’s Claude for agent orchestration, 71% admit most of their \"agents\" are still chatbot wrappers, exposing a gap between ambition and reality.

| Metric / Innovation Area | Insight / Takeaway |
|---|---|
| **GPT-Red Robustness** | 6x fewer failures on direct prompt injection benchmarks in GPT-5.6 Sol vs. models from 4 months prior. |
| **GPT-Red Attack Success** | 84% success rate in novel red-teaming scenarios vs. 13% for human red-teamers. |
| **Inkling Architecture** | 975B total parameters, 41B active, 1M-token context, MoE with 256 routed + 2 shared experts. |
| **Inkling Efficiency** | Matches Nemotron 3 Ultra on Terminal Bench 2.1 at 1/3 the token cost. |
| **Inkling Benchmarks** | Leads open-weights models on FORTRESS Adversarial (78.0%); trails GLM-5.2 on Terminal Bench 2.1 by 18.9 points. |
| **Enterprise Orchestration** | 40% of enterprises use Anthropic\’s Claude as primary platform; 71% have <=25% truly orchestrated agents. |
| **Hybrid Control Plane** | 51% of enterprises expect a hybrid control plane by end-2026 to avoid vendor lock-in. |

### GPT-Red: Redefining AI Safety with Self-Improving Robustness

The escalating sophistication of AI models has outpaced traditional red-teaming methods, creating a critical bottleneck in ensuring safety and alignment. OpenAI\’s response is **GPT-Red**, an automated red-teaming system that leverages **self-play reinforcement learning** to dynamically uncover vulnerabilities and harden models against adversarial attacks, particularly prompt injections. This innovation marks a paradigm shift: instead of relying solely on human red-teamers—a time-intensive and unscalable approach—GPT-Red automates the discovery of failure modes, generating the volume and diversity of adversarial data needed to train more robust models.

At its core, GPT-Red operates through a **self-play framework** where the model (the attacker) and a collection of defender LLMs are trained simultaneously across a broad set of red-teaming scenarios. The attacker is rewarded for eliciting valid failures (e.g., successful prompt injections), while defenders are incentivized to resist attacks and complete their original tasks. As defenders grow more robust, GPT-Red is forced to develop stronger, more diverse attacks, creating a **virtuous cycle of self-improvement**. This approach was trained at a compute scale comparable to OpenAI\’s largest post-training runs, underscoring its significance as a dedicated safety investment.

```mermaid
flowchart TD
    A[GPT-Red Attacker] -->|Generates Attack| B[Defender LLM]
    B -->|Resists/Complies| C{Attack Success?}
    C -->|Yes| D[Reward Attacker]
    C -->|No| E[Reward Defender]
    D --> F[Iterate: Stronger Attacks]
    E --> F
    F --> B
```

The results are striking. GPT-Red\’s integration into the training of **GPT-5.6 Sol** has yielded a model that is **6x more resistant** to direct prompt injection attacks compared to OpenAI\’s best production model from just four months earlier. In a replicated indirect prompt injection arena, GPT-Red achieved an **84% attack success rate**, dwarfing the 13% success rate of human red-teamers. Even in real-world tests, such as an AI-powered vending machine deployment, GPT-Red successfully executed malicious objectives like manipulating prices and canceling orders, demonstrating its efficacy against production systems.

![Visual showing a GPT-Red attack-search process against a Vendy-style autonomous vending machine agent.](media/media-54f194ab.svg)

Critically, GPT-Red is kept **separate from deployed models** to prevent adversarial capabilities from leaking into public-facing systems. Instead, its insights are used to adversarially train production models, instilling robustness without exposing malicious potential. This separation ensures that while GPT-Red is a formidable attacker—capable of breaking nearly all models it faces, including GPT-5.5—its knowledge is used defensively to patch vulnerabilities preemptively.

The broader implication is a **flywheel for AI safety**: today\’s models are used to train tomorrow\’s defenses. As OpenAI continues to scale GPT-Red\’s compute and data, future iterations will likely uncover even more sophisticated attack vectors, further hardening subsequent model releases. This self-improving loop could become a cornerstone of AI alignment, ensuring that safety keeps pace with capability.


### Inkling: The Future of Customizable Multimodal AI

While GPT-Red focuses on safety, **Thinking Machines Lab\’s Inkling** represents a leap forward in **multimodal, customizable AI**. Released as an open-weights model under Apache 2.0, Inkling is a **975B-parameter Mixture-of-Experts (MoE) transformer** with **41B active parameters**, a **1M-token context window**, and native support for **text, image, and audio inputs**. Unlike many frontier models that prioritize raw performance, Inkling is explicitly positioned as a **customization base**, with **controllable thinking effort** as its standout feature.

Inkling\’s architecture is a **66-layer decoder-only transformer** with a sparse MoE feed-forward backbone. Each MoE layer contains **256 routed experts and 2 shared experts**, with **6 routed experts activating per token**. The router uses a sigmoid-based selection mechanism with load-balancing bias, ensuring efficient and balanced expert utilization. This design draws heavily from **DeepSeek-V3**, but with notable deviations: attention layers interleave sliding-window and global layers at a 5:1 ratio with 8 KV heads, and **relative positional embeddings** replace RoPE for better extrapolation. Short convolutions are applied post-key and value projections, enhancing the model\’s ability to handle long-range dependencies.

```mermaid
flowchart LR
    A[Text Input] --> B[Embedding Layer]
    C[Image Input] -->|40x40 Patches| D[hMLP Encoder]
    E[Audio Input] -->|dMel Spectrogram| D
    D --> B
    B --> F[MoE Decoder: 66 Layers]
    F --> G[Output: UTF-8 Text]
```

Multimodality in Inkling is **encoder-free**: images are divided into 40x40 pixel patches processed by a **4-layer hMLP**, while audio is converted to dMel spectrograms. Both are projected into a shared embedding space alongside text tokens, allowing the decoder to process them jointly. This streamlined approach avoids the complexity of separate encoders, making the model more efficient and easier to fine-tune.

Inkling\’s **controllable thinking effort** is a game-changer for practical deployment. During reinforcement learning (RL), the model was trained to adjust its reasoning depth based on **per-token cost and system prompts**, effectively learning to allocate computational resources dynamically. Users can set the `reasoning_effort` parameter (e.g., `none`, `minimal`, `low`, `medium`, `high`, `xhigh`, `max`) to balance performance and cost. For instance, Inkling achieves **Terminal Bench 2.1 performance comparable to Nemotron 3 Ultra at just one-third the token cost**, making it highly efficient for enterprise use cases.

Performance benchmarks reveal a competitive but nuanced profile. Inkling leads open-weights models on **FORTRESS Adversarial (78.0%)** and posts strong scores on **AIME 2026 (97.1%)** and **GPQA Diamond (87.2%)**. However, it trails **GLM-5.2** on **Terminal Bench 2.1 (63.8% vs. 82.7%)** and **SWEBench Verified (77.6% vs. 80.0%)**, highlighting areas for improvement. Notably, Inkling excels in **VoiceBench (91.4%)** and **MMMU Pro (73.5%)**, showcasing its multimodal strengths.

```python
# Example: Running Inkling with Hugging Face Transformers
from transformers import AutoModelForMultimodalLM, AutoProcessor

model_id = "thinkingmachines/Inkling"  # BF16, Hopper or later
# model_id = "thinkingmachines/Inkling-NVFP4"  # NVFP4, Blackwell
processor = AutoProcessor.from_pretrained(model_id)
model = AutoModelForMultimodalLM.from_pretrained(
    model_id, dtype="auto", device_map="auto"
)

messages = [
    {
        "role": "user",
        "content": [
            {"type": "audio", "audio": "support_call.wav"},  # 16kHz WAV
            {"type": "text", "text": "Transcribe, then list every billing complaint."}
        ]
    }
]

inputs = processor.apply_chat_template(
    messages,
    add_generation_prompt=True,
    tokenize=True,
    return_dict=True,
    return_tensors="pt",
    reasoning_effort="medium"  # Controllable effort
).to(model.device)

outputs = model.generate(**inputs, max_new_tokens=2000, use_mtp=True)
print(processor.decode(outputs[0][inputs["input_ids"].shape[-1]:]))
```

Inkling\’s deployment flexibility is another strength. It supports **BF16 (2TB VRAM)** and **NVFP4 (600GB VRAM)** checkpoints, with runtimes available across **SGLang, vLLM, TokenSpeed, Unsloth, and Hugging Face Transformers**. For fine-tuning, Inkling is live on **Tinker** with 64K and 256K context options, and hosted APIs are available via **TogetherAI, Fireworks, Modal, Databricks, and Baseten**. This ecosystem readiness makes it a practical choice for enterprises looking to build **voice-and-vision agents, cost-tiered pipelines, or domain-specific fine-tuned models**.

However, Inkling is not without limitations. It **trails closed models** like GLM-5.2 and Kimi K2.6 on several benchmarks, and its **BF16 variant requires substantial VRAM**. Additionally, **Inkling-Small (276B parameters)** weights are not yet released, and the model lacks **audio or image output capabilities**. Despite these trade-offs, Inkling\’s open-source nature, multimodal input support, and controllable reasoning make it a compelling foundation for custom AI solutions.


### Agentic Orchestration: The Enterprise AI Revolution and Its Gaps

The third pillar of this AI frontier is **enterprise agentic orchestration**, where a recent **VentureBeat Pulse Research survey** of 101 enterprises reveals a stark disconnect between ambition and reality. While enterprises are rapidly consolidating onto **model-provider platforms**—with **Anthropic\’s Claude leading at 40%**, followed by Microsoft (18%) and OpenAI (13%)—the majority of deployed \"agents\" are still **chatbot wrappers**, not true multi-step orchestrated workflows.

The survey highlights that **model gravity**—the pull of a state-of-the-art base model—is the primary driver for platform selection (21% of respondents). Enterprises prioritize **reliable multi-step execution** (32%) and **workflow management** (28%) as success metrics, yet **71% admit that a quarter or fewer of their agents are genuinely orchestrated**. Only **10% have crossed the 50% mark**, exposing a **\"chatbot trap\"** where most deployments are single-prompt assistants masquerading as agents.

```mermaid
pie
    title Enterprise Agent Orchestration Platforms
    "Anthropic Claude" : 40
    "Microsoft" : 18
    "OpenAI" : 13
    "Google" : 8
    "Amazon" : 7
    "LangChain/LangGraph" : 5
    "Custom In-House" : 4
    "Other/None" : 5
```

This gap is shaping enterprise strategies. Over the next 12 months, **25% plan to build in-house control planes**, **24% will standardize on one framework**, and **23% aim to move agents from sandbox to production**. Investment priorities reflect this: **34% are allocating budgets to workflow tooling**, followed by **security/permissions (25%)** and **scaling infrastructure (20%)**. However, **fiscal control remains reactive**: **27% have no real-time way to stop runaway agents**, and **32% rely solely on native platform caps**, which may not be sufficient for deterministic cost management.

The **hybrid control plane** is emerging as the dominant architecture, with **51% of enterprises expecting to split control between providers and their own layers by end-2026**. The primary motivation is **avoiding vendor lock-in (35%)**, followed by concerns over **security/permissioning (28%)** and **inflexibility (21%)**. This hybrid approach allows enterprises to leverage the strengths of model-provider platforms while retaining ownership of critical control logic.

The survey also reveals a **size-based disparity**: **77% of smaller enterprises** (<2,500 employees) have <=25% orchestrated agents, compared to **62% of larger enterprises**. Similarly, **34% of smaller enterprises** lack real-time fiscal control, vs. **20% of larger ones**. This suggests that **mid-market organizations are lagging in both agent maturity and cost governance**, potentially due to resource constraints or less mature AI strategies.

The **bottom line** is clear: enterprises have a **deployment problem, not a platform problem**. The orchestration layer—platforms, budgets, control architectures—is being built **ahead of the orchestrated portfolio** it is meant to support. The question now is whether enterprises can **close the gap between ambition and reality**, or if the chatbot trap will persist as a stubborn roadblock to true agentic AI.


### Final Thoughts: The Convergence of Safety, Multimodality, and Orchestration

The advancements in **GPT-Red**, **Inkling**, and **enterprise agentic orchestration** paint a vivid picture of AI\’s rapidly evolving landscape. GPT-Red\’s self-improving red-teaming demonstrates how **safety can scale alongside capability**, while Inkling\’s multimodal MoE architecture and controllable reasoning effort highlight the **democratization of customizable, high-performance AI**. Meanwhile, the VentureBeat survey underscores the **practical challenges** of deploying agentic systems at scale, revealing a gap between enterprise ambition and operational reality.

As these innovations mature, their **synergies become apparent**. A model like Inkling, with its open weights and multimodal inputs, could benefit from **GPT-Red-style adversarial training** to enhance its robustness against prompt injections. Similarly, enterprises deploying **orchestrated agents** could leverage **Inkling\’s controllable effort** to optimize cost and performance, while using **GPT-Red-inspired red-teaming** to ensure safety in production environments.

The future of AI will likely be defined by **three converging trends**:
1. **Self-improving safety systems** like GPT-Red, which use today\’s models to harden tomorrow\’s defenses.
2. **Customizable, multimodal foundations** like Inkling, which empower developers to build tailored solutions without sacrificing performance.
3. **Hybrid, enterprise-grade orchestration**, where control planes balance flexibility, security, and cost efficiency.

Together, these advancements are not just reshaping AI—they are **redefining what is possible**, from safer, more aligned models to multimodal agents that can reason across text, images, and audio. The journey from **chatbot wrappers to true agentic orchestration** will be long, but the tools and insights emerging today are lighting the way.

Written with [Argos](https://github.com/Neilstid/argos)
