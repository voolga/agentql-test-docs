---
title: Frequently Asked Questions
---

### **Q.** How do I debug my script?

**Ans:** Since the release of `0.5.0` version, AgentQL SDK has an internal debug mode, which is implemented as a context manager that could be wrapped around your script. Once a crash happens or the script finishes running, the debug mode will save the following contents to the folder path specified during setup process or `AGENTQL_DEBUG_PATH` environment variable (default path is `YOUR_HOME_PATH/.agentql/debug`):

1. Log of each action taken by AgentQL SDK.
2. Error Information (if the script crashes).
3. Screenshot of each page on which a `query` action is performed.
4. [Accessibility Tree](https://developer.mozilla.org/en-US/docs/Glossary/Accessibility_tree) of the last page before the crash or the end of script.
5. Meta information (OS, Python version, AgentQL version)

The following script is an example of how you could use this debug mode:

```python
import logging
import agentql
from agentql.sync_api import DebugManager
from playwright.sync_api import sync_playwright

logging.basicConfig(level=logging.DEBUG)
log = logging.getLogger(__name__)

QUERY = """
{
    search_box
    search_btn
    about_link
}
"""

# The following context manager will enable debug mode for the script.
with DebugManager.debug_mode():
    with sync_playwright() as playwright, playwright.chromium.launch() as browser:
        page = agentql.wrap(browser.new_page())
        page.goto("https://www.google.com")

        log.debug("Analyzing...")
        response = page.query_elements(QUERY)

        log.debug("Inputting text...")
        # Buggy code that will crash the script. When it crashes, the debug manager will save debug files to designated directory (~/.agentql/debug by default).
        response.search.type("tinyfish")

        log.debug('Clicking "Search" button...')
        response.search_btn.click()
```

### **Q.** How do I specify the format of data returned by AgentQL?

**Ans:** Since the release of `0.4.7` version, AgentQL supports providing context to query in the following way:

```AgentQL
{
    products[] {
        price(Format the output as a numerical value only, no dollar sign)
        name(Format the output with the following prefix: amazon_)
    }
}
```

As you can see, by providing formatting instructions as a context into the query, you could instruct AgentQL to return the expected data format.

### **Q.** While writing AgentQL Script, how do I ensure the web page is loaded entirely?

**Ans:** Loading web page entirely right now is considered as an application logic, as it is difficult to objectively tell what does it mean that entire web page is loaded. As some websites have lazy loading and are infinitely scroablle and so for those kind of web pages what does it mean to load the entire web page.

You can use [Playwright API](https://playwright.dev/python/docs/api/class-mouse#mouse-wheel) to scroll the page up and down to load the more content.

```python
import agentql
from playwright.sync_api import sync_playwright

QUERY = """
{
    video_comments[] {
        user_name
        rating
        comment
    }
}
"""

with sync_playwright() as playwright, playwright.chromium.launch() as browser:
    page = agentql.wrap(browser.new_page())
    page.goto("https://www.youtube.com/watch?v=qQkBeOisNM0")

    for _ in range(3):
        # Scroll down 3 times, each by 1000 pixels
        page.mouse.wheel(delta_x=0, delta_y=1_000)
        page.wait_for_page_ready_state()  # Give it the opportunity to load more content

    page.mouse.wheel(delta_x=0, delta_y=-3_000)  # Scroll up 3000 pixels, back to top

    response = page.query_data(QUERY)
    print(f"Video comments: \n{response}")
```

### **Q.** How do I make sure that my script waits enough for all the element to appear on the page?

**Ans:**
AgentQL SDK provides the `wait_for_page_ready_state()` API to determine page readiness. This method waits for the page to be in a ready state and all the necessary network requests to be completed. However, there are some cases where the page might not be ready even after the method returns. This could be due to the dynamic nature of the page. In such cases, you can use the `time.sleep()` function to add a additional delay to your script.

```python
import time
import agentql
from playwright.sync_api import sync_playwright

QUERY = """
{
    related_videos[] {
        video_name
        video_rating
    }
}
"""

with sync_playwright() as playwright, playwright.chromium.launch() as browser:
    page = agentql.wrap(browser.new_page())
    page.goto("https://www.youtube.com/watch?v=qQkBeOisNM0")

    page.wait_for_page_ready_state()  # Wait for the page to fully load

    time.sleep(5)  # Give additional time if needed

    response = page.query_data(QUERY)
    print(f"Related videos: \n{response}")
```
