---
title: First steps
---

AgentQL is a robust query language that identifies elements on a webpage using natural language with the help of AI. It can be used to automate tasks on the web, extract data, and interact with websites in real-time. AgentQL uses AI to infer which element or data you mean based on _term names and query structure_, so it will find what you're looking for even if the page's markup and layout change drastically.

AgentQL's Python SDK allows you to write Python scripts that identify elements and extract data from the web using the AgentQL query language. In this guide, you will learn how to use AgentQL queries and the SDK to automate page interactions and data extraction from the page.

## Prerequisites

- [The AgentQL SDK](/installation/sdk-installation)

## Instructions

The script below will open a browser and do the following:

1. Navigate to **[scrapeme.live/shop](https://scrapeme.live/shop)**.
2. Input "fish" into the search field in header section.
3. Press "Enter" key to perform the search.
4. Close the the browser after 10 seconds.

Save the following script in a file named **example_script.py** then open a terminal in your project's folder and run the script with `python3 example_script.py`.

```python filename="example_script.py"
import time
import agentql
from playwright.sync_api import sync_playwright


with sync_playwright() as playwright, playwright.chromium.launch(headless=False) as browser:
    page = agentql.wrap(browser.new_page())
    page.goto("https://scrapeme.live/shop/")

    QUERY = """
    {
        search_box
    }
    """

    response = page.query_elements(QUERY)

    response.search_box.type("fish")
    page.keyboard.press("Enter")

    # Used only for demo purposes. It allows you to see the effect of script.
    time.sleep(10)
```

Here's how you can create it step by step:

### Step 0: Create a New Python Script

In your project folder, create a new Python script and name it `example_script.py`.

### Step 1: Import Required Libraries

Import needed functions and classes from `playwright` library and import the `time` and `agentql` libraries.

```python filename="example_script.py"
import time
import agentql
from playwright.sync_api import sync_playwright
```

[Playwright](https://playwright.dev/) is an end-to-end automation and testing tool that can be used for automation. In this example, it manages open the browser and interacting with the elements AgentQL returns.

### Step 2: Launch the Browser and Open the Website

The last preparation step is launching the browser and navigating to the target website. This is done using usual Playwright's API. The only difference is the type of the page — instead of [Playwright's `Page`](https://playwright.dev/python/docs/api/class-page) class, it will be wrapped with `agentql.wrap()`, and you will get [AgentQL's `Page`](/api-references/agentql-page) class that will be the main interface not only for interacting with the web page but also for executing AgentQL queries.

```python filename="example_script.py"
with sync_playwright() as playwright, playwright.chromium.launch(headless=False) as browser:
    page = agentql.wrap(browser.new_page())
    page.goto("https://scrapeme.live/shop")
```

<Admonition type="tip">
- Default AgentQL SDK implementation is built on top of Playwright and uses all of its functionality for interacting with browser, page and elements on the page.
- By default Playwright launches the browser in headless mode. Here it is explicitly set to `False` for the sake of example.
</Admonition>

### Step 3: Define AgentQL Query

AgentQL queries are how you query elements from a web page. A query describes the elements you want to interact with or consume content from and defines your desired output structure.

```python filename="example_script.py"
QUERY = """
{
    search_box
}
"""
```

In this query, we specify the element we want to interact with on `"https://scrapeme.live/shop/"`:

- `search_box` - search input field

<Admonition type="info">
- To learn more about AgentQL query syntax, capabilities and best practices, check out our [AgentQL Query documentation](/agentql-query/query-intro).
- If you want to query just one element on the page, you can use simpler [`get_by_prompt`](/api-references/agentql-page#getbyprompt) API that can identify an element by natural language description.
</Admonition>

### Step 4: Execute AgentQL Query

AgentQL's `Page` extends Playwright's `Page` class with querying capabilities:

```python filename="example_script.py"
response = page.query_elements(QUERY)
```

`response` variable will have the same structure as defined by the given AgentQL query, i.e. it will have 1 field: `search_box`. This field will either be `None` if described element was not found on the page, or an instance of [`Locator`](https://playwright.dev/python/docs/api/class-locator) class that allows you to interact with the found element.

### Step 5: Interact with Web Page

```python filename="example_script.py"
response.search_box.type("fish")
```

This line uses the `type` method on the `search_box` element found in the previous step. It mimics typing "fish" into the search box.

```python filename="example_script.py"
page.keyboard.press("Enter")
```

Here, the `Enter` method is called on the `keyboard` attribute of the page, simulating a press on the `Enter` key.

<Admonition type="info">
The `type` method is coming from the Playwright's `Locator` class. To get a full list of methods available for interacting with web elements, please refer to [this class's documentation](https://playwright.dev/docs/api/class-locator).
</Admonition>

### Step 6: Pause the Script Execution

```python filename="example_script.py"
time.sleep(10)
```

Here, standard `time` library is used to pause the execution for 10 seconds to see the effect of this script before closing the browser.

<Admonition type="warning">
`time.sleep()` is used only for demo purposes and will impact the performance. Don't use it in production!
</Admonition>

### Step 7: Stop the Browser

```python filename="example_script.py"
browser.close()
```

Finally, the `close` method is called on the `browser` object, ending the web browsing session. This is important for properly releasing resources.

### Step 8: Run the Script

Open a terminal in your project's folder and run the script:

```bash
python3 example_script.py
```
