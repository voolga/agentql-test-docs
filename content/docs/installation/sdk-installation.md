---
title: AgentQL SDK
---

## Prerequisites

- [Python 3.8 or higher](https://www.python.org/downloads/)

<Admonition type="note">
You may want to use a [virtual environment](https://www.freecodecamp.org/news/how-to-setup-virtual-environments-in-python/), but it is not required.
</Admonition>

## Installation options

There are two ways to install AgentQL SDK:

* [AgentQL CLI installation](#option-1-agentql-cli-installation) — Get set up fast by installing the AgentQL library and then using AgentQL CLI to download dependencies and setup AgentQL API Key.
* [Manual installation](#option-2-manual-installation) — For a more customized setup, manually install the AgentQL SDK.

### Option 1: AgentQL CLI Installation

#### 1. Install AgentQL library

From your terminal, run the following command to install the AgentQL library:


```bash
pip3 install agentql
```

#### 2. Install dependencies and set API key

The following AgentQL CLI command will prompt you for your API key. When prompted, simply copy and paste your AgentQL API key into the terminal.

```bash
agentql init
```

### Option 2: Manual Installation

From your terminal, run the following command to install the AgentQL library:

#### 1. Install AgentQL library

```bash
pip3 install agentql
```

#### 2. Install playwright driver

The default version of AgentQL Python SDK uses [Playwright](https://playwright.dev/) as a web driver, so Playwright dependencies need to be installed.

```bash
playwright install chromium
```

#### 3. Set your AgentQL API Key

Set the `AGENTQL_API_KEY` environment variable with your [API key](https://dev.agentql.com/).

<Tabs labels={["Linux/MacOS", "Windows"]}>
    <Tab>
        To set the environment variable temporarily for your terminal session,
        in your terminal run

        ```bash
        export AGENTQL_API_KEY=your-api-key
        ```
    </Tab>

    <Tab>

    #### Powershell

    If you are using **Powershell** as your terminal, you can set the environment variable with the following command

    ```bash
    $env:AGENTQL_API_KEY="your-api-key"
    ```

    #### Command Prompt

    If you are using **Command Prompt** as your terminal, you can set the environment variable with the following command

    ```bash
    set AGENTQL_API_KEY=your-api-key
    ```

    </Tab>

</Tabs>

## Run Your First AgentQL Script

Now you are ready to run your first AgentQL script! Continue to [First Steps](/getting-started/first-steps) to get started.
