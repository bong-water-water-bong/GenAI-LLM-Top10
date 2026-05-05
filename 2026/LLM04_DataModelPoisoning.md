## LLM04:2025 Data and Model Poisoning

### Description

Data & Model Poisoning describes a class of attacks and failures where an adversary (or unsafe process) manipulates data or model artifacts to embed harmful behavior, bias, or exploitable weaknesses into an AI system. In modern GenAI environments, poisoning is not limited to “training data” in the traditional sense it can occur anywhere data is ingested, transformed, retrieved, or reused, including during pre-training, fine-tuning, embedding creation, retrieval augmentation (RAG), and model distribution. The result is an AI system that may still appear functional but behaves in ways that undermine trust, safety, and security.
Data poisoning occurs when pre-training, fine-tuning, or embedding data is tampered with to introduce vulnerabilities, backdoors, or biases. This can happen intentionally (malicious poisoning) or unintentionally (poor data hygiene, contaminated sources). The manipulation compromises model integrity and the model learns the wrong patterns, internalizes malicious correlations, or is conditioned to behave incorrectly under certain conditions. The consequences include harmful outputs, impaired capabilities, and degraded reliability.
The key idea: poisoning targets the model’s “learning process,” not a single runtime bug. Unlike typical software vulnerabilities that can be patched by fixing code, poisoning can require data revalidation, retraining, model replacement, or pipeline redesign making it expensive and operationally disruptive.Poisoning can occur across multiple stages of the LLM lifecycle like:
Pre-training: The model learns from broad, large-scale corpora. If a portion of that corpus is maliciously crafted or contaminated, the model may absorb harmful patterns, unsafe instructions, or skewed representations.
Fine-tuning: Models are adapted for specific tasks or domains (e.g., customer support, coding assistants, financial Q&A). If fine-tuning datasets contain manipulated samples, the model can inherit domain-specific failure modes or hidden triggers.
Embeddings and vectorization: Text is converted into vectors for search and retrieval. Poisoning can target embedding generation data (or the stored vectors) to influence what content is retrieved, resulting in “steered” answers or subtle misinformation.
Transfer learning / model reuse: Organizations frequently reuse pre-trained models or community models. If the source model is compromised, downstream systems inherit that compromise.
Continuous learning / retraining pipelines: Some systems update with new data over time. If ingestion is automated and insufficiently validated, attackers can feed poisoned data into the loop and gradually shape model behavior.
Understanding these stages is essential because it clarifies where vulnerabilities originate and how they propagate.
Data poisoning surface expands because organizations increasingly rely on:
-External datasets (public sources, scraped content, third-party corpora)
-RAG and embeddings (vector databases, retrieval pipelines, document ingestion)
-Shared models and open repositories (community weights, fine-tunes, adapters)
-Agentic workflows and automation (AI controlling tasks, actions, and tool use)
.

### Common Examples of Vulnerability

1. Malicious actors introduce harmful data during training, leading to biased outputs. Techniques like "Split-View Data Poisoning" or "Frontrunning Poisoning" exploit model training dynamics to achieve this.
  (Ref. link: [Split-View Data Poisoning](https://github.com/GangGreenTemperTatum/speaking/blob/aad68f8521119596abb567d94fbd10bdd652ac82/docs/conferences/dc604/hacker-summer-camp-23/Ads%20_%20Poisoning%20Web%20Training%20Datasets%20_%20Flow%20Diagram%20-%20Exploit%201%20Split-View%20Data%20Poisoning.jpeg))
  (Ref. link: [Frontrunning Poisoning](https://github.com/GangGreenTemperTatum/speaking/blob/aad68f8521119596abb567d94fbd10bdd652ac82/docs/conferences/dc604/hacker-summer-camp-23/Ads%20_%20Poisoning%20Web%20Training%20Datasets%20_%20Flow%20Diagram%20-%20Exploit%202%20Frontrunning%20Data%20Poisoning.jpeg))
1. Attackers can inject harmful content directly into the training process, compromising the model’s output quality.
2. Users unknowingly inject sensitive or proprietary information during interactions, which could be exposed in subsequent outputs.
3. Unverified training data increases the risk of biased or erroneous outputs.
4. Lack of resource access restrictions may allow the ingestion of unsafe data, resulting in biased outputs.

### Prevention and Mitigation Strategies

1. Track data origins and transformations using tools like OWASP CycloneDX or ML-BOM and leverage tools such as [Dyana](https://github.com/dreadnode/dyana) to perform dynamic analysis of third-party software. Verify data legitimacy during all model development stages.
2. Vet data vendors rigorously, and validate model outputs against trusted sources to detect signs of poisoning.
3. Implement strict sandboxing to limit model exposure to unverified data sources. Use anomaly detection techniques to filter out adversarial data.
4. Tailor models for different use cases by using specific datasets for fine-tuning. This helps produce more accurate outputs based on defined goals.
5. Ensure sufficient infrastructure controls to prevent the model from accessing unintended data sources.
6. Use data version control (DVC) to track changes in datasets and detect manipulation. Versioning is crucial for maintaining model integrity.
7. Store user-supplied information in a vector database, allowing adjustments without re-training the entire model.
8. Test model robustness with red team campaigns and adversarial techniques, such as federated learning, to minimize the impact of data perturbations.
9. Monitor training loss and analyze model behavior for signs of poisoning. Use thresholds to detect anomalous outputs.
10. During inference, integrate Retrieval-Augmented Generation (RAG) and grounding techniques to reduce risks of hallucinations.

### Example Attack Scenarios

#### Scenario #1

  An attacker biases the model's outputs by manipulating training data or using prompt injection techniques, spreading misinformation.

#### Scenario #2

  Toxic data without proper filtering can lead to harmful or biased outputs, propagating dangerous information.

#### Scenario #3

  A malicious actor or competitor creates falsified documents for training, resulting in model outputs that reflect these inaccuracies.

#### Scenario #4

  Inadequate filtering allows an attacker to insert misleading data via prompt injection, leading to compromised outputs.

#### Scenario #5

  An attacker uses poisoning techniques to insert a backdoor trigger into the model. This could leave you open to authentication bypass, data exfiltration or hidden command execution.

### Reference Links

1. [How data poisoning attacks corrupt machine learning models](https://www.csoonline.com/article/3613932/how-data-poisoning-attacks-corrupt-machine-learning-models.html): **CSO Online**
2. [MITRE ATLAS (framework) Tay Poisoning](https://atlas.mitre.org/studies/AML.CS0009/): **MITRE ATLAS**
3. [PoisonGPT: How we hid a lobotomized LLM on Hugging Face to spread fake news](https://blog.mithrilsecurity.io/poisongpt-how-we-hid-a-lobotomized-llm-on-hugging-face-to-spread-fake-news/): **Mithril Security**
4. [Poisoning Language Models During Instruction](https://arxiv.org/abs/2305.00944): **Arxiv White Paper 2305.00944**
5. [Poisoning Web-Scale Training Datasets - Nicholas Carlini | Stanford MLSys #75](https://www.youtube.com/watch?v=h9jf1ikcGyk): **Stanford MLSys Seminars YouTube Video**
6. [ML Model Repositories: The Next Big Supply Chain Attack Target](https://www.darkreading.com/cloud-security/ml-model-repositories-next-big-supply-chain-attack-target) **OffSecML**
7. [Data Scientists Targeted by Malicious Hugging Face ML Models with Silent Backdoor](https://jfrog.com/blog/data-scientists-targeted-by-malicious-hugging-face-ml-models-with-silent-backdoor/) **JFrog**
8. [Backdoor Attacks on Language Models](https://towardsdatascience.com/backdoor-attacks-on-language-models-can-we-trust-our-models-weights-73108f9dcb1f): **Towards Data Science**
9. [Never a dill moment: Exploiting machine learning pickle files](https://blog.trailofbits.com/2021/03/15/never-a-dill-moment-exploiting-machine-learning-pickle-files/) **TrailofBits**
10. [arXiv:2401.05566 Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training](https://www.anthropic.com/news/sleeper-agents-training-deceptive-llms-that-persist-through-safety-training) **Anthropic (arXiv)**
11. [Backdoor Attacks on AI Models](https://www.cobalt.io/blog/backdoor-attacks-on-ai-models) **Cobalt**

### Related Frameworks and Taxonomies

Refer to this section for comprehensive information, scenarios strategies relating to infrastructure deployment, applied environment controls and other best practices.

- [AML.T0018 | Backdoor ML Model](https://atlas.mitre.org/techniques/AML.T0018) **MITRE ATLAS**
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework): Strategies for ensuring AI integrity. **NIST**
- [ML07:2023 Transfer Learning Attack](https://owasp.org/www-project-machine-learning-security-top-10/docs/ML07_2023-Transfer_Learning_Attack) **OWASP Machine Learning Security Top Ten**
