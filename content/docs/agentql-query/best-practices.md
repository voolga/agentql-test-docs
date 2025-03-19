---
title: Best Practices for AgentQL Queries
authors:
  name: Frank Feng
  title: TinyFish Software Engineer
---

AgentQL queries allow users to retrieve the exact web page elements for interaction or data retrival. Designed with flexibility in mind, AgentQL queries are schema-less, meaning query elements are free-form and not strongly typed. However, there are some syntax requirements as well as best practices for creating an AgentQL query.

## AgentQL query syntax

The list below contains all syntax requirements for AgentQL Query:

* Enclose query with curly braces.
* Put new elements on new lines (i.e. one element per line).
* Do not separate elements with punctuation.
* A container element should enclose its children elements with curly braces.
* Create a list element by adding closed brackets '[]' after the term.

### Query examples

This is an example of a single-level query that tries to retrieve two elements (search box and search button) on the web page:

```AgentQL
{
    search_box
    search_btn
}
```

#### Nested query

This is an example of a nested query that tries to get a "sign in" button from a page's header and an "about" button from its footer. In this case, `<header />` and `<footer />` elements serve as container elements that capture the hierarchical relationship of the desired elements.

```AgentQL
{
    header {
        sign_in_btn
    }
    footer {
        about_btn
    }
}
```

#### Query that returns a list

This is an example of a query with list element. This query tries to capture all the links on a web page. The AgentQL server will return an array of links for this query:

```AgentQL
{
    links[]
}
```

#### Nesting and lists combined

This is another example of a query with list element. However, the query is specifying the exact information wanted in every list item. In this case, AgentQL server will return an array. Each array item will contain the price, the rating, and reviews of one product.

<Admonition type="note">
The list element can be nested. The `reviews[]` element tries to capture all reviews for the parent product.
</Admonition>

```AgentQL
{
    products[] {
        price
        rating
        reviews[]
    }
}
```

## Recommended practices for creating queries

AgentQL Query is designed to be flexible, but there are some recommended practices that may improve the response quality from AgentQL server:

* Use lowercase letters for all the terms in query.
* Use underscores (`_`) to separate words within a term (ie `user_image`).
* Append `btn` to the term to indicate the element is a clickable (ie `search_btn`).
* Append `box` to the name to indicate the element is inputtable (ie `search_box`).
* Indent children elements in accordance with their parent's indentation.

## How to find the exact element on the page

When there are multiple elements with the same or similar names on the web page, the AgentQL server may need further hints from your AgentQL query to find the exact element you are looking for. There are several things you can do with your query to help improve AgentQL's accuracy.

### Describe the element more specifically

More accurate or specific name is a powerful tool to get better query results. For instance, using “sign_in_with_google_btn” instead of “sign_in” may help with targeting a specific button on a page.

```AgentQL
{
    sign_in_with_google_btn
}
```

### Include hierarchy hints

Hierarchy hints reduce ambiguity. For example, if there are very similar buttons (f.i. “Sign In”) present on the web page, but one of them is positioned in the header and another one is in sign in form, you could try to specify such semantic information.

Consider the following example web page and queries:

![Linkedin WebPage and Queries](/images/docs/best-query-practice-p1.png)

Different container elements (`header` and `form`) will convey different hierarchical information to AgentQL server and locate different sign-in buttons on this page.

```AgentQL
{
    header_button
}
```

<Admonition type="note">
It can help to think of how you might increase a rule's specificity with a CSS selector.
</Admonition>

### Use surrounding elements

Another way to reduce ambiguity is to specify surrounding elements, so its more clear where the element is located. For example, if you are trying to locate “Sign in” button, which is placed between other 2 buttons, it may help to specify those other 2 buttons as well (even if you are not planning to interact with them) to clarify element location.

```AgentQL
{
    button_a
    sign_in_btn
    button_b
}
```

## Examples

Here shows some examples of working with real web pages.

### Retrieving Phone Model Button

In this example, the Agent query retrieves two buttons for different models by specifying their containing element.

```AgentQL
{
    model_selector {
        iphone_15_pro_max_btn
        iphone_15_pro_btn
    }
}
```

![Apple Page Model Buttons](/images/docs/best-query-practice-p2.png)

Alternatively, you could query the model selector itself. The button information will be preserved in the response, so you could retrieve the buttons through parsing.

```AgentQL
{
    model_selector
}
```

![Apple Page Model Selection Section](/images/docs/best-query-practice-p3.png)


### Retriving Amazon Product Information

This AgentQL query gets the name and price of each product on the Amazon product listing page.

```AgentQL
{
    results {
        products[] {
            product_name
            prouct_price
        }
    }
}
```

![Amazon Product Page](/images/docs/best-query-practice-p5.png)

If you want **all** the relevant information of each product, you could generalize the query above.

```AgentQL
{
    results {
        products[]
    }
}
```

![Amazon Product Page](/images/docs/best-query-practice-p6.png)
