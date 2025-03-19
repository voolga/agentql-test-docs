---
title: Release Notes
---

## Version 0.5.3

### Fixes

- Fixed accessibility tree generation creating duplicate IDs for web elements.
- Optimized accessibility tree generation by including nodes with `aria-hidden=true` attribute by default.
- Adjusted how API key is checked so that keys set through environment variable will take precedence over those set in config file.

### Improvements

- Debug mode now generates `request_id` information for each `query` request. Users can share this information with Tiny Fish developers when asking for help with a specific query.

## Version 0.5.2

### New Features

- Query Data

Previously, AgentQL adhered to a one-to-one relationship between query terms and web elements, which sometimes made it difficult to query a block of text or retrieve actual text value from responses. Now, users can easily achieve these tasks with the newly added `session.query_data()` method. The following example demonstrates how to use this endpoint:

```python filename="agentql_example.py"
import agentql

session = agentql.start_session("https://apply.workable.com/pony-dot-ai/j/56A463E1D3/")

QUERY = """
{
    required_programming_skills (just the skill name)[]
    base_salary_min (without the dollar sign, use _ as separator)
    base_salary_max (with dollar sign)
}
"""

response = session.query_data(QUERY)

# Text of the query terms could be directly retrieved in the following way
print(f"Base salary min: {response.base_salary_min}")
print(f"Base salary max: {response.base_salary_max}")

for skill in response.required_programming_skills:
    print(f"Required programming skill: {skill}")

session.stop()
```

### Fixes

- Improved accessibility tree generation logic by removing HTML elements with the `code` tag.

## Version 0.5.1

### Fixes

- Fixed the script getting stuck in an infinite loop when the starting character and the ending character of the query are the same, but they are not quotation marks

## Version 0.5.0

### Breaking changes

- Modules structure update. Playwright web drivers are now located in `agentql.ext.playwright.sync_api` and `agentql.ext.playwright.async_api` for synchronous and asynchronous versions respectively:

```python filename="agentql_example.py"
from agentql.ext.playwright.sync_api import PlaywrightWebDriver
from agentql.ext.playwright.async_api import PlaywrightWebDriver
```

### New features

- Debug Mode

Users can now use AgentQL SDK's Debug Mode to debug their scripts. The following example demonstrates how to enable this mode:

<Tabs labels={["Sync", "Async"]}>
  <Tab>

    ```python filename="agentql_example.py"
    from agentql.sync_api import DebugManager

    with DebugManager.debug_mode():
        your_script
    ```

  </Tab>
  <Tab>

    ```python filename="agentql_example.py"
    from agentql.async_api import DebugManager

    async with DebugManager.debug_mode():
        your_script
    ```

  </Tab>
</Tabs>

It will save meta information (like OS, Python version, AgentQL version), logs, error information, last accessibility tree used, and screenshots of every page queried to the debug folder. The default path is `$HOME/.agentql/debug`.

- Query Terms' Context

Previously, when describing a term in the query, users would need to do something like this:

```AgentQL
{
    second_button_from_the_top_next_to_login_button_only_if_hero_image_is_present
}
```

Now, AgentQL Query supports providing context for the query terms. Add it inside parentheses like this:

```AgentQL
{
    button(This is the second button from the top. It is next to login button and will only appear when hero image is present)
}
```

For more details, please check out our [query introduction page](/agentql-query/query-intro).

- Search in Documentation Website

Users can now search for keywords in AgentQL Documentation Website.

### Improvements

- Supported iterating over query's collection data items via `for` loop
- Improved typechecking for AgentQL response
- Added AgentQL config file path to the `agentql init` command's output

### Fixes

- Fixed a crash in `wait_for_page_ready_state()` method when it was invoked before page redirection
- Fixed `session.current_page` not updating when opening new tab after clicking on a link

## Version 0.4.7

### Fixes:

- Refactored part of the internal logic of query syntax

## Version 0.4.6

### Improvements:

- Improved the error message for a better debugging experience
- Allowed History log to output logging information even when an error is raised

### Fixes:

- Fixed scrolling not working on some websites
- Fixed accessibility tree not being captured correctly on some websites

## Version 0.4.5

### Improvements:

- Added **AgentQL CLI** which is a tool designed to assist you in using the AgentQL SDK. It can help you set up your development environment.
- Added **Trail Logger** which can log actions taken by AgentQL SDK and display them at the end of a session. This can be used for debugging your scripts. The Trail Logger can be enabled through `enable_history_log` parameter in `start_session()` method and the logs can be obtained through `session.get_last_trail()`.
- Added `Session#last_accessibility_tree` property to get the last captured accessibility tree. It can be helpful for debugging purposes.
- Added `Popup#page_url` property to get the URL of the page where the popup occurred. It can be used when analyzing popup on different pages.
- Adjusted the error message for `AttributeNotFoundError` for better debugging information.
- Moved the import path for `ProxySettings`, `Locator` and `Page` class to `agentql.ext.playwright`.

### Fixes:

- Fixed some web pages with empty iframes HTML element crashing the accessibility tree generation logic.
- Fixed `wait_for_page_ready_state() ` not reliably waiting on some websites.

## Version 0.4.4

### Fixes:

- Addressed incorrect hidden elements detection logic

## Version 0.4.3

### Fixes:

- Fixed some web page elements being incorrectly marked as "hidden" and not included in the query result.

## Version 0.4.2

### Fixes:

- Fixed the page not being closed when the session was closed.

## Version 0.4.1

### Fixes:

- Fixed SDK crash to enable async SDK usage and multiple sync sessions.
- Fixed a potential resource leak issue during session creation failures.

## Version 0.4.0

### Breaking changes

- Major modules structure overhaul. Please refer to the [corresponding Blog Post](/blog/modules-structure-overhaul) for more details.
- Playwright Web Driver now starts in "headed" mode by default. To start it in "headless" mode, users need to pass `headless=True` to the `PlaywrightWebDriver` constructor.

## Version 0.3.1

### Fixes:

- Fixed SDK crash on Python versions < 3.10

### Improvements:

- Added `Session#last_query` and `Session#last_response` methods to get the last response and query objects respectively. These can be helpful for debugging purposes.

## Version 0.3.0

<Admonition type="caution">
We've migrated our SDK from webql to agentql to be consistent with our new branding! This release introduces breaking changes. Please refer to "Breaking Changes" section for latest information.
</Admonition>

### Breaking changes

- As we have moved our SDK from webql to agentql, our Python library is now called `agentql` and you can import the same with `import agentql`

- API key setup, instead of `WEBQL_API_KEY`, now the users need to set `AGENTQL_API_KEY`.

<Admonition type="note">
We have also updated our docs to reflect those changes! The underlying APIs available and the way they can be leveraged are still the same.
</Admonition>

## Version 0.2.8

### Hotfix release

- Fixed `TypeError: AsyncClient.post() got an unexpected keyword argument 'allow_redirects'`

## Version 0.2.7

<Admonition type="caution">
This release introduces some breaking changes. Please refer to "Breaking Changes" section for latest information.
</Admonition>

### Breaking changes

As we continue drawing a clearer line between Session and WebDriver, we removed several APIs which were previously present in `Session` class:

```python filename="agentql_example.py"
# Removed APIs
session.scroll_up()
session.scroll_down()
session.scroll_to_bottom()
session.load_user_session_state()
session.wait_for_page_ready_state()
session.get_user_session_state()
session.save_user_session_state()
```

All these methods are now available in `WebDriver` class, so you can use them in the following way:

```python filename="agentql_example.py"
session.driver.scroll_up()
session.driver.scroll_down()
session.driver.scroll_to_bottom()
session.driver.load_user_session_state()
session.driver.wait_for_page_ready_state()
session.driver.get_user_session_state()
session.driver.save_user_session_state()
```

### Improvements

- Fixed possible crash in PlaywrightDriver related to unbound variable (#252)
- Allowed http redirects for AgentQL API calls (#257)
- Fixed resource leak: reuse existing browser context for iframes (#259)
- Fixed resource leak: dom update listener is never removed (#258)
- Moved to tf-playwright-stealth (#260)
- Relaxed dependency requirements (#261)
- Added environment variable to control API host (#262)

## Version 0.2.6

### Improvements

- Optimized the code by making `enable_stealth_mode()` method sync in Asynchronous version of SDK.

## Version 0.2.5

### Highlights

This release introduces public APIs for checking whether web driver is in `headless` mode and for retrieving `web driver` instance in `Session` class. Several bug fixes and code optimization are also included in this release.

### New Features

- API to retrieve `web driver` instance from `Session`

Users can now interact with the web driver instance directly from `Session` class in the following way:

```python filename="agentql_example.py"
# This will scroll to the bottom of the page
session.driver.scroll_to_bottom()

# This will wait for page to enter a stable state
session.driver.wait_for_page_ready_state()
```

- API to retrieve `headless` setting

Users can now determine whether the browser is started in `headless` mode by invoking `session.driver.is_headless()`.

### Bug Fixes

- Fixed a bug where users can not chain methods for response object.

## Version 0.2.4

### Highlights

This release introduces `Stealth Mode` to SDK. Stealth mode will **decrease users' possibility of being marked as bot** on some websites.

### New Features

- Stealth Mode

Users can enable stealth mode by invoking `enable_stealth_mode()` method in `Web Driver` class. Users can pass in their `User Agent`, `webgl renderer`, and `webgl vendor` information to maximize the effect of stealth mode.

Users can activate the `Stealth Mode` like this:

```python filename="agentql_example.py"
import webql as wql
from webql.sync_api.web import PlaywrightWebDriver

driver = PlaywrightWebDriver(headless=False)

# Enable the stealth mode and set the stealth mode configuration
driver.enable_stealth_mode(
    webgl_vendor=VENDOR_INFO,
    webgl_renderer=RENDERER_INFO,
    nav_user_agent=USER_AGENT_INFO,
)
```

## Version 0.2.3

### Highlights

This release improves the stability and reliability of SDK by introducing fixes to some known bugs.

### Bug Fixes

- Fixed a bug where page interaction sometimes froze in headless mode.
- Fixed a bug for data postprocessing in async environment.

## Version 0.2.2

### Highlights

This release introduces a new API through which users can retrieve `Page` object from web driver. In addition, this release also includes several bug fixes and code optimization.

### New Features

- New public API for getting `Page` object from web driver

A public API has been added to `Session` class for retrieving `Page` object. With the `Page` object, users can interact with web pages more freely, such as page refreshing and navigation.

For instance, to refresh the page, users can use the following script:

```python filename="agentql_example.py"
session = webql.start_session()

# This will reload the current web page
session.current_page.reload()
```

To navigate to a new website, users can use the following script:

```python filename="agentql_example.py"
session = webql.start_session()

# This will take the page to a new website
session.current_page.goto("new website link")
```

### Bug Fixes

- Fixed a bug where None value in response data is not handled properly.
- Fixed a bug where to_data() method is not working properly in the asynchronous environment.

## Version 0.2.1

### Highlights

This release introduces a new feature where users can retrieve and load browser's authentication session to maintain login state.

### New Features

- Get & Set User Authentication Session:

With this release, users can maintain the previous login state by initializing a session with the user authentication state.

To retrieve the authentication state from the current session, users can utilize `Session` class's `get_user_auth_state()`:

```python filename="agentql_example.py"
# Prior to this point, the script has already signed into a website

# This will retrieve the auth state for current session
user_auth_state = session.get_user_auth_state()

# The session info can be saved to local file system like this
with open(FILE_PATH, "w") as f:
    f.write(json.dumps(user_auth_state))
```

To load the authentication state while initializing the session, users can pass `user_auth_state` into `start_session()`'s `user_auth_session` parameter:

```python filename="agentql_example.py"
user_auth_session = None

# To load user_auth_session from local file, users can do something like this
with open(FILE_PATH, "r") as f:
    user_auth_session = json.loads(f.read())

session = webql.start_session(user_auth_session=user_auth_session)
```

For a more detailed instruction on how to retrieve and load user session, please refer to the [following example](https://github.com/tinyfish-io/fish-tank/blob/main/examples/save_and_load_context/save_and_load_context.py) in our example repository.

## Version 0.2.0

<Admonition type="caution">
This release introduces some breaking changes. Please refer to "Breaking Changes" section for latest information.
</Admonition>

### Highlights

This release introduces the asynchronous version of the package. Now users can utilize AgentQL in an optimized fashion within their asynchronous environment.

### New Features

- Asynchronous Support:
  With this release, users can start an asynchronous session using the following script:

```python filename="agentql_example.py"
import webql

async_session = await webql.start_async_session()
```

For a more detailed instruction on how to use async version, please refer to the [following example](https://github.com/tinyfish-io/fish-tank/blob/main/examples/async_example/async_example.py) in our example repository.

### Breaking Changes

We have introduced some changes to our public API structure. Specifically, **users need to choose between synchronous API and asynchronous API when importing web drivers and helper methods**.

Now, `PlaywrightWebDriver` and `close_all_popups_handler` need to be imported in the following fashion:

- Synchronous

```python filename="agentql_example.py"
from webql.sync_api.web import PlaywrightWebDriver
from webql.sync_api import close_all_popups_handler
```

- Asynchronous

```python filename="agentql_example.py"
from webql.async_api.web import PlaywrightWebDriver
from webql.async_api import close_all_popups_handler
```

The following way of importing `PlaywrightWebDriver` and `close_all_popups_handler` is no longer supported.

<Admonition type="warning">
The following script is deprecated and no longer supported.
</Admonition>

```python filename="agentql_example.py"
from webql.web import PlaywrightWebDriver
from webql import close_all_popups_handler
```
