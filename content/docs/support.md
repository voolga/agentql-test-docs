---
title: Support and help
---

We want you get the most out of AgentQL. If you encounter any issues or have questions, reach out to use on our community support channel.

## Where to get support

### Primary upport channel: Discord
Our Discord server is the heart of our community and the quickest way to get support. Here’s how to access it:

1. Join the [AgentQL Discord server](https://discord.gg/agentql).
2. Navigate to the **#support** channel.
3. Post your question or describe your issue.

Our AgentQL team members actively monitor this channel and will assist you as quickly as possible.

### Email support

If you prefer a more traditional route, you can reach us via email at support@agentql.com. We’ll respond as soon as we can.

## Guidelines for efective support requests

To help us provide the best possible support as quickly as possible, please follow these guidelines when posting in the **#support** channel:

1. **Be specific**: Clearly describe the issue you're facing or the question you have. Include any error messages you're seeing.

2. **Provide context**: Share relevant details about your environment, such as:
   - AgentQL version
   - Python version
   - Operating system
   - Browser (if applicable)

3. **Include a minimal reproducible example**: If possible, please provide a small code snippet that demonstrates the issue. This helps us understand and diagnose the problem more quickly.

4. **Share your AgentQL query**: Include the full query in your support request.

5. **Describe expected vs. actual behavior**: Explain what you expected to happen and what actually occurred.

6. **Use code formatting**: When sharing code or error messages, use Discord's code formatting (triple backticks: `/```/`) to make it easier to read.

## Example of a good support request

I'm having trouble extracting product prices from an e-commerce site. Here's my query:

```AgentQL
{
    products[] {
        name
        price
    }
}
```

I'm using AgentQL version 0.5.0 with Python 3.9 on Windows 10. When I run this query, I get all the product names correctly, but the prices are coming back as None. I expected to see the actual price values. Any ideas what might be causing this?
