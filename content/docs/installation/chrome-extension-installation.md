---
title: AgentQL Debugger Extension
---

AgentQL's Chrome Extension lets you query data and elements from websites directly from your browser. Use it to test and perfect queries before using them with the [AgentQL SDK](sdk-installation) to automate tasks, scrape data, or write tests on the web.

## Prerequisites

- Your [AgentQL API key](https://dev.agentql.com/)

## Installation

1. Install the AgentQL Chrome Extension from the Chrome Web Store [here](https://chromewebstore.google.com/detail/agentql-ide/idnejmodeepdobpinkkgpkeabkabhhej?hl=en&authuser=0).
   ![Add Chrome Extension](/images/docs/chrome-extension-p3.png)

2. Press the **Ctrl+Shift+I** keys or **Cmd+Opt+I** on Mac to bring up Chrome's devtools panel.

3. If you don't see "AgentQL" in the top bar of the devtools panel, click on the overflow menu button (») and select "AgentQL" from the list.

    <Admonition type="note">
    The AgentQL tab will only appear when inspecting websites with URLs.
    </Admonition>
   ![Selecting AgentQL tab](/images/docs/chrome-extension-install.webp)

4. Enter your API key ([get one here](https://dev.agentql.com/) if you don't have one).

   ![AgentQL tab](/images/docs/chrome-extension-p1.png)

5. You're ready to query!

   ![Query demo](/images/docs/demo-1.webp)

## Try it out

To get a feel for how the AgentQL query language works, paste the following query into the extension:

```AgentQL
{
    tutorial_images[]
}
```

#### Fetch Web Element

Click "Fetch Web Elements" to locate the precise elements on the webpage. Hover over the returned element to see it highlighted on the page. (Can't see it? Try clicking the eye icon <img src="/images/eye.svg" alt="eye svg" style={{display: "inline", width:"18px", height:"18px", margin:"0px"}} /> to scroll it into view!) You can also click on the `</>` to see the element highlighted in the DevTools' "Elements" panel.

#### Fetch Data

Click "Fetch Data" to retrieve URLs for all images included in this tutorial—except for the logo at the top of the page. AgentQL is clever enough to know that the logo is not part of the tutorial content. What other clever things can you get AgentQL to do?

Try "Fetch Web Elements" and "Fetch Data" for the following query to get the next page in the documentation:

```AgentQL
{
    next_page_link
}
```

Now learn how to use your query with the [AgentQL SDK](sdk-installation).

## Troubleshooting

If you cannot find the extension in your DevTools, try the following:

- Make sure you're on a website with a URL.
- Try reopening your DevTools panel.
