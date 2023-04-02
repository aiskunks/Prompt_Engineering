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
## Standard Prompts

We have tried a very simple prompt above. A standard prompt has the following format:

```
<Question>?
```
 
This can be formatted into a QA format, which is standard in a lot of QA dataset, as follows:

```
Q: <Question>?
A: 
```

Given the standard format above, one popular and effective technique for prompting is referred to as few-shot prompting where we provide exemplars. Few-shot prompts can be formatted as follows:

```
<Question>?
<Answer>

<Question>?
<Answer>

<Question>?
<Answer>

<Question>?

```


And you can already guess that its QA format version would look like this:

```
Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A: <Answer>

Q: <Question>?
A:
```

Keep in mind that it's not required to use QA format. The format depends on the task at hand. For instance, you can perform a simple classification task and give exemplars that demonstrate the task as follows:

*Prompt:*
```
This is awesome! // Positive
This is bad! // Negative
Wow that movie was rad! // Positive
What a horrible show! //
```

*Output:*
```
Negative
```

Few-shot prompts enable in-context learning which is the ability of language models to learn tasks given only a few examples. We will see more of this in action in the upcoming guides.

---
## Elements of a Prompt

As we cover more and more examples and applications that are possible with prompt engineering, you will notice that there are certain elements that make up a prompt. 

A prompt can contain any of the following components:

**Instruction** - a specific task or instruction you want the model to perform

**Context** - can involve external information or additional context that can steer the model to better responses

**Input Data** - is the input or question that we are interested to find a response for

**Output Indicator** - indicates the type or format of the output.

Not all the components are required for a prompt and the format depends on the task at hand. We will touch on more concrete examples in upcoming guides.

---
## General Tips for Designing Prompts

Here are some tips to keep in mind while you are designing your prompts:


### Start Simple
As you get started with designing prompts, you should keep in mind that it is an iterative process that requires a lot of experimentation to get optimal results. Using a simple playground like OpenAI's or Cohere's is a good starting point. 

You can start with simple prompts and keep adding more elements and context as you aim for better results. Versioning your prompt along the way is vital for this reason. As we read the guide you will see many examples where specificity, simplicity, and conciseness will often give you better results.

When you have a big task that involves many different subtasks, you can try to break down the task into simpler subtasks and keep building up as you get better results. This avoids adding too much complexity to the prompt design process at the beginning.

### The Instruction
You can design effective prompts for various simple tasks by using commands to instruct the model what you want to achieve such as "Write", "Classify", "Summarize", "Translate", "Order", etc.

Keep in mind that you also need to experiment a lot to see what works best. Try different instructions with different keywords, contexts, and data and see what works best for your particular use case and task. Usually, the more specific and relevant the context is to the task you are trying to perform, the better. We will touch on the importance of sampling and adding more context in the upcoming guides.

Others recommend that instructions are placed at the beginning of the prompt. It's also recommended that some clear separator like "###" is used to separate the instruction and context. 

For instance:

*Prompt:*
```
### Instruction ###
Translate the text below to Spanish:

Text: "hello!"
```

*Output:*
```
¡Hola!
```

### Specificity
Be very specific about the instruction and task you want the model to perform. The more descriptive and detailed the prompt is, the better the results. This is particularly important when you have a desired outcome or style of generation you are seeking. There aren't specific tokens or keywords that lead to better results. It's more important to have a good format and descriptive prompt. Providing examples in the prompt is very effective to get desired output in specific formats. 

When designing prompts you should also keep in mind the length of the prompt as there are limitations regarding how long this can be. Thinking about how specific and detailed you should be is something to consider. Too many unnecessary details are not necessarily a good approach. The details should be relevant and contribute to the task at hand. This is something you will need to experiment with a lot. We encourage a lot of experimentation and iteration to optimize prompts for your applications.

As an example, let's try a simple prompt to extract specific information from a piece of text.

*Prompt:*
```
Extract the name of places in the following text. 

Desired format:
Place: <comma_separated_list_of_company_names>

Input: "Although these developments are encouraging to researchers, much is still a mystery. “We often have a black box between the brain and the effect we see in the periphery,” says Henrique Veiga-Fernandes, a neuroimmunologist at the Champalimaud Centre for the Unknown in Lisbon. “If we want to use it in the therapeutic context, we need to understand the mechanism."
```

*Output:*
```
Place: Champalimaud Centre for the Unknown, Lisbon
```

The input text is obtained from [this Nature article](https://www.nature.com/articles/d41586-023-00509-z).

### Avoid Impreciseness

Given the tips above about being detailed and improving format, it's easy to fall into the trap of wanting to be too clever about prompts and potentially creating imprecise descriptions. It's often better to be specific and direct. The analogy here is very similar to effective communication -- the more direct, the more effective the message gets across.

For example, you might be interested in learning the concept of prompt engineering. You might try something like:

```
Explain the concept of prompt engineering. Keep the explanation short, only a few sentences, and don't be too descriptive.
```

It's not clear from the prompt above how many sentences to use and what style. You might still somewhat get good responses with the above prompts but the better prompt would be one that is very specific, concise, and to the point. Something like:

```
Use 2-3 sentences to explain the concept of prompt engineering to a high school student.
```

### To do or not to do?
Another common tip when designing prompts is to avoid saying what not to do but say what to do instead. This encourages more specificity and focuses on the details that lead to good responses from the model.

Here is an example of a movie recommendation chatbot failing at exactly what I don't want it to do because of how I wrote the instruction -- focusing on what not to do. 

*Prompt:*
```
The following is an agent that recommends movies to a customer. DO NOT ASK FOR INTERESTS. DO NOT ASK FOR PERSONAL INFORMATION.

Customer: Please recommend a movie based on my interests.
Agent: 
```

*Output:*
```
Sure, I can recommend a movie based on your interests. What kind of movie would you like to watch? Do you prefer action, comedy, romance, or something else?
```

Here is a better prompt:

*Prompt:*
```
The following is an agent that recommends movies to a customer. The agent is responsible to recommend a movie from the top global trending movies. It should refrain from asking users for their preferences and avoid asking for personal information. If the agent doesn't have a movie to recommend, it should respond "Sorry, couldn't find a movie to recommend today.".

Customer: Please recommend a movie based on my interests.
Agent:
```

*Output:*
```
Sorry, I don't have any information about your interests. However, here's a list of the top global trending movies right now: [list of movies]. I hope you find something you like!
```

Some of the examples above were adopted from the ["Best practices for prompt engineering with OpenAI API" article.](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-openai-api)


---
[Next Section (Basic Prompting)](./prompts-basic-usage.md)
