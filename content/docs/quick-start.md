---
title: AgentQL Quick Start
---

This guide will get you started with the basics of AgentQL queries, how to use the Chrome extension for debugging, and how to implement queries using AgentQL's Python SDK.

## What is an AgentQL query?

AgentQL is a query language and a set of supporting developer tools designed to identify web elements and their data using natural language and return them in the shape you define.

Here is an AgentQL query you can use right now to locate the search button on this page:

```AgentQL
{
    search_button
}
```

This query can return each heading on this page:

```AgentQL
{
    headings[]
}
```

The following will show you how to execute queries to retrieve data and elements from web pages starting right here, with the page you are on now.

## Get your API key

You need an API key to make AgentQL queries. [Get one in our DevPortal](https://dev.agentql.com), and you'll be ready to go!

## Query this page with the AgentQL Debugger Chrome extension

The AgentQL Debugger lets you write and test queries in real-time on web pages, without needing to spin up the Python SDK. It's perfect for debugging queries before putting them into production! Here's how to get started:

1. Install the AgentQL Debugger from the [Chrome Web Store](https://chromewebstore.google.com/detail/agentql-ide/idnejmodeepdobpinkkgpkeabkabhhej?hl=en&authuser=0).

2. Come back to this page and open Chrome DevTools (**Ctrl+Shift+I** on Windows/Linux or **Cmd+Opt+I** on Mac).

3. In the top bar of the devtools panel, select "AgentQL".

<Admonition type="note">
If you don't see the option, click on the overflow menu button (») and select "AgentQL" from the list.
</Admonition>

4. Enter your API key when prompted.

5. In the query panel, try this query:

```AgentQL
{
    search_button
}
```

7. Click "Fetch Web Elements" to run the query to return the search button element.

In the **AgentQL tab**, you will find an entry for the search button. If you hover over it you will see it highlighted on the page.

### Try it out!

The best way to learn how AgentQL works is to play around with it in the extension. Here are some things you can try:

* Click the **eye icon**, and you will navigate to the element on the page.
* Click **`</>`**, and you will navigate to the element in the devtools.
* Click "Fetch Data" to return the contents of the queried elements instead of the elements themselves (great for scraping).
* Use `[]` to return a list of items.
* Use `()` to add additional context to find the exact element:
```AgentQL
{
    headings(all the headings inside the article)[]
}
```

Experiment with different queries to get a feel for how AgentQL works. For example, try this one out with "Fetch Data" to get a list of all the headings on this page:

```AgentQL
{
    headings[]
}
```

You can even nest items:

```AgentQL
{
    breadcrumbs {
        first_item
        last_item
    }
}
```

## Perform the query with the AgentQL Python SDK

Now that you're familiar with writing queries, let's use the Python SDK to run the same query programmatically.

<Admonition type="note">
If you are set up to use virtual environments, we recommend using one for the following steps.
</Admonition>

1. In you project folder, install the AgentQL SDK and initialize AgentQL:

  ```bash
  pip3 install agentql
  agentql init
  ```
2. Provide your API key.
3. Create a new Python file, `example_script.py`.
4. Add the following code:

```python filename="example_script.py"
import time
import agentql
from playwright.sync_api import sync_playwright

# Initialise the browser
with sync_playwright() as playwright, playwright.chromium.launch(headless=False) as browser:
    page = agentql.wrap(browser.new_page())
    page.goto("https://docs.agentql.com/quick-start")

    # Find "Search" button using Smart Locator
    search_button = page.get_by_prompt("search button")
    # Interact with the button
    search_button.click()

    # Define a query for modal dialog's search input
    SEARCH_BOX_QUERY = """
    {
        modal {
            search_box
        }
    }
    """

    # Get the modal's search input and fill it with "Quick Start"
    response = page.query_elements(SEARCH_BOX_QUERY)
    response.modal.search_box.type("Quick Start")

    # Define a query for the search results
    SEARCH_RESULTS_QUERY = """
    {
        modal {
            search_box
            search_results {
                items[]
            }
        }
    }
    """

    # Execute the query after the results have returned then click on the first one
    response = page.query_elements(SEARCH_RESULTS_QUERY)
    response.modal.search_results.items[0].click()

    # Used only for demo purposes. It allows you to see the effect of the script.
    time.sleep(10)
```

5. Run the script:

```bash
python3 example_script.py
```

This script will open this site, **docs.agentql.com**, click the search button, fill the search modal's input with "Quick Start," and click on the first result—bringing you back to this page.

## Next steps

Congratulations! You've now used AgentQL queries both in the Chrome extension and the Python SDK. This is usually how you will work with AgentQL: optimizing and debugging queries with the extension before running them in Python scripts with the SDK.

Here are some next steps to explore:

- Learn more about [AgentQL query syntax](/agentql-query/query-intro)
- Explore [best practices for writing queries](/agentql-query/best-practices)
- Check out an [example script for collecting YouTube comment data](/getting-started/example-script)

Happy querying with AgentQL!
