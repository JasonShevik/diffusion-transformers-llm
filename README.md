# diffusion-transformers-llm
This will be a small scale proof-of-concept of a decoder-only transformer based large language model that outputs via diffusion rather than sequentially generating tokens. As a proof of concept, I will start with training a model to auto complete the next 50 or so tokens from an input of 100 to 200 tokens.

## Table of Contents
* Architecture
* De-noising Function
* A 'Convolutional' LLM
* Advanced Training Methods

## Architecture
Transformer based decoder-only architecture.

## De-Noising Function
When prompted, the model will create a random block of noise vectors in the token embedding space, then iteratively refine this noise until it outputs a coherent response all at once. To achieve this, at each iteration the model will output the parameters for a function to apply to the noisy vectors so that they slowly move toward areas in the embedding that correspond to a good response. This function should be non-linear and should affect each vector differently based on the input prompt and the attention mechanisms (self-attention, cross-attention).

### Nested Neural Nets
One option for this function is a neural network. The main model, at each iteration, would output the parameters for a very small neural network to transform the noise.

### Fourier Series
Another option is a Fourier series. If an embedded token has 'n' dimensions, then a single interval of the function would go from 0 to n-1. Define a vector whose values are the value of the Fourier series at the integer X values, then subtract that vector from the noisy vector.

## A 'Convolutional' LLM
Rather than using the de-noising function to modify each noisy token vector individually at each step, they could be modified in groups like a [convolutional network](https://en.wikipedia.org/wiki/Convolutional_neural_network). A function could denoise a group of 't' tokens at a time, then the function would 'slide' over the noisy output similar to how a kernal slides over an image in an image model. The function would do a full slide over the output noise for each iteration. The number of tokens to transform at a time could change at each iteration, or it could be based on some linguistic data about patterns in language.

## Advanced Training Methods
### Code Switching
Current LLMs struggle with [code switching](https://en.wikipedia.org/wiki/Code-switching) where three or more languages are used within a single text, this is despite the fact that they excel at translation more broadly and a single model can understand hundreds of different languages. I believe this is due to [over-fitting](https://en.wikipedia.org/wiki/Overfitting) and memorization. The models are not trained on data that constantly switches between languages, so they do not generalize their knowledge, but rather, I believe, silo away memorized knowledge for each language leading to a situation where a model may know things in one langauge but not another. Memorization is an enormous issue for LLMs. Recent research, including [this paper](https://www.nature.com/articles/s41586-023-06668-3), shows how diversifying training to include "gibberish" languages in the context of logic puzzles increases a models reasoning ability. Creating thousands of puzzles like these to sprinkle into training seems unfeasible. Rather, this kind of training that forces generalization would ideally be implicit in all training.

I believe that training a model on extreme code-switching text would result in models learning implicit generalization. If every time the model encounters a fact it is represented with a different combination of different languages in a different order then it will be forced to learn the underlying meaning of these facts rather than memorizing text that correctly explains it. The model could then be fine-tuned to speak only one language at a time.

I will attempt to build a small dataset with extreme code switching using Wikipedia. There are thousands of pages that are available in hundreds of languages. For example, [this page](https://en.wikipedia.org/wiki/Blue) on the color blue is available in 162 different langauges, and I would create a new page for the color that uses all 162 languages randomly throughout the article. This should ideally be higher quality data than training on all 162 pages separately.

