---
title: AgentQL Page
---

AgentQL `Page` is a wrapper around Playwright's [`Page`](https://playwright.dev/python/docs/api/class-page) that provides access to AgentQL's querying API. 

The following example creates a Playwright's page, navigates it to a URL, and queries for WebElements using AgentQL:

<Tabs labels={["Sync", "Async"]}>
<Tab>

```python filename="agentql_example.py"
import time
import agentql
from playwright.sync_api import sync_playwright

QUERY = """
{
    search_box
    search_btn
}
"""

with sync_playwright() as p, p.chromium.launch(headless=False) as browser:
    page = agentql.wrap(browser.new_page()) # Wrapped to access AgentQL's query API's
    page.goto("https://duckduckgo.com") # [!code highlight]

    aql_response = page.query_elements(QUERY) # [!code highlight]
    aql_response.search_box.type("AgentQL")
    aql_response.search_btn.click()

    # Used only for demo purposes. It allows you to see the effect of the script.
    time.sleep(10)
```

</Tab>
<Tab>

```python filename="agentql_example.py"
import asyncio
import agentql
from playwright.async_api import async_playwright

QUERY = """
{
    search_box
    search_btn
}
"""

async def main():
    async with async_playwright() as p, await p.chromium.launch(headless=False) as browser:
        page = await agentql.wrap_async(browser.new_page())  # Wrapped to access AgentQL's query API's
        await page.goto("https://duckduckgo.com") # [!code highlight]

        aql_response = await page.query_elements(QUERY) # [!code highlight]
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

### get_by_prompt

Returns a single web element located by a natural language prompt (as opposed to a AgentQL query).

#### Usage

<Tabs labels={["Sync", "Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    search_box = page.get_by_prompt(prompt="Search input field")
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    search_box = await page.get_by_prompt(prompt="Search input field")
    ```
  </Tab>
</Tabs>

#### Arguments

- `prompt` [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)

  The natural language description of the element to locate.

- `timeout` [int](https://docs.python.org/3/library/stdtypes.html#index-13) (optional)

  Timeout value in seconds for the connection with backend API service.

- `wait_for_network_idle` [bool](https://docs.python.org/3/library/stdtypes.html) (optional)

  Whether to wait for network reaching full idle state before querying the page. If set to `False`, this method will only check for whether page has emitted [`load` event](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event). Default is `True`.

- `include_aria_hidden` [bool](https://docs.python.org/3/library/stdtypes.html) (optional)

  Whether to include elements with `aria-hidden` attribute in the accessibility tree. Default is `True`.

#### Returns

- [Locator](https://playwright.dev/python/docs/api/class-locator) | [`None`](https://docs.python.org/3/library/constants.html#None)

  Playwright Locator for the found element or `None` if no matching elements were found.

---

### query_elements

Queries the web page for multiple web elements that match the AgentQL query.

#### Usage

<Tabs labels={["Sync", "Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    agentql_response = page.query_elements(
        query="""
            {
                search_box
                search_btn
            }
            """
    )
    print(agentql_response.to_data())
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    agentql_response = await page.query_elements(
        query="""
            {
                search_box
                search_btn
            }
            """
    )
    print(await agentql_response.to_data())
    ```
  </Tab>
</Tabs>

#### Arguments

- `query` [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)

  An [AgentQL query](/agentql-query/query-intro) in String format.

- `timeout` [int](https://docs.python.org/3/library/stdtypes.html#index-13) (optional)

  Timeout value in seconds for the connection with the backend API service.

- `wait_for_network_idle` [bool](https://docs.python.org/3/library/stdtypes.html) (optional)

  Whether to wait for network reaching full idle state before querying the page. If set to `False`, this method will only check for whether page has emitted [`load` event](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event). Default is `True`.

- `include_aria_hidden` [bool](https://docs.python.org/3/library/stdtypes.html) (optional)

  Whether to include elements with `aria-hidden` attribute in the accessibility tree. Default is `True`.

#### Returns

- [AQLResponseProxy](aqlresponse_reference)

  The AgentQL response object with elements that match the query. Response provides access to requested elements via its fields.

---

### query_data

Queries the web page for data that matches the AgentQL query, such as blocks of text or numbers.

#### Usage

<Tabs labels={["Sync", "Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    retrieved_data = page.query_data(
        query="""
            {
                products[] {
                  name
                  price
                }
            }
            """
    )
    print(retrieved_data)
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    retrieved_data = await page.query_data(
        query="""
            {
                products[] {
                  name
                  price
                }
            }
            """
    )
    print(retrieved_data)
    ```
  </Tab>
</Tabs>

#### Arguments

- `query` [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)

  An [AgentQL query](/agentql-query/query-intro) in String format.

- `timeout` [int](https://docs.python.org/3/library/stdtypes.html#index-13) (optional)

  Timeout value in seconds for the connection with backend API service.

- `wait_for_network_idle` [bool](https://docs.python.org/3/library/stdtypes.html) (optional)

  Whether to wait for network reaching full idle state before querying the page. If set to `False`, this method will only check for whether page has emitted [`load` event](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event). Default is `True`.

- `include_aria_hidden` [bool](https://docs.python.org/3/library/stdtypes.html) (optional)

  Whether to include elements with `aria-hidden` attribute in the accessibility tree. Default is `True`.

#### Returns

- [dict](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)

  Data that matches the query.

---

### wait_for_page_ready_state

Waits for the page to reach the "Page Ready" state, i.e. page has entered a relatively stable state and most main content is loaded. Might be useful before triggering an AgentQL query or any other interaction for slowly rendering pages.

#### Usage

<Tabs labels={["Sync", "Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    page.wait_for_page_ready_state()
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    await page.wait_for_page_ready_state()
    ```
  </Tab>
</Tabs>

#### Arguments

- `wait_for_network_idle` [bool](https://docs.python.org/3/library/stdtypes.html) (optional)

  Whether to wait for network reaching full idle state. If set to `False`, this method will only check for whether page has emitted [`load` event](https://developer.mozilla.org/en-US/docs/Web/API/Window/load_event). Default is `True`.

#### Returns

- [NoneType](https://docs.python.org/3/library/constants.html#None)

---

### enable_stealth_mode

Enables "stealth mode" with given configuration. To avoid being marked as a bot, parameters' values should match the real values used by your device.

<Admonition type="note">
Use browser fingerprinting websites such as [bot.sannysoft.com](https://bot.sannysoft.com/) and [pixelscan.net](https://pixelscan.net/) for realistic examples.
</Admonition>

#### Usage

<Tabs labels={["Sync", "Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    page.enable_stealth_mode(
        webgl_vendor=your_browser_vendor,
        webgl_renderer=your_browser_renderer,
        nav_user_agent=navigator_user_agent,
    )
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    await page.enable_stealth_mode(
        webgl_vendor=your_browser_vendor,
        webgl_renderer=your_browser_renderer,
        nav_user_agent=navigator_user_agent,
    )
    ```
  </Tab>
</Tabs>

#### Arguments

- `webgl_vendor` [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str) (optional)

  The vendor of the GPU used by WebGL to render graphics, such as `Apple Inc.`. After setting this parameter, your browser will emit this vendor information.

- `webgl_renderer` [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str) (optional)

  Identifies the specific GPU model or graphics rendering engine used by WebGL, such as `Apple M3`. After setting this parameter, your browser will emit this renderer information.

- `nav_user_agent` [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str) (optional)

  Identifies the browser, its version, and the operating system, such as `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.0.0 Safari/537.36`. After setting this parameter, your browser will send this user agent information to the website.

#### Returns

- [NoneType](https://docs.python.org/3/library/constants.html#None)
