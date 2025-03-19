---
title: Why AgentQL?
---

AgentQL is a reliable, flexible, and intuitive query language for locating web elements. Developers can specify web elements easily with its natural language-like query syntax without diving into complex DOM structures or writing fragile XPath expressions. AgentQL can return data or web elements with queries and prompts.

This example demonstrates an AgentQL query that can be used locate a search box and its button on a web page.

```AgentQL
{
    search_input_field
    search_button
}
```

You can pass an AgentQL query to one of two query methods: [`query_elements`](/api-references/agentql-page#queryelements) returns elements, and [`query_data`](/api-references/agentql-page#querydata), which returns the data in those elements.

AgentQL can also return an element from a prompt using its  [`get_by_prompt`](/api-references/agentql-page#getbyprompt) method:

```python filename="example_script.py"
page.get_by_prompt("the search input field")
```

## Advantages over traditional methods

XPath and CSS selectors have been the go-to solutions for web scraping and automation, but they come with drawbacks:

1. **Fragility**: Traditional selectors break easily when websites update their structure.
2. **Complexity**: XPath and complex CSS selectors can be difficult to write and maintain.
3. **Lack of Context**: Traditional selectors don't understand the meaning or purpose of elements.

AgentQL lets you spend less time updating broken selectors and more time extracting data and building automations by solving for these problems:

* **Robustness:** AgentQL adapts to changes in website structure automatically
* **Simplicity:** AgentQL queries use natural language descriptions instead of rigid syntax
* **Context-aware:** AgentQL uses AI to understand the context and meaning of web elements

## Features

### Semantic Selectors

AgentQL uses AI to build a semantic understanding of the context surrounding the web elements on a page. Elements are found based on their meaning and context, not just their position in the DOM, making them more:

* **Stable** even when website layouts change over time.
* **Reusable** across sites, standardizing outputs.
* **Intuitive** for both developers and non-technical users to write

### Natural Language Queries

With AgentQL, you describe what you're looking for in plain English and can even pass entire prompts to explain in great detail what you’re looking for on a page. This makes queries more readable across teams and time and more maintainable.

### Controlled Output

Your query defines the structure of your output data, eliminating post-processing steps.

### Deterministic Results

AgentQL not only allows you to define an exact response structure, but also provides consistent and reliable results. You will get the same output for the same input, every time. This lets you confidently automate processes and tests.

## Primary Use Cases

AgentQL is useful for data scraping and extraction, web automation, and testing.

### Data Scraping

AgentQL can extract structured data from websites:

* Gather pricing details from multiple storefronts
* Collect social media metrics
* Aggregate news and articles from multiple sources

### Web Automation

Streamline repetitive tasks and complex workflows:

* Automate form submissions
* Interact with web applications programmatically
* Create powerful web-based bots and agents

### End-to-End Testing

Build more robust and maintainable test suites:

* Write tests that are resistant to UI changes
* Reduce test flakiness and maintenance overhead

## Conclusion

AgentQL can change how you interact with web content. If you’d like to dive in, check these out:

<Cards>

  <Card title="Quick Start" description="Get up and running with AgentQL in less than five minutes." icon="doc" href="/quick-start" />

  <Card
    title="Main Concepts"
    description="Understand how AgentQL and its developer tools work together." href="/intro/main-concepts"
    icon="doc" />

  <Card
    title="AgentQL Queries" description="Learn the syntax for writing your own Queries."
    href="/agentql-query/query-intro"
    icon="doc" />

  <Card
    title="Examples"
    description="Fork some example Python apps and start coding."
    href="/examples"
    icon="doc" />

</Cards>
