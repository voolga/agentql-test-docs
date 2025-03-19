---
title: AgentQL
---

The `agentql` module provides utility methods to convert Playwright's [`Page`](https://playwright.dev/python/docs/api/class-page) to [AgentQL's `Page`](/api-references/agentql-page), which gives access to AgentQL's querying API.

The following example creates a page, navigates it to a URL, and queries for web elements:

<Tabs labels={["Sync", "Async"]}>
<Tab>

```python filename="agentql_example.py"
import time
import agentql # [!code highlight]
from playwright.sync_api import sync_playwright

QUERY = """
{
    search_box
    search_btn
}
"""

with sync_playwright() as p, p.chromium.launch(headless=False) as browser:
    page = agentql.wrap(browser.new_page()) # Wraps the Playwright Page to access AgentQL's features. # [!code highlight]
    page.goto("https://duckduckgo.com")

    aql_response = page.query_elements(QUERY)
    aql_response.search_box.type("AgentQL")
    aql_response.search_btn.click()

    # Used only for demo purposes. It allows you to see the effect of the script.
    time.sleep(10)
```

</Tab>
<Tab>

```python filename="agentql_example.py"
import asyncio
import agentql # [!code highlight]
from playwright.async_api import async_playwright

QUERY = """
{
    search_box
    search_btn
}
"""

async def main():
    async with async_playwright() as p, p.chromium.launch(headless=False) as browser:
        page = await agentql.wrap_async(browser.new_page())  # Wraps the Playwright Page to access AgentQL's features. # [!code highlight]
        await page.goto("https://duckduckgo.com")

        aql_response = await page.query_elements(QUERY)
        await aql_response.search_box.type("AgentQL")
        await aql_response.search_btn.click()

        # Used only for demo purposes. It allows you to see the effect of the script.
        await asyncio.sleep(10)

asyncio.run(main())
```

</Tab>
</Tabs>

---

## Methods

### wrap

Casts a Playwright Sync `Page` object to an AgentQL `Page` type to get access to AgentQL's querying API. See [AgentQL `Page`](agentql-page) reference for API details.

#### Usage

```python filename="agentql_example.py"
page = agentql.wrap(browser.new_page())
```

#### Arguments

- `page` [Playwright's Page](https://playwright.dev/python/docs/api/class-page)

#### Returns

- [AgentQL Page](agentql-page)

---

### wrap_async

Casts a Playwright Async `Page` object to an AgentQL `Page` type to get access to the AgentQL's querying API. See [AgentQL `Page`](agentql-page) reference for API details.

#### Usage

```python filename="agentql_example.py"
page = agentql.wrap_async(browser.new_page())
```

#### Arguments

- `page` [Playwright's Page](https://playwright.dev/python/docs/api/class-page)

#### Returns

- [AgentQL Page](agentql-page)
