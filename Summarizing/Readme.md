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
#### Comment
But in these summaries, even though it  generated the information relevant to shipping, 
it had some other information too, which you could decide may or may not be hopeful. So depending on how you want to summarize it, 
you can also ask it to extract information rather than summarize it.

## Try "extract" instead of "summarize"
```python
prompt = f"""
Your task is to extract relevant information from \ 
a product review from an ecommerce site to give \
feedback to the Shipping department. 
​
From the review below, delimited by triple quotes \
extract the information relevant to shipping and \ 
delivery. Limit to 30 words. 
​
Review: ```{prod_review}```
"""
​
response = get_completion(prompt)
print(response)
```
Output
```
The product arrived a day earlier than expected.
```
Lastly, a concrete example for how to use this in a workflow to help summarize multiple reviews to make them easier to read.
## Summarize multiple product reviews
```python
review_1 = prod_review 
​
# review for a standing lamp
review_2 = """
Needed a nice lamp for my bedroom, and this one \
had additional storage and not too high of a price \
point. Got it fast - arrived in 2 days. The string \
to the lamp broke during the transit and the company \
happily sent over a new one. Came within a few days \
as well. It was easy to put together. Then I had a \
missing part, so I contacted their support and they \
very quickly got me the missing piece! Seems to me \
to be a great company that cares about their customers \
and products. 
"""
​
# review for an electric toothbrush
review_3 = """
My dental hygienist recommended an electric toothbrush, \
which is why I got this. The battery life seems to be \
pretty impressive so far. After initial charging and \
leaving the charger plugged in for the first week to \
condition the battery, I've unplugged the charger and \
been using it for twice daily brushing for the last \
3 weeks all on the same charge. But the toothbrush head \
is too small. I’ve seen baby toothbrushes bigger than \
this one. I wish the head was bigger with different \
length bristles to get between teeth better because \
this one doesn’t.  Overall if you can get this one \
around the $50 mark, it's a good deal. The manufactuer's \
replacements heads are pretty expensive, but you can \
get generic ones that're more reasonably priced. This \
toothbrush makes me feel like I've been to the dentist \
every day. My teeth feel sparkly clean! 
"""
​
# review for a blender
review_4 = """
So, they still had the 17 piece system on seasonal \
sale for around $49 in the month of November, about \
half off, but for some reason (call it price gouging) \
around the second week of December the prices all went \
up to about anywhere from between $70-$89 for the same \
system. And the 11 piece system went up around $10 or \
so in price also from the earlier sale price of $29. \
So it looks okay, but if you look at the base, the part \
where the blade locks into place doesn’t look as good \
as in previous editions from a few years ago, but I \
plan to be very gentle with it (example, I crush \
very hard items like beans, ice, rice, etc. in the \ 
blender first then pulverize them in the serving size \
I want in the blender then switch to the whipping \
blade for a finer flour, and use the cross cutting blade \
first when making smoothies, then use the flat blade \
if I need them finer/less pulpy). Special tip when making \
smoothies, finely cut and freeze the fruits and \
vegetables (if using spinach-lightly stew soften the \ 
spinach then freeze until ready for use-and if making \
sorbet, use a small to medium sized food processor) \ 
that you plan to use that way you can avoid adding so \
much ice if at all-when making your smoothie. \
After about a year, the motor was making a funny noise. \
I called customer service but the warranty expired \
already, so I had to buy another one. FYI: The overall \
quality has gone done in these types of products, so \
they are kind of counting on brand recognition and \
consumer loyalty to maintain sales. Got it in about \
two days.
"""
​
reviews = [review_1, review_2, review_3, review_4]
```
```python
for i in range(len(reviews)):
    prompt = f"""
    Your task is to generate a short summary of a product \ 
    review from an ecommerce site. 
​
    Summarize the review below, delimited by triple \
    backticks in at most 20 words. 
​
    Review: ```{reviews[i]}```
    """
​
    response = get_completion(prompt)
    print(i, response, "\n")
```
Output
```
0 Soft and cute panda plush toy loved by daughter, but a bit small for the price. Arrived early. 

1 Affordable lamp with storage, fast shipping, and excellent customer service. Easy to assemble and missing parts were quickly replaced. 

2 Good battery life, small toothbrush head, but effective cleaning. Good deal if bought around $50. 

3 Mixed review of a blender system with price gouging and decreased quality, but helpful tips for use. 
```
