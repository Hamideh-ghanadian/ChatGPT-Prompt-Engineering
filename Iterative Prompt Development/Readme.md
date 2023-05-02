# Iterative Prompt Development

Iterative Prompt Development is a technique used in Natural Language Processing (NLP) and AI development, particularly for language models like GPT-3, to create and refine prompts or prompts templates through an iterative process.

The process involves creating a set of prompts or prompt templates, then testing them with the language model, analyzing the results, and then refining or iterating on the prompts based on the insights gained from the analysis. This process is repeated until the desired performance and quality of the model are achieved.

The iterative prompt development process is essential for improving the performance and accuracy of language models, particularly in generating high-quality outputs for specific tasks, such as translation, summarization, and text generation. By refining and optimizing prompts through this iterative process, developers can train language models to better understand the nuances of language and produce more accurate and relevant results.
Iterative Prompt Develelopment
In this lesson, you'll iteratively analyze and refine your prompts to generate marketing copy from a product fact sheet.

## Setup
```python
import openai
import os
```
```python
openai.api_key  = os.getenv('OPENAI_API_KEY')
def get_completion(prompt, model="gpt-3.5-turbo"):
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
```
Generate a marketing product description from a product fact sheet
```python
fact_sheet_chair = """
OVERVIEW
- Part of a beautiful family of mid-century inspired office furniture, 
including filing cabinets, desks, bookcases, meeting tables, and more.
- Several options of shell color and base finishes.
- Available with plastic back and front upholstery (SWC-100) 
or full upholstery (SWC-110) in 10 fabric and 6 leather options.
- Base finish options are: stainless steel, matte black, 
gloss white, or chrome.
- Chair is available with or without armrests.
- Suitable for home or business settings.
- Qualified for contract use.
​
CONSTRUCTION
- 5-wheel plastic coated aluminum base.
- Pneumatic chair adjust for easy raise/lower action.
​
DIMENSIONS
- WIDTH 53 CM | 20.87”
- DEPTH 51 CM | 20.08”
- HEIGHT 80 CM | 31.50”
- SEAT HEIGHT 44 CM | 17.32”
- SEAT DEPTH 41 CM | 16.14”
​
OPTIONS
- Soft or hard-floor caster options.
- Two choices of seat foam densities: 
 medium (1.8 lb/ft3) or high (2.8 lb/ft3)
- Armless or 8 position PU armrests 
​
MATERIALS
SHELL BASE GLIDER
- Cast Aluminum with modified nylon PA6/PA66 coating.
- Shell thickness: 10 mm.
SEAT
- HD36 foam
​
COUNTRY OF ORIGIN
- Italy
"""
```
```python
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.
​
Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.
​
Technical specifications: ```{fact_sheet_chair}```
"""
response = get_completion(prompt)
print(response)
```
Output
```
Introducing our stunning mid-century inspired office chair, the perfect addition to any home or business setting.\
Part of a beautiful family of office furniture, including filing cabinets, desks, bookcases, meeting tables, and\
more, this chair is available in several options of shell color and base finishes to suit your style. Choose from\
plastic back and front upholstery (SWC-100) or full upholstery (SWC-110) in 10 fabric and 6 leather options.

The chair is constructed with a 5-wheel plastic coated aluminum base and features a pneumatic chair adjust for easy raise/
lower action. It is available with or without armrests and is qualified for contract use. The base finish options are \
stainless steel, matte black, gloss white, or chrome.

Measuring at a width of 53 cm, depth of 51 cm, and height of 80 cm, with a seat height of 44 cm and seat depth of\
41 cm, this chair is designed for ultimate comfort. You can also choose between soft or hard-floor caster options\
and two choices of seat foam densities: medium (1.8 lb/ft3) or high (2.8 lb/ft3). The armrests are available in \
either an armless or 8 position PU option.

The materials used in the construction of this chair are of the highest quality. The shell base glider is made of \
cast aluminum with modified nylon PA6/PA66 coating and has a shell thickness of 10 mm. The seat is made of HD36 foam,\
ensuring maximum comfort and durability.

This chair is made in Italy and is the perfect combination of style and functionality. Upgrade your workspace with \
our mid-century inspired office chair today!
```
#### Issue 1: The text is too long
Limit the number of words/sentences/characters.
```python
prompt = f"""
Your task is to help a marketing team create a 
description for a retail website of a product based 
on a technical fact sheet.
​
Write a product description based on the information 
provided in the technical specifications delimited by 
triple backticks.
​
Use at most 50 words.
​
Technical specifications: ```{fact_sheet_chair}```
"""
response = get_completion(prompt)
print(response)
```
Output
```
Introducing our mid-century inspired office chair, perfect for home or business settings. Available in a range of shell colors and base finishes, with or without armrests. Choose from 10 fabric and 6 leather options for full or plastic upholstery. With a 5-wheel base and pneumatic chair adjust, it's both stylish and functional. Made in Italy.
```
```python
len(response)
```
Output
```
344
```
#### Issue 2. Text focuses on the wrong details
