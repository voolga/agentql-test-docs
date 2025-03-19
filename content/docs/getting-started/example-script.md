---
title: Example Script for Collecting YouTube Comment Data
---

In this guide, you will learn how to use an AgentQL script to navigate to a YouTube video and collect comment data.

## Prerequisites

- [The AgentQL SDK](/installation/sdk-installation)

## Instructions

<Admonition type="note">
If you'd like to start with the full example script, you can [find it at the end of the tutorial](#putting-it-all-together).
</Admonition>

### Step 0: Create a New Python Script

In your project folder, create a new Python script and name it `example_script.py`.

### Step 1: Import Required Libraries

Import needed functions and classes from `playwright` and `agentql` libraries and import the `logging` library.

```python filename="example_script.py"
import logging
import agentql
from playwright.sync_api import sync_playwright
```

`logging` provides logging, debugging, and information messages, `playwright` provides core browser interaction functionality and `agentql` adds the main AgentQL functionality.

### Step 2: Launch the Browser and Open the Website

```python filename="example_script.py"
logging.basicConfig(level=logging.DEBUG)
log = logging.getLogger(__name__)
URL = "https://www.youtube.com/"

with sync_playwright() as playwright, playwright.chromium.launch(headless=False) as browser:
    page = agentql.wrap(browser.new_page())
    page.goto(URL)
```

- Set up logging: `logging.basicConfig(level=logging.DEBUG)` configures the logging to show debug-level messages.
- Define `URL`: The URL `"https://www.youtube.com/"` is the target website for the script.
- Start Playwright instance with `sync_playwright()`.
- Launch the browser: `playwright.chromium.launch(headless=False)`
- Create a new page in the browser and wrap it to get access to the AgentQL's querying API: `agentql.wrap(browser.new_page())`.
- Navigate to the website with `page.goto(URL)`.

### Step 3: Define AgentQL Queries

```python filename="example_script.py"
SEARCH_QUERY = """
{
    search_input
    search_btn
}
"""

VIDEO_QUERY = """
{
    videos[] {
        video_link
        video_title
        channel_name
    }
}
"""

VIDEO_CONTROL_QUERY = """
{
    expand_description_btn
}
"""

DESCRIPTION_QUERY = """
{
    description_text
}
"""

COMMENT_QUERY = """
{
    comments[] {
        channel_name
        comment_text
    }
}
"""
```

These queries provide the tool for communication and interaction with the right elements on the website. Ensuring you have functional and reliable queries is paramount!

<Admonition type="tip">
Use the AgentQL Chrome Extension in parallel with the AgentQL SDK to test different query formats and keywords directly with the webpage!
</Admonition>

### Step 4: Try & Except Block

```python filename="example_script.py"
try:

    # query logic

except Exception as e:
    log.error(f"Found Error: {e}")
```

Catching Errors: Since an AgentQL query will either return an `AQLResponseProxy` or throw an error during the querying process, best practice suggests to encapsulate query logic with a try and catch block for better debugging.

### Step 5: Execute Search Query and Interact with Search Elements

```python filename="example_script.py"
response = page.query_elements(SEARCH_QUERY)
response.search_input.type("machine learning", delay=75)
response.search_btn.click()
```

- Search Query: Here we pass the `SEARCH_QUERY` to query specific elements on the page to interact with the search elements on YouTube page.
- Type and Click: It types `"machine learning"` into the search input with a delay of 75ms between keystrokes and then clicks `search_btn`

<Admonition type="tip">
More information on the `type()` and other available interaction APIs you can find in the official [Playwright documentation](https://playwright.dev/docs/api/class-locator).
</Admonition>

<Admonition type="tip">
Use the AgentQL `to_data()` API when you want to convert the AgentQL Response into `dict` of data and work with it rather than treating it as web elements!
</Admonition>

### Optional Step: Convert AgentQL response to a Python dict

Since the raw response is an AgentQL response object, it is not easy to work with it. You can use the `to_data()` API to convert the response to a Python `dict`.

```python filename="example_script.py"
SAMPLE_DATA_QUERY = """
{
    videos[] {
        title
        date_posted
        views
    }
}
"""
response = page.query_elements(SAMPLE_DATA_QUERY)

log.debug(response.to_data())
```

The `to_data()` API converts the AgentQL response to a structured dict in which it replaces the response nodes with text contents of the nodes.

Sample result would be as follows:

```json filename="Response Data"
{
  "videos": [
    {
      "title": "This is a nice video!",
      "date_posted": "1 month ago",
      "views": "2.7K Views"
    },
    {
      "title": "This maybe a better video!",
      "date_posted": "3 months ago",
      "views": "2.7 Million Views"
    },
    {
      "title": "This is best video!",
      "date_posted": "1 year ago",
      "views": "37.5K Views"
    }
  ]
}
```

### Step 6: Execute Video Query and Interact with Video Elements

```python filename="example_script.py"
response = page.query_elements(VIDEO_QUERY)
log.debug(
    f"Clicking Youtube Video: {response.videos[0].video_title.text_content()}"
)
response.videos[0].video_link.click()  # click the first youtube video
```

- Video Query: The script runs the `VIDEO_QUERY` to interact with video elements on the search results page.
- Click on Video: It clicks on the first video link in the results.
- Logging: A debug message logs the title of the clicked video.

### Step 7: Control Video Playback and Show Description

```python filename="example_script.py"
response = page.query_elements(VIDEO_CONTROL_QUERY)
response.expand_description_btn.click()
```

- Video Control Query: Run the `VIDEO_CONTROL_QUERY` to interact with video controls.

### Step 8: Capture and Log Video Description

Instead of converting the intermediate AgentQL response to `dict` via `to_data()` API you can call `page.query_data()` method to query for structured data at the first place.

```python filename="example_script.py"
response_data = page.query_data(DESCRIPTION_QUERY)
log.debug(
    f"Captured the following description: \n{response_data['description_text']}"
)
```

- Description Query: Executes the `DESCRIPTION_QUERY`.
- Logging Description: Logs the captured description of the video.

### Step 9: Scroll Down the Page to Load Comments

To load comments on a YouTube video page, we need to scroll down the page a few times.

```python filename="example_script.py"
for _ in range(3):
    page.keyboard.press("PageDown")
    page.wait_for_page_ready_state()
```

- Press `PageDown` Button: Presses the `PageDown` button to scroll down the page.
- Wait for Page Ready State: Waits for the comments to load with AgentQL's `wait_for_page_ready_state()` method.

### Step 10: Capture and Log Comments

```python filename="example_script.py"
response = page.query_elements(COMMENT_QUERY)
log.debug(f"Captured {len(response.comments)} comments!")
```

- Execute Query: Pass the session our `COMMENT_QUERY` to capture comment section data.
- Count and Log: Here we simply log the number of comments captured.

### Step 11: Stop the Browser

```python filename="example_script.py"
browser.close()
```

- Call `browser.close()` to releasing resources and close the browser.

### Step 12: Run the Script

Open a terminal in your project's folder and run the script:

```bash
python3 example_script.py
```

## Putting it all together…

Here is the complete script, if you'd like to copy it directly:

```python filename="example_script.py"
import logging
import time
import agentql
from playwright.sync_api import sync_playwright

logging.basicConfig(level=logging.DEBUG)
log = logging.getLogger(__name__)
URL = "https://www.youtube.com/"

with sync_playwright() as playwright, playwright.chromium.launch(headless=False) as browser:
    page = agentql.wrap(browser.new_page())
    page.goto(URL)

    SEARCH_QUERY = """
    {
        search_input
        search_btn
    }
    """

    VIDEO_QUERY = """
    {
        videos[] {
            video_link
            video_title
            channel_name
        }
    }
    """

    VIDEO_CONTROL_QUERY = """
    {
        play_or_pause_btn
        expand_description_btn
    }
    """

    DESCRIPTION_QUERY = """
    {
        description_text
    }
    """

    COMMENT_QUERY = """
    {
        comments[] {
            channel_name
            comment_text
        }
    }
    """

    try:
        # search query
        response = page.query_elements(SEARCH_QUERY)
        response.search_input.type("machine learning", delay=75)
        response.search_btn.click()

        # video query
        response = page.query_elements(VIDEO_QUERY)
        log.debug(f"Clicking Youtube Video: {response.videos[0].video_title.text_content()}")
        response.videos[0].video_link.click()  # click the first youtube video

        # video control query
        response = page.query_elements(VIDEO_CONTROL_QUERY)
        response.expand_description_btn.click()

        # description query
        response_data = page.query_data(DESCRIPTION_QUERY)
        log.debug(f"Captured the following description: \n{response_data['description_text']}")

        # Scroll down the page to load more comments
        for _ in range(3):
            page.keyboard.press("PageDown")
            page.wait_for_page_ready_state()

        # comment query
        response = page.query_elements(COMMENT_QUERY)
        log.debug(f"Captured {len(response.comments)} comments!")

    except Exception as e:
        log.error(f"Found Error: {e}")
        raise e

    # Used only for demo purposes. It allows you to see the effect of the script.
    time.sleep(10)
```
