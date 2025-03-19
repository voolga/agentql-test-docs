---
title: AQLResponseProxy
---

The [AQLResponseProxy](aqlresponse_reference) class is returned by AgentQL Page's [query_elements()](agentql-page#query_elements) method. **Not the actual data**, AQLResponseProxy is a metadata that allows for intuitive access to web elements using dot notation. But this class can be converted into raw data as structured dictionary through its `to_data()` method.

To access desired web elements, users can directly use the names defined in queries as attributes of the response object. It will return desired elements as [Playwright Locator](https://playwright.dev/python/docs/api/class-locator) objects, and users can interact with these elements, such as click or type, through **Playwright Locator API**.

The following example queries for web elements through `query_elements()` method, interacts with these elements through `AQLResponseProxy` objects, and converts `AQLResponseProxy` objects into raw data.

<Tabs labels={["Sync","Async"]}>
  <Tab>

    ```python filename="agentql_example.py"
    import time
    import agentql
    from playwright.sync_api import sync_playwright

    QUERY = """
    {
        search_box
        header {
            search_btn
        }
    }
    """

    with sync_playwright() as playwright, playwright.chromium.launch(headless=False) as browser:
        page = agentql.wrap(browser.new_page())
        page.goto("https://duckduckgo.com")

        # Get AQLResponseProxy, which contains desired web elements
        aql_response = page.query_elements(QUERY) # [!code highlight]

        # Access the elements with dot notation and interact with them as Playwright Locator objects
        aql_response.search_box.type("AgentQL") # [!code highlight]

        # To access a nested elements in query, simply chain attributes together with dot notation
        aql_response.header.search_btn.click() # [!code highlight]

        # Convert response into raw data as structured dictionary with to_data() method
        raw_data_in_dict = aql_response.to_data() # [!code highlight]
        print(raw_data_in_dict)

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
        header {
            search_btn
        }
    }
    """

    async def main():
        async with async_playwright() as playwright, await playwright.chromium.launch(headless=False) as browser:
            page = await agentql.wrap_async(browser.new_page())
            await page.goto("https://duckduckgo.com")

            # Get AQLResponseProxy, which contains desired web elements
            aql_response = await page.query_elements(QUERY) # [!code highlight]

            # Access the elements with dot notation and interact with them as Playwright Locator objects
            await aql_response.search_box.type("AgentQL") # [!code highlight]

            # To access a nested element in query, simply chain attributes together with dot notation
            await aql_response.header.search_btn.click() # [!code highlight]

            # Convert response into raw data as structured dictionary with to_data() method
            raw_data_in_dict = await aql_response.to_data() # [!code highlight]
            print(raw_data_in_dict)

            # Used only for demo purposes. It allows you to see the effect of the script.
            await asyncio.sleep(10)

    asyncio.run(main())
    ```

  </Tab>
</Tabs>

---

## Methods

### to_data

Converts the response data into a structured dictionary based on the query tree.

#### Usage

<Tabs labels={["Sync","Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    aql_response = page.query_elements(QUERY)
    aql_response.to_data()
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    aql_response = await page.query_elements(QUERY)
    await aql_response.to_data()
    ```
  </Tab>
</Tabs>

#### Returns

- [dict](https://docs.python.org/3/library/stdtypes.html#dict)

A structured Python dictionary in the following format.

```python filename="agentql_example.py"
{
  "query_field": "text content of the corresponding web element"
}
```

---

### \_\_getattr\_\_

If this method is called on an innermost node of the query, it will return the desired web element as a [Playwright Locator](https://playwright.dev/python/docs/api/class-locator) object. Please check the [Playwright API reference](https://playwright.dev/python/docs/api/class-locator) to see available methods in `Locator` class.

If it is called on a container node of the query, it will return another `AQLResponseProxy` object, which can be further interacted to get `Playwright Locator` object.

#### Usage

<Tabs labels={["Sync","Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    QUERY = """
    {
        search_btn
        search_results[]
    }
    """

    aql_response = page.query_elements(QUERY)

    # This invokes Playwright Locator object's click() method
    aql_response.search_btn.click()

    # This iterate through search result with AQLResponseProxy
    for search_result in aql_response.search_results:
        print(search_result)
    ```

  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    QUERY = """
    {
        search_btn
        search_results[]
    }
    """

    aql_response = await page.query_elements(QUERY)

    # This invokes Playwright Locator object's click() method
    await aql_response.search_btn.click()

    # This iterate through search result with AQLResponseProxy
    for search_result in aql_response.search_results:
        print(search_result)
    ```

  </Tab>
</Tabs>

#### Arguments

- `name` [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)

The name of the attribute to retrieve.

#### Returns

- [AQLResponseProxy](aqlresponse_reference) | [Playwright Locator](https://playwright.dev/python/docs/api/class-locator)

---

### \_\_getitem\_\_

Allows indexing into the response data if it's a list.

#### Usage

<Tabs labels={["Sync", "Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    QUERY = """
    {
        search_results[]
    }
    """

    aql_response = page.query_elements(QUERY)

    # Get the second result in the list
    second_result = aql_response.search_results[1]
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    QUERY = """
    {
        search_results[]
    }
    """

    aql_response = await page.query_elements(QUERY)

    # Get the second result in the list
    second_result = aql_response.search_results[1]
    ```
  </Tab>
</Tabs>

#### Arguments

- `index` [int](https://docs.python.org/3/library/stdtypes.html#index-13)

The index of the item in the list to retrieve.

#### Returns

- [AQLResponseProxy](aqlresponse_reference) | [Playwright Locator](https://playwright.dev/python/docs/api/class-locator)

The corresponding `AQLResponseProxy` list item.

---

### \_\_len\_\_

Returns the number of items in the response data if it is a list.

#### Usage

<Tabs labels={["Sync", "Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    QUERY = """
    {
        search_results[]
    }
    """

    aql_response = page.query_elements(QUERY)

    # Get the number of search results
    result_count = len(aql_response.search_results)
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    QUERY = """
    {
        search_results[]
    }
    """

    aql_response = await page.query_elements(QUERY)

    # Get the number of search results
    result_count = len(aql_response.search_results)
    ```
  </Tab>
</Tabs>

#### Returns

- [int](https://docs.python.org/3/library/stdtypes.html#index-13)

Number of items in the list.

---

### \_\_str\_\_

Returns a string representation of the response data in JSON format.

#### Usage

<Tabs labels={["Sync", "Async"]}>
  <Tab>
    ```python filename="agentql_example.py"
    QUERY = """
    {
        search_results[]
    }
    """

    aql_response = page.query_elements(QUERY)

    # Convert results to a JSON string for display
    result_in_json_string = print(aql_response.search_results)
    ```
  </Tab>
  <Tab>
    ```python filename="agentql_example.py"
    QUERY = """
    {
        search_results[]
    }
    """

    aql_response = await page.query_elements(QUERY)

    # Convert results to a JSON string for display
    result_in_json_string = print(aql_response.search_results)
    ```
  </Tab>
</Tabs>

#### Returns

- [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)
