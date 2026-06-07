# HigherGov MCP

{% hint style="info" %}
This feature is currently in beta.  If you have use cases not currently supported, please feel free to send your suggestions to contact@highergov.com
{% endhint %}

### Overview

HigherGov’s MCP allows subscribers to access HigherGov data directly inside supported AI assistants and LLM tools for search, research, and analysis.

MCP stands for **Model Context Protocol**. It is a standard way for AI assistants to securely connect to external tools and data sources. With the HigherGov MCP, an LLM can search HigherGov opportunities, awards, contractors, documents, NSNs, and related records using natural language.

The HigherGov MCP is powered by the [**HigherGov API**](highergov-api.md). Each subscription includes 10,000 records per month, which will be sufficient for the vast majority of use cases.

**HigherGov MCP endpoint:**

```
https://www.highergov.com/mcp/
```

***

## Creating a HigherGov API Key

API keys can be managed when signed in to HigherGov from the [API Management](https://www.highergov.com/api-management/) screen (requires Admin access).

Or navigate manually:

1. Sign in to HigherGov.
2. Select the **gear icon** in the upper-right corner.
3. Select **API**.
4. Click **Generate Key**.
5. Copy and securely save the key.

> Important: the full API key is only displayed once when it is created. Make sure to copy and securely store it before leaving the page.

***

## Authentication

The preferred authentication method is to send your HigherGov API key as a bearer token:

```
Authorization: Bearer YOUR_HIGHERGOV_API_KEY
```

Some MCP clients support custom headers directly. If your client does, use the bearer-token method above.

Some LLMs may also use the term Access Token or API Key.

***

## Connecting to HigherGov MCP

> MCP support is changing quickly across LLM providers. The exact setup steps may change, so always check your LLM provider’s latest MCP or connector documentation.

At a high level, setup usually works like this:

1. Open your AI assistant’s **MCP**, **Connector**, **App**, or **Tools** settings.
2. Add a new remote MCP server.
3. Enter the HigherGov MCP endpoint:

```
https://www.highergov.com/mcp/
```

4\. Set authentication using your HigherGov API key:

```
Authorization: Bearer YOUR_HIGHERGOV_API_KEY
```

5. Save or scan tools.
6. Start asking questions naturally.

Example prompts:

```
Find active federal contract opportunities for cybersecurity services.
```

```
Find potential teaming partners for cybersecurity work.
```

```
Search recent federal contract awards for zero trust.
```

***

### Claude

Claude supports custom connectors using remote MCP servers. In Claude, custom connectors can be added from **Settings → Connectors**, where users can enter the remote MCP server URL. Team and Enterprise administrators may also manage custom connectors through admin connector settings. ([claude.com](https://claude.com/docs/connectors/custom/remote-mcp))

Use:

```
https://www.highergov.com/mcp/
```

Authentication:

```
Authorization: Bearer YOUR_HIGHERGOV_API_KEY
```

General steps:

1. Open Claude.
2. Go to **Settings → Connectors**.
3. Select **Add custom connector**.
4. Enter the HigherGov MCP URL.
5. Add authentication if supported by your Claude plan/client.
6. Enable the connector in a chat.

Claude allows connectors to be enabled or disabled per conversation from the chat interface. ([claude.com](https://claude.com/docs/connectors/custom/remote-mcp))

***

### OpenAI / ChatGPT

OpenAI supports remote MCP servers through ChatGPT apps/connectors and through the OpenAI API. Remote MCP servers are servers available over the public internet that implement MCP. ([platform.openai.com](https://platform.openai.com/docs/guides/tools-remote-mcp))

For ChatGPT, custom MCP apps are managed through **Developer Mode** for eligible workspaces. OpenAI’s current help documentation describes availability for ChatGPT Business, Enterprise, and Edu workspaces, with admins or authorized developers able to create and test custom MCP apps. ([help.openai.com](https://help.openai.com/en/articles/12584461-developer-mode-apps-and-full-mcp-connectors-in-chatgpt-beta))

Use:

```
https://www.highergov.com/mcp/
```

Authentication:

```
Authorization: Bearer YOUR_HIGHERGOV_API_KEY
```

General steps:

1. Confirm your workspace supports custom MCP apps/connectors.
2. Enable Developer Mode if required.
3. Create a new custom app/connector.
4. Provide the HigherGov MCP endpoint.
5. Configure authentication using your HigherGov API key.
6. Scan tools.
7. Test the app in a new chat.
8. Publish or enable it for your workspace if desired.

For API usage, configure HigherGov as a remote MCP server using the HigherGov MCP endpoint and bearer authorization. OpenAI’s Responses API supports remote MCP servers using a `server_url`, and authorization may be supplied depending on the server. ([platform.openai.com](https://platform.openai.com/docs/guides/tools-remote-mcp))

Example shape:

```json
{
  "tools": [
    {
      "type": "mcp",
      "server_label": "highergov",
      "server_url": "https://www.highergov.com/mcp/",
      "authorization": "Bearer YOUR_HIGHERGOV_API_KEY"
    }
  ]
}
```

***

### Gemini

Gemini CLI supports MCP servers through `settings.json` or the `gemini mcp add` command. Gemini CLI supports stdio, SSE, and Streamable HTTP transports; for HigherGov, use the HTTP transport. ([github.com](https://github.com/google-gemini/gemini-cli/blob/main/docs/tools/mcp-server.md))

Use:

```
https://www.highergov.com/mcp/
```

#### Add with CLI command

```bash
gemini mcp add \
  --transport http \
  --header "Authorization: Bearer YOUR_HIGHERGOV_API_KEY" \
  highergov \
  https://www.highergov.com/mcp/
```

Verify the connection:

```bash
gemini mcp list
```

Gemini CLI also provides an `/mcp` command inside the CLI to show configured servers, connection status, available tools, and discovery status. ([github.com](https://github.com/google-gemini/gemini-cli/blob/main/docs/tools/mcp-server.md))

#### Or configure `settings.json`

```json
{
  "mcpServers": {
    "highergov": {
      "httpUrl": "https://www.highergov.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_HIGHERGOV_API_KEY"
      },
      "timeout": 30000
    }
  }
}
```

***

## MCP Tools and Common Use Cases

The HigherGov MCP exposes tools that let your AI assistant search HigherGov data and retrieve supporting records.

You do not need to know the tool names to use the MCP. In most cases, you can ask questions naturally and the LLM will choose the appropriate tool.

***

### Opportunity Search

**Tool:** `search_opportunities`

Use this to find and identify relevant opportunities, including:

* Federal contract opportunities
* State and local contact opportunities
* Federal grant opportunities
* DIBBS opportunities
* Federal forecasts
* SLED forecasts

Common use cases:

* Find active opportunities that match your products or services.
* Identify opportunities by market, NAICS, customer, or topic.
* Search federal and state/local opportunities together.
* Forecast upcoming opportunities using federal forecasts and SLED forecasts.
* Use a saved HigherGov search to monitor opportunities.
* Find relevant opportunities and then retrieve the solicitation documents.

Example prompts:

```
Find active federal contract opportunities for custodial services.
```

```
Search active federal and state/local opportunities for physical security or surveillance.
```

```
Find active federal opportunities under NAICS 541512 related to zero trust.
```

```
Search federal forecasts related to IT modernization.
```

```
Find upcoming SLED forecast opportunities for construction services.
```

***

### Opportunity Documents

**Tool:** `get_opportunity_documents`

Use this to retrieve documents and extracted text for an opportunity.

Common use cases:

* Summarize solicitation documents.
* Extract requirements from an RFP or RFQ.
* Review attachments for due dates, submission instructions, evaluation criteria, or scope.
* Pull document text for an opportunity found through opportunity search.

Example prompts:

```
Find active opportunities for janitorial services and summarize the documents for the top result.
```

```
Get the documents for this opportunity and summarize the scope of work.
```

```
Review the solicitation documents and identify submission requirements.
```

***

### Awards Research

**Tools:**

* `search_federal_contracts`
* `search_federal_idvs`
* `search_federal_grants`
* `search_federal_subcontracts`
* `search_federal_subgrants`

Use these tools to research historical awards, spending, recipients, contract vehicles, and subcontract/subgrant activity.

Common use cases:

* Research prior federal contract awards in a market.
* Find likely incumbents or competitors.
* Search IDVs, GWACs, BPAs, IDIQs, and other federal contract vehicles.
* Research federal grant awards by topic, recipient, or program.
* Analyze subcontract or subgrant activity involving a company.
* Understand historical buying patterns before pursuing an opportunity.

Example prompts:

```
Search federal contract awards for zero trust under NAICS 541512.
```

```
Find federal contracts awarded to this UEI.
```

```
Search IDVs involving cloud migration.
```

```
Search federal grant awards related to workforce development.
```

```
Find subcontract awards involving this company.
```

***

### Contractor and Partner Research

**Tools:**

* `search_awardees`
* `get_awardee_detail`

Use these tools to find and research companies, including potential teaming partners.

Common use cases:

* Find potential partners for an opportunity or market.
* Identify contractors by capability, NAICS, UEI, or CAGE.
* Find a company’s UEI or CAGE code.
* Review contractor profiles, registrations, NAICS codes, PSC codes, business types, and capabilities.
* Research agency experience, vehicles, and subcontract relationships.
* Compare potential partners or competitors.

Example prompts:

```
Find potential teaming partners for cybersecurity work.
```

```
Find small business partners with experience in physical security.
```

```
Find the UEI and CAGE code for Leidos.
```

```
Get the detailed profile for this UEI.
```

```
Find companies with primary NAICS 541611 and federal health agency experience.
```

***

### NSN / DIBBS Research

**Tool:** `search_nsn`

Use this to search NATO Stock Number and DIBBS-related data.

Common use cases:

* Look up an NSN.
* Find suppliers by CAGE code.
* Search by part number.
* Review recent supplier and award history for an NSN.

Example prompts:

```
Look up NSN 6505013306269.
```

```
Find NSNs associated with CAGE 95403.
```

```
Search for this part number and summarize suppliers.
```

<br>
