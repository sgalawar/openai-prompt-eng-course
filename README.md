# Prompt Engineering Course | by OpenAI
___DISCLOSURE: The README in this repository, while comprehensive and educational about the course content, may ironically fail to fulfill its intended role by not exclusively explaining the included code and instead providing an overly verbose course summary.___

__What are these notes?__

- These notes are from the ChatGPT prompt engineering course by DeepLearning.AI. It aims to enhance prompt creation for large language models and explore their capabilities.

__What are the goals of the course material?__

- Develop precise, reliable, and validated NLP prompts for models like ChatGPT.
- Gain deeper insights into the potential and constraints of natural language processing models.
***
## Introduction
__What is a Large Language Model (LLM)?__
- It is an AI that produces human-like text from the data it's trained on. Although it can't understand or hold opinions (for now), it's helpful for tasks like writing, coding, and language learning.

#### Two types of Large Language Models (LLM)
- __Base LLM__: Predicts next word based on the training data.
```
 Example 1 - Prompt: Once upon a time, there was a unicorn | Answer: that lived in a magical forest with all her unicorn friends.
```
Or
```
Example 2 - Prompt: What is the capital of France? | Answer: What is France's largest city? What is France's population? What is the currency of France?
```
- __Instruction Tuned LLM__: Tries to follow indstructions. Fine-tune on instructions and good attempts at following those instructions. RLHF: Reinforcement Learning with Human Feedback. Helpful, honest, harmless.
```
Example: Prompt: What is the capital of France? | Answer: The capital of France is Paris.
```
***
## Guidelines for Prompting
This is the __setup__ to load the API key and relevant Python libraries.

To install the OpenAI Python library:
```python
!pip install openai
```
The library needs to be configured with your account's secret key, which is available on the [website](https://platform.openai.com/account/api-keys).

You can either set it as the __OPENAI_API_KEY__ environment variable before using the library:
```python
!export OPENAI_API_KEY='sk-...'
```
Or, set __openai.api_key__ to its value:
```python
import openai
openai.api_key = "sk-..."
```
This code __loads the OpenAI API key__ for you:
```python
import openai
import os

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv())

openai.api_key  = os.getenv('OPENAI_API_KEY')
```

And this is a __helper function__ that makes it easier to use prompts and look at the generated outputs:

```python
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
```

### Prompting Principles
- __Principle 1: Write clear and specific instructions.__
- __Principle 2: Give the model time to "think".__

Here are some tactics to help you achieve these principles:

#### For principle 1:

- __Tactic 1__: __Use delimiters__ to clearly indicate distinct parts of the input. Delimiters can be anything: ```, """, < >, <tag> </tag>, :.
- __Tactic 2__: Ask for a __structured__ output. JSON, HTML. Example: _Provide them in JSON format with the following keys: book_id, title, author, genre._
- __Tactic 3__: Ask the model to check whether __conditions__ are satisfied. Example: _If the text does not contain X then do Y_.
- __Tactic 4__: "Few-shot" prompting, where the AI is given a few __examples__ of a task before performing a similar task. Example: _Your task is to answer in a consistent style. X: blah. Y: blah blah blah. X: blah blah._

#### For principle 2:

- __Tactic 1__: Specify the steps required to complete a task. Example: _Perform the following actions: 1- Summarize blah. 2- Translate bla blah blah. 3- List and output blah blah._

- __Tactic 2__: Ask for output in a specified format. Example: _Text: 'text to summarize', Summary: 'summary', Translation: 'summary translation', Names: 'list of names'._

- __Tactic 3__: Instruct the model to work out its own solution before rushing to a conclusion.

### Model Limitations: Hallucinations
Hallucinations in AI refer to the generation of __plausible but incorrect or unverifiable information__ by the model. Example: _If you ask an AI model, "What color is a Math Fairy's dress?" and the AI responds with "A Math Fairy's dress is typically blue with equations written all over it.", this would be considered a hallucination. In reality, a "Math Fairy" does not exist, so the AI is inventing or "hallucinating" this information._
***
## Iterative Prompt Development
This concept means __to continuously improve the instructions__ given to the language model to get better results.

### Issues a user my encounter
- __Issue 1__: The text retrieved is __too long__.
    - Solution: __Limit__ the number of words/sentences/characters. Example: "_Your task is X. Use at most 50 words._"
- __Issue 2__: The text retrieved focuses on the __wrong details__.
    - Solution: Include in your prompt to __focus__ on the aspects that are __relevant__ to the __intended audience__. Example: "_Your task is X. The X is intended for Y audience, so X should focus on Z._"
- __Issue 3__: The text retrieved requires __more specific__ information.
    - Solution: Be __specific__ about the output you want.
***
## Summarizing
- Request to summarize with a __word limit__. Example: _Summarize the following text, delimited by curly braces, in at most 30 words. Review: { text to summarize }._
- Summarize with a __focus__ on something specific. Example: _Summarize X, and focus on Y._
- Depending on how you want your retrieved text, you can try using the word __"extract"__ instead of "summarize" to get information that is more mostly relevant to the input text.
- Use a __for loop__ to summarize multiple long texts in succession. Example:
```python
for i in range(len(reviews)):
    prompt = f"""
    Your task is to generate a short summary of a product \ 
    review from an ecommerce site. 

    Summarize the review below, delimited by triple \
    backticks in at most 20 words. 

    Review: ```{reviews[i]}```
    """

    response = get_completion(prompt)
    print(i, response, "\n")
```
***
## Acknowledgements
 - Shout out to my friend [Gabriel Martinica](github.com/Gmartinica) for providing me with some of his notes from the [course](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/).

