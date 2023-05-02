Here we are going to use as the running example, the task of summarising this product review. Got a panda plush toy from a daughter's birthday 
who loves it and takes it everywhere and so on and so on. If you're building an e-commerce website and there's just a large volume of reviews, having 
a tool to summarise the lengthy reviews could give you a way to very quickly glance over more reviews to get a better sense of what all your 
customers are thinking. Your task is to generate a short summary of a product review from e-commerce websites, summarise  the review below and so on in at 
most 30 words.

# Summarizing
In this lesson, you will summarize text with a focus on specific topics.

## Setup
```python
import openai
import os
openai.api_key  = os.getenv('OPENAI_API_KEY')
```
```python
def get_completion(prompt, model="gpt-3.5-turbo"): # Andrew mentioned that the prompt/ completion paradigm is preferable for this class
    messages = [{"role": "user", "content": prompt}]
    response = openai.ChatCompletion.create(
        model=model,
        messages=messages,
        temperature=0, # this is the degree of randomness of the model's output
    )
    return response.choices[0].message["content"]
 ```
 ## Text to summarize
```python
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site. 

Summarize the review below, delimited by triple 
backticks, in at most 30 words. 

Review: ```{prod_review}```
"""

response = get_completion(prompt)
print(response)
```
Output
```
Soft and cute panda plush toy loved by daughter, but a bit small for the price. Arrived early.
```
## Summarize with a focus on shipping and delivery
```python
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site to give feedback to the \
Shipping deparmtment. 
​
Summarize the review below, delimited by triple 
backticks, in at most 30 words, and focusing on any aspects \
that mention shipping and delivery of the product. 
​
Review: ```{prod_review}```
"""
​
response = get_completion(prompt)
print(response)
```
Output
The output is started focusing on the arrival of the package.
```
The panda plush toy arrived a day earlier than expected, but the customer felt it was a bit small for the price paid.
```

## Summarize with a focus on price and value
```pyhton
prompt = f"""
Your task is to generate a short summary of a product \
review from an ecommerce site to give feedback to the \
pricing deparmtment, responsible for determining the \
price of the product.  
​
Summarize the review below, delimited by triple 
backticks, in at most 30 words, and focusing on any aspects \
that are relevant to the price and perceived value. 
​
Review: ```{prod_review}```
"""
​
response = get_completion(prompt)
print(response)
```
Output
```
The panda plush toy is soft, cute, and loved by the recipient, but the price may be too high for its size.
```
