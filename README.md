# diffusion-transformers-llm
This will be a small scale proof-of-concept of an encoder-decoder transformer based large language model that outputs via diffusion rather than sequentially generating tokens.

## Table of Contents
* Architecture
* Encoder
  * Dynamic Dimensionality
* Language Diffusion
  * Nested Neural Nets
  * A 'Convolutional' LLM

## Architecture
Transformer based encoder-decoder architecture.

## Encoder
### Dynamic Dimensionality
In linguistics and information theory, different words have properties like [Shannon Entropy](https://en.wikipedia.org/wiki/Entropy_(information_theory)) and [Mutual Information](https://en.wikipedia.org/wiki/Mutual_information), which have implications about the meanings of words in different contexts, as well as the fact that some words simply contain more information than others. Because of this, it intuitively makes sense that in a language embedding, words with less entropy, mutual information, and information generally, might not need to make use of all dimensions in the embedding space. Such words may lack a litany of different relationship types that other words have, and could have many of zero values in its vector.

An optimal embedding and encoding layer should both encourage this type of structure to form. This project will place a high focus on an extremely high quality and robust embedding space.

To achieve this, when training the embedding matrix I will employ [Regularization](https://en.wikipedia.org/wiki/Regularization_(mathematics)) through weight decay and [Dropout](https://en.wikipedia.org/wiki/Dilution_(neural_networks)) directly on the values of the embedding matrix during training. This will encourage tokens vectors to reduce the number of dimensions they use as long as it doesn't harm performance, and it will simultaneously encourage robust representations that don't overly rely on specific dimensions. These methods will also be used on the encoding layer.

## Language Diffusion
### Nested Neural Nets
Starting with a random block of noise vectors in the embedding space, a function will iteratively refine this noise until it outputs a coherent response all at once. To achieve this, at each iteration the model will use both the prompt and current state of the noise as input to output the parameters for a small neural network. The neural network will make small modifications the noisy vectors so that they slowly move toward areas in the embedding that correspond to a good response. The model will simultaneously learn attention scores for the noise, and de-noise it at the same time.

### A 'Convolutional' LLM
Rather than using the de-noising function to modify each noisy token vector individually at each step, they could be modified in groups like a [convolutional network](https://en.wikipedia.org/wiki/Convolutional_neural_network). The small network could denoise a group of noise vectors at a time, then 'slide' over the full block similar to how a kernal slides over an image in an image model. The function would do a full slide over the output noise for each iteration.



