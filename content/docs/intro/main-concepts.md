---
title: Main Concepts
---

AgentQL is an AI-powered query language and a suite of tools designed to change how developers retrieve elements and data from web sites. It uses a natural language query along with analysis of the page to precisely and efficiently return web elements and data. It consists of the following main components:

- [**AgentQL Query Language**](#agentql-query-language) simplifies the process of locating web elements with plain language.
- [**AgentQL Python SDK**](#agentql-python-sdk) integrates AgentQL queries into automation scripts.
- [**AgentQL Debugger (Chrome Extension)**](#agentql-debugger) enables interactive testing of AgentQL queries in real time.

## Core Components

### AgentQL Query Language

The AgentQL Query Language is the heart of the system. It uses a natural language-like syntax that allows developers to describe web elements and data in an intuitive, human-readable way:

```AgentQL
{
    search_input
}
```

In this query, `search_input` is the element being located, but you could easily return a different element by using an alternative term of your own, like `first_name_input`, or `search_button` or `search_btn`. This approach abstracts away the complexity of DOM traversal. It is flexible and adaptable to changes in webpage structure and resilient to website updates that would break traditional selectors like XPath or DOM or CSS selectors. It can return both elements for interaction and data for extraction.

### AgentQL Python SDK

The AgentQL Python SDK is the primary interface for integrating AgentQL into automation scripts, web scrapers, and testing frameworks like Playwright. Key features include:

* Simple API for executing queries and handling responses
* Support for both synchronous and asynchronous operations
* First class integration with popular web automation tool Playwright
* Comprehensive error handling and debugging capabilities

### AgentQL Debugger

Using the AgentQL Debugger Chrome Extension, developers can efficiently iterate on their queries, ensuring they work correctly before integrating them into larger automation scripts. It lets you write queries and see the elements AgentQL finds in real-time.

* Write and test queries in real-time against live webpages
* Visualize matched elements directly on the page
* Iterate quickly on query refinements
* Understand how AgentQL interprets different page structures

Please refer to the [AgentQL Debugger](/installation/chrome-extension-installation) documentation for detailed instructions on how to install and use the extension.

## Methods of interaction

AgentQL provides 2 ways of interacting with web content:

### Element Queries

AgentQL’s [`query_elements()`](/api-references/agentql-page\#queryelements) method returns web elements that can be manipulated programmatically. It is useful for:

* Interacting with page's elements and navigation
* Extracting properties of specific elements

AgentQL’s [`get_by_prompt()`](/api-references/agentql-page\#getbyprompt) method returns a single web element using a natural language prompt (as opposed to a full query).

### Data Queries

AgentQL’s [`query_data()`](/api-references/agentql-page\#querydata) method returns structured information from webpages. It is useful for:

* Scraping product information, prices, or reviews
* Collecting article content or metadata
* Extracting tabular data from complex layouts

## Conclusion

Understanding these main concepts—AgentQL Query, AgentQL Debugger, and AgentQL SDK—is crucial for leveraging the full power of AgentQL in your web automation tasks. Each component plays a distinct role in simplifying and enhancing the development process, making AgentQL an indispensable tool for modern web automation.

<Cards>

  <Card title="Quick Start" description="Get up and running with AgentQL in less than five minutes." icon="doc" href="/quick-start" />

  <Card title="Why AgentQL?" description="AgentQL provides many advantages over traditional query methods." icon="doc" href="/intro/why-agentql" />

  <Card
    title="AgentQL Queries" description="Learn the syntax for writing your own Queries."
    href="/agentql-query/query-intro"
    icon="doc" />

</Cards>
