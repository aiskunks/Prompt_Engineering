# Prompting Introduction

Prompt engineering is a new discipline that focuses on developing and optimizing prompts to effectively utilize language models (LMs) for various applications and research topics. It involves improving our understanding of the capabilities and limitations of large language models (LLMs), and enhancing their performance on tasks ranging from simple question answering to complex arithmetic reasoning. Developers use prompt engineering to create robust and effective prompting techniques that can interface with LLMs and other tools.

To help beginners get started with using prompts to interact with LLMs, this guide covers the basics of standard prompts. It provides a general idea of how prompts work and how to use them effectively.

All examples are tested with `text-davinci-003` (using OpenAI's playground) unless otherwise specified. It uses the default configurations, e.g., `temperature=0.7` and `top-p=1`.

Topic:
- [Basic Prompts](#basic-prompts)
- [A Word on LLM Settings](#a-word-on-llm-settings)
- [Standard Prompts](#standard-prompts)
- [Prompt Elements](#elements-of-a-prompt)
- [General Tips for Designing Prompts](#general-tips-for-designing-prompts)

---
## General Rules of Thumb 

Prompt engineering is a powerful tool for utilizing language models (LMs), but the quality of results is dependent on the amount of information provided. A prompt can include instructions, questions, and examples to guide the LM's output. It's essential to design effective prompts to instruct the model on a particular task, a practice known as prompt engineering.

The guide provides a comprehensive overview of standard prompts, few-shot prompting, and the different components that make up a prompt, such as instruction, context, input data, and output indicator. The guide also provides general tips for designing effective prompts, such as being specific, avoiding impreciseness, and starting simple.

When working with prompts, you can also adjust the LM settings to control the results, such as temperature and top_p. Finally, the guide encourages experimentation and iteration to optimize prompts for specific applications.
Rules of Thumb and Examples
Note: the "{text input here}" is a placeholder for actual text/context

Use the latest model
For best results, we generally recommend using the latest, most capable models. As of November 2022, the best options are the “text-davinci-003” model for text generation, and the “code-davinci-002” model for code generation.

Put instructions at the beginning of the prompt and use ### or """ to separate the instruction and context
Less effective ❌:

Summarize the text below as a bullet point list of the most important points.

{text input here}

Better ✅:

Summarize the text below as a bullet point list of the most important points.

python
Copy code
Text: """
{text input here}
"""
Be specific, descriptive and as detailed as possible about the desired context, outcome, length, format, style, etc
Be specific about the context, outcome, length, format, style, etc

Less effective ❌:

Write a poem about OpenAI.

Better ✅:

Write a short inspiring poem about OpenAI, focusing on the recent DALL-E product launch (DALL-E is a text to image ML model) in the style of a {famous poet}

Articulate the desired output format through examples (example 1, example 2).
Less effective ❌:

Extract the entities mentioned in the text below. Extract the following 4 entity types: company names, people names, specific topics and themes.

Text: {text}

Better ✅:

Extract the important entities mentioned in the text below. First extract all company names, then extract all people names, then extract specific topics which fit the content and finally extract general overarching themes

Desired format:

yaml
Copy code
Company names: <comma_separated_list_of_company_names>
People names: -||-
Specific topics: -||-
General themes: -||-
Text: {text}

Start with zero-shot, then few-shot (example), neither of them worked, then fine-tune
✅ Zero-shot

Extract keywords from the below text.

Text: {text}

Keywords:

✅ Few-shot - provide a couple of examples

Extract keywords from the corresponding texts below.

Text 1: Stripe provides APIs that web developers can use to integrate payment processing into their websites and mobile applications.
Keywords 1: Stripe, payment processing, APIs, web developers, websites, mobile applications

Text 2: OpenAI has trained cutting-edge language models that are very good at understanding and generating text. Our API provides access to these models and can be used to solve virtually any task that involves processing language.
Keywords 2: OpenAI, language models, text processing, API.

Text 3: {text}
Keywords 3:

✅ Fine-tune: see fine-tune best practices here.

Reduce “fluffy” and imprecise descriptions
Less effective ❌:

The description for this product should be fairly short, a few sentences only, and not too much more.

Better ✅:

Use a 3 to 5 sentence paragraph to describe this product.

Instead of just saying what not to do, say what to do instead
Less effective ❌:

The following is a conversation between an Agent and a Customer. DO NOT ASK USERNAME OR PASSWORD. DO NOT REPEAT.

Customer: I can’t log in to my account.
Agent:

Better ✅:

The following is a conversation between an Agent and a Customer. The agent will attempt to diagnose the problem and suggest a solution, whilst refraining from asking any questions related to PII. Instead of asking for PII,
## Basic Prompts

You can already achieve a lot with prompts, but the quality of results depends on how much information you provide it. A prompt can contain information like the `instruction` or `question` you are passing to the model and include other details such as `inputs` or `examples`. 

Here is a basic example of a simple prompt:

*Prompt*
```
The sky is
```

*Output:*
```
blue

The sky is blue on a clear day. On a cloudy day, the sky may be gray or white.
```

As you can see, the language model outputs a continuation of strings that make sense given the context `"The sky is"`. The output might be unexpected or far from the task we want to accomplish. 

This basic example also highlights the necessity to provide more context or instructions on what specifically we want to achieve.

Let's try to improve it a bit:

*Prompt:*
```
Complete the sentence: 

The sky is
```

*Output:*

```
 so  beautiful today.
```

Is that better? Well, we told the model to complete the sentence so the result looks a lot better as it follows exactly what we told it to do ("complete the sentence"). This approach of designing optimal prompts to instruct the model to perform a task is what's referred to as **prompt engineering**. 

The example above is a basic illustration of what's possible with LLMs today. Today's LLMs can perform all kinds of advanced tasks that range from text summarization to mathematical reasoning to code generation.

---
## A Word on LLM Settings

When working with prompts, you will be interacting with the LLM via an API or directly. You can configure a few parameters to get different results for your prompts. 

**Temperature** - In short, the lower the temperature the more deterministic the results in the sense that the highest probable next token is always picked. Increasing the temperature could lead to more randomness encouraging more diverse or creative outputs. We are essentially increasing the weights of the other possible tokens. In terms of application, we might want to use a lower temperature for something like fact-based QA to encourage more factual and concise responses. For poem generation or other creative tasks, it might be beneficial to increase the temperature. 

**Top_p** - Similarly, with top_p, a sampling technique with temperature called nucleus sampling, you can control how deterministic the model is at generating a response. If you are looking for exact and factual answers keep this low. If you are looking for more diverse responses, increase to a higher value. 

The general recommendation is to alter one, not both.

Before starting with some basic examples, keep in mind that your results may vary depending on the version of LLM you are using. 


---
[Next Section (Basic Prompting)](./prompts-basic-usage.md)
