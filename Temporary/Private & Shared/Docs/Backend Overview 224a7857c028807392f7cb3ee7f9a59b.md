# Backend Overview

# Framework

| Web | Quart |
| --- | --- |
| Validation | QuartSchema |
| Database | asqlite |

# Quick Async Primer

In normal Python, things run one after another. You can’t do multiple things at once. This makes web servers slow and unresponsive because if an operation takes a long time, the entire application freezes. This is a bad look when you want to make a nice, responsive user interface.

One of the more modern and widely-accepted solutions to this is called async i/o. It’s a way of writing code that runs asynchronously, so you can run many things at once, which is great for web servers.

Around the codebase you’ll see a lot of `async` and `await` scattered around. These are the syntactical building blocks for writing async code. In async python, you define and call async functions, which you do with the `async` and `await` keywords

Here’s what the syntax means:

`async def`  is for defining an async function. async functions are just like normal functions, but they allow you to use `async/await` keywords inside. You can only call async functions within async functions.

`await something()` is for calling an async function. You must call async functions using `await` otherwise they won’t run. If you don’t `await` an async function, you’ll probably get a warning when running the code like “RuntimeWarning: coroutine 'function_name' was never awaited.”

**Important detail**: When writing async code, you have to be careful when mixing it with normal code. **If normal code takes too long to run**, even if it’s in an async function, **it will freeze the entire app!!!** This is because async code isn’t fully running in parallel, it’s actually just a clever way of running sync code to make it appear like multiple pieces of code are running at once. This only works if you use `async` and `await` and make sure all normal Python code doesn’t take too long to run. If you need to run normal code that takes awhile, there’s more on that below.

An example:

```python
import time
import asyncio

# This is wrong
async def wrong(seconds: int):
    time.sleep(5)  # !!!! WILL FREEZE APP FOR 5 SECONDS

# This is correct
async def pause(seconds: int):
	print("Before sleep")
	await asyncio.sleep(seconds)  # doesn't freeze app
	print("After sleep")
	
# Calling this elsewhere...
await pause(5)
```

There are a couple of other keywords you might see around:

`async with` is the same as Python’s `with` keyword, but for async

`async for` is the same as `for` except for async

An example:

```python
# This is taken from app.py

# db.acquire gets a connection to the db in parallel
# this is so we can have multiple connections to the db at once
async with app.db.acquire() as conn:
		# now we have our connection, and can use it with await to execute SQL
    files = await conn.fetchall("SELECT * FROM files;")

# after we have left the `async with`, the connection is closed
# and cleaned up automatically. No need for conn.close() or anything
```

## Running normal, long-running code with async code

Sometimes you’ll need to run code that takes a long time with async code. Here’s how to do that in our codebase:

```python
# Say you have a function that runs for awhile
def takes_time(echo_back: str, times: int):
    time.sleep(10)
    return echo_back * times
    
# Here's how to run it in an async-friendly way:
# You can pass the arguments in after the function
result = await asyncio.to_thread(takes_time, "Hello ", times=2)
# result contains "Hello Hello "
```

## More

This is just scratching the surface! There’s a lot of very interesting details about how async code actually works that I haven’t covered. If you want to read more, I recommend [reading this article](https://realpython.com/async-io-python/).