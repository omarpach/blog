---
title: Curso AWS - IA Generativa
---
# Module 3: Introducing Gen. AI
## Foundation Models

- Based on complex NNs & trained on massive datasets, which allows them to produce *generalized responses* to natual language inputs
- Types of FMs:
	- Language: Generate text-based responses (e.g. GPT)
	- Image: Generate high-resolution images (e.g. [DallE](https://openai.com/es-419/index/dall-e-3/))
	- Audio: Generate music, sounds and speech (e.g. [Stable Audio Open](https://huggingface.co/stabilityai/stable-audio-open-1.0), [Microsoft Sam](https://www.tetyys.com/SAPI4/))
	- Biomedical: Generate protein or molecular structures (e.g. [AlphaFold](https://deepmind.google/science/alphafold/))
- **Prompt:** An instruction (in natural language) given to the Gen. AI model, requesting it to perform a task

## How FMs work

| Class of model                         | Characteristics                                                                                                                              | Common uses                                                                                                                          |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Diffusion Models                       | These models learn to generate new data by modelling the process of adding noise to a clean input and then learning to reverse that process. | - Generation of high-quality, diverse and realistic-looking images<br>- Creation of novel molecular structures<br>- Audio generation |
| Generative adversarial networks (GANs) | Consists of two NNs, one generates data, one classifies real from fake data, and the generative tries to fool the classifying one.           | - Photo-realistic image generation<br>- Img2Img translation and style transfer<br>- Audio generation                                 |
| Variational autoencoders (VAEs)        | VAEs learn a latent representation of input data and generate new samples that resemble the original.                                        | <br>- Img generation<br>- Img interpolation and style transfer<br>- Audio generation                                                 |
| Transformer-based models               | These models can focus on the most relevant parts of an input and pay attention to different parts of the input simultaneously.              | - LLMs, NLP and translation<br>- Multi-modal tasks, such as image captioning                                                         |
	
- How LLMs work
		- Tokenization
		- Embeddings
		- Prediction of outputs based on probability
	- Challenges of gen. models
		- Hallucinations
			- Generate results that look correct but are false
		- Toxicity
			- Generate outputs that include offensive content
		- Bias
			- Models unknowingly perpetuate or amplify societal biases (e.g. racism, sexism) present in training data ([gorilla incident](https://s.wsj.net/public/resources/images/BN-JE720_Google_E_20150701142359.jpg))
	- Inference parameters
		- Make the model more or less 'creative'
		- More deterministic -> Increase accuracy
		- Less deterministic -> Increase creativity
	- They can use external data

## AWS generative AI services
**Three primary services**
![[files/aws-ai-services.png]]
They go alongside the scale of less managed/more control

### Amazon SageMaker AI

- **You have full control over training and deployment pipelines**
- Build, train and deploy ML models, including FMs
- Get access to purpose-built tools for the ML lifecycle, including:
	- training infrastructure
	- governance
	- operations tools
- Access public FMs with Amazon SageMaker Jumpstart

### Amazon Bedrock

- Use established FMs
- Designed to *easily evaluate and test* FMs, so you can find the best one for your needs
- Don't worry about infrastructure
- *Unified API*
- Implement guardrails

### Amazon Q

- Amazon's own deployment of Gen. AI FMs
- Directed at end users
- Built on Bedrock
- Amazon Q Developer: Coding assistant
- Amazon Q Business: Chat assistant

## Generative AI use cases

- **When it is a good idea**
	- Processing and analyzing diverse, unstructured data sources
	- Sy
- **When it is a bad idea**
	- Ethical concerns
	- Accuracy and consistency requirements
	- Explainability or transparency required
	- Lack of high-quality data
	- Unclear business value for cost

# Module 4: Using Prompts and Prompt Engineering

## The value of prompt engineering

**Advantages of prompt engineering:**
- Higher quality outputs
- Reduce costs
- Increase safety measures
- Augment the model capabilities with domain-specific knowledge, by including it *as part of the prompt*, removing the need for additional model training.

## Structuring an LLM prompt

**Prompt elements can be described in two ways:**
- Tell the model *exactly* what to do (Describing the tasks a model should perform)
	- Task description: Clearly define the task/objective
	- Instruction or step-by-step instructions
	- Response format or output characteristics (e.g. establish length, format, etc.)
- Tell the model what you want (Customizing the nature of the response)
	- Input data, context or examples
	- Persona or role, audience, tone or style
	- Constraints or exclusions

**Use consistent formatting**, one example of this is tagging
```XML
<constraints>
...
</constraints>
<format>
...
</format>
<context>
...
</context>
<task>Perform (task description) in the context of <context> and follow the directions included as <constraints>. Format the output as described in <format>. </task>
```

## Prompting techniques

### Zero shot, single-shot and few-shot

![[shot-prompting.png]]

### Tools, actions & exclusions

![[tools-actions-negations-prompt.png]]

### Chain of Thought

Provides *step-by-step instructions*

### Tree of thought

*Extendes Chain of Thought*

### Refine results

- Iterative refinement: Evaluate the output, adjust the prompt and repeat until you get the desired response
- Output selection: Choose from multiple model responses
- Output adjustments: Edit the outputs to achieve the desired result, includes filtering out undesired or not relevant parts of the response, and post-editing the response to achieve the desired results, adhere to a tone, etc.

## The risk of adverse prompts

==**Adverse prompts**: Malicious prompts that intend to alter the behavior of gen. AI systems.==

| Prompt injection                             | Prompt leaking                                       | Jailbreaking                                                | Social Engineering                            |
| -------------------------------------------- | ---------------------------------------------------- | ----------------------------------------------------------- | --------------------------------------------- |
| Embed malicious instructions within a prompt | Get the model to leak information about how it works | Circumvent the constraints and safety measures (guardrails) | Exploit the AI system's attempt to be helpful |
