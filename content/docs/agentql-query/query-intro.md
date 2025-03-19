---
title: AgentQL Query Introduction
---

The AgentQL query serves as the building block of your script. In this section, we will break down the query structure and highlight the possible formats a valid query can take.

### Single Term Query

A **single term query** enables you to retrieve a single element on the webpage. Here is an example of how you can write a single term query to retrieve a search box.

```AgentQL
{
    search_box
}
```

### List Term Query

A **list term query** enables you to retrieve a list of similar elements on the webpage. Here is an example of how you can write a list term query to retrieve a list of prices of apples.

```AgentQL
{
    apple_price[]
}
```

You can also specify the exact field you want to return in the list. Here is an example of how you can specify that you want the name and price from the list of products.

```AgentQL
{
    products[] {
        name
        price
    }
}
```

### Combining Single Term Queries and List Term Queries.

You can query for both **single terms** and **list terms** by combining the two formats above.

```AgentQL
{
    author
    date_of_birth
    book_titles[]
}
```

### Giving context to queries

There two main ways you can provide additional context to your queries.

#### Structural Context

You can nest queries within parent containers to indicate that your target web element is in a particular section of the webpage.

```AgentQL
{
    footer {
        social_media_links[]
    }
}
```

#### Semantic Context

You can also provide a short description within parentheses to guide AgentQL in locating the right element(s).

```AgentQL
{
    footer {
        social_media_links(The icons that lead to Facebook, Snapchat, etc.)[]
    }
}
```

### Syntax Guidelines

All AgentQL query terms must be enclosed within curly braces {}. The following query structure is invalid because the term `social_media_links` is wrongly enclosed within ().

```AgentQL
( # Should be {
    social_media_links(The icons that lead to Facebook, Snapchat, etc.)[]
) # Should be }
```

You cannot include new lines in your semantic context. The following query structure is invalid because the semantic context is not contained within one line.

```AgentQL
{
    social_media_links(The icons that lead
        to Facebook, Snapchat, etc.)[]
}
```
