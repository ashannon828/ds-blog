+++
title = 'Creating Synthetic Data with Python Using Faker'
date = 2024-04-04T09:36:01+02:00
draft = false
+++

Let's start from the very beginning. What is synthetic data, and why do we even need it? In simple words, synthetic data is just that—synthetic or fake data created to make some task easier. Common use cases include masking sensitive data so that it can be shared, simulating events like fraudulent transactions, using dummy data to help with quality assurance, and many more.

In my case, I needed dummy data to test a database I was developing and [Faker](https://faker.readthedocs.io/en/master/) is my go to python library for that. I figure, why not write a quick blog post to share how I use it.

### Getting started with Faker

First install the library with `pip install Faker`. Then import it to create and initialize a faker generator. Once we can do that, we can call some of the built-in methods, like `fake.name()` and `fake.address()` to see how it works.

```python
from faker import Faker

fake = Faker()

fake.name() # 'Theresa Brown'
fake.address() # '449 Catherine Prairie\nSouth Danielle, AS 95267'
```

In most cases, we'll want more than one data point. To do that, it's as simple as dropping the faker instance into a loop.

```python
for _ in range(3): # Good practice to use _ if you don't need the variable.
  print(fake.name())

# 'Katrina Watson'
# 'Kathryn Santana'
# 'Helen Frederick'
```

You can probably see how the built-in methods are helpful, but the power of faker comes when you start creating custom providers.

### Creating a custom provider

Creating a custom provider is pretty simple. You import the base provider and pass it to your class. In my case, I needed to generate employee data to test a database.

```python
from faker.providers import BaseProvider

class EmployeeProvider(BaseProvider):
    def role(self, r):
        roles = r # an array of roles, i.e., manager, employee, etc
        return random.choices(roles, weights=[.1, .5, .3, .1])
```

The first method I added was to generate an employee's role and then use Python's built-in random library to choose them based on a specific probability I assigned. Anyway, once we create a provider, we need to pass it back to Faker so that we can use it.

```python
fake.add_provider(EmployeeProvider)
```

### That's all there is to it

Once you create your provider, it's a matter of deciding how you want to use it. In my case, I just connected to the database and inserted my 10k rows into it. In most cases, you'll probably want to build a data frame and save it as a CSV.
