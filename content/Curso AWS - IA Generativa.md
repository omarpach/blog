---
title: Curso AWS - IA Generativa
---
# Modulo 3: Introducing Gen. AI
- **Foundation Models**
	- Based on complex NNs & trained on massive datasets, which allows them to produce *generalized responses* to natual language inputs
	- Types of FMs:
		- Language: Generate text-based responses (e.g. GPT)
		- Image: Generate high-resolution images (e.g. [DallE](https://openai.com/es-419/index/dall-e-3/))
		- Audio: Generate music, sounds and speech (e.g. [Stable Audio Open](https://huggingface.co/stabilityai/stable-audio-open-1.0), [Microsoft Sam](https://www.tetyys.com/SAPI4/))
		- Biomedical: Generate protein or molecular structures (e.g. [AlphaFold](https://deepmind.google/science/alphafold/))
	- **Prompt:** An instruction (in natural language) given to the Gen. AI model, requesting it to perform a task
- How FMs work

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