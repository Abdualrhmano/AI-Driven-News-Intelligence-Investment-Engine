

# AI-Driven News Intelligence & Investment Engine

**AI-Driven News Intelligence & Investment Engine** is an advanced automated system that leverages **AI agents** to analyze real-time news from **NewsAPI**, generate **intelligence analysis** and **investment signals**, and deliver actionable insights via **Telegram** notifications while logging data to **Google Sheets**.[1]

[
[
[
[

## 1. Overview
This **n8n workflow** runs every **4 hours** to:
- Fetch Arabic news on **Business OR War OR Sports OR Technology** using **NewsAPI** (API key: `1dd6d387cc20466ab1a8f94dd965d1f4`).
- Process articles with **Google Gemini** AI agent for:
  - **Intelligence Analysis**
  - **Investment Signals** (with confidence score 1-10)
- Append results to **Google Sheets** (Spreadsheet ID: `1FzRueFUeS5Y7TK7MBLEGz2BPmCWFFqTpJmEa77lC1I`, Sheet: `gid=0`).
- Send notifications to **Telegram** chat (`chatId: 7522858254`).

**Key Outputs**: Date, Title, Intelligence Analysis, Investment Signal, Source URL.[1]

## 2. System Architecture

```mermaid
graph LR
    A[Schedule Trigger<br/>Every 4 Hours] --> B[HTTP Request<br/>NewsAPI<br/>q=Business|War|Sports|Technology<br/>language=ar]
    B --> C[AI Agent<br/>Google Gemini<br/>Prompt: Title + Description<br/>Output: Analysis + Signal + Confidence]
    C --> D[Google Sheets<br/>Append Row<br/>Columns: Date|Title|Analysis|Signal|URL]
    D --> E[Telegram<br/>Send Message<br/>chatId: 7522858254]
    
    F[Google Gemini<br/>Chat Model] -.-> C
    
    style A fill:#e8f5e8
    style C fill:#f3e5f5
    style D fill:#fff3e0
    style E fill:#e3f2fd
```
**Workflow powered by n8n nodes: `n8n-nodes-base.scheduleTrigger`, `n8n-nodes-base.httpRequest`, `n8n-nodes-langchain.agent`, `n8n-nodes-base.googleSheets`, `n8n-nodes-base.telegram`.** [1]

## 3. Key Features
1. **Real-Time News Monitoring**: Arabic-focused news aggregation every 4 hours.
2. **AI-Powered Analysis**: **Gemini** generates intelligence summaries and investment recommendations.
3. **Structured Data Logging**: Automatic export to Google Sheets with columns: **Date**, **Title**, **Intelligence Analysis**, **Investment Signal**, **Source URL**.
4. **Instant Notifications**: Telegram alerts for high-confidence signals.
5. **Scalable Workflow**: n8n-based, with retry-on-fail and persistent execution.

## 4. Quick Start
### Prerequisites
- **n8n** instance (self-hosted or cloud).
- **Google Sheets OAuth2** credentials.
- **Telegram API** credentials.
- **Google Gemini/PaLM API** key (`HGZ932ZRKyaWA57j`).
- **NewsAPI** key (`1dd6d387cc20466ab1a8f94dd965d1f4`).

### Setup
1. Import `My-workflow.json` into n8n.
2. Configure credentials:
   - Google Sheets: `googleSheetsOAuth2Api`.
   - Telegram: `telegramApi`.
   - Gemini: `googlePalmApi`.
3. Update **Spreadsheet ID** and **chatId** if needed.
4. Activate workflow.

```bash
# Example n8n import (CLI)
n8n import:workflow --input My-workflow.json
n8n start
```

## 5. Workflow Nodes Details[1]
| Node | Type | Position | Key Parameters |
|------|------|----------|----------------|
| **Schedule Trigger** | `n8n-nodes-base.scheduleTrigger` | (-464, -576) | Interval: 4 hours |
| **HTTP Request** | `n8n-nodes-base.httpRequest` | (-336, -576) | NewsAPI everything endpoint |
| **AI Agent** | `n8n-nodes-langchain.agent` | (-208, -576) | Prompt: Title + Desc → Analysis + Signal |
| **Google Sheets** | `n8n-nodes-base.googleSheets` | (64, -576) | Append to Sheet ID `1FzRueFUeS5Y7TK7MBLEGz2BPmCWFFqTpJmEa77lC1I` |
| **Telegram** | `n8n-nodes-base.telegram` | (16, -272) | chatId: `7522858254` |
| **Gemini Model** | `n8n-nodes-langchain.lmChatGoogleGemini` | (-208, -384) | Credentials: `Google Gemini/PaLM Api account 2` |

## 6. Data Schema (Google Sheets Columns)[1]
- **Date**: `json.publishedAt`
- **Title**: `json.title`
- **Intelligence Analysis**: AI Agent output
- **Investment Signal**: AI Agent output (w/ confidence)
- **Source URL**: `json.url`

## 7. Customization
- **Adjust Schedule**: Change `hoursInterval` in Schedule Trigger.
- **News Queries**: Modify `q=Business OR War OR Sports OR Technology` or add keywords.
- **AI Prompt**: Edit in AI Agent node for custom analysis.
- **Notifications**: Update Telegram chatId or add channels.

## 8. Security & Best Practices
- Rotate API keys regularly.
- Use environment variables for credentials.
- Monitor n8n execution logs.
- Set up workflow error handling.

## 9. License
MIT License - Free to use, modify, and distribute.

## Key Takeaways
- Fully automated **news-to-investment** pipeline using **n8n + Gemini AI**.
- Logs to **Google Sheets** for easy analysis/export.
- **Telegram** for real-time alerts on investment opportunities.
- Ready-to-deploy with your provided `My-workflow.json`.[1]

Hope this helps! Let me know if you need adjustments, like adding screenshots, more details, or deployment instructions.

الاقتباسات:
[1] My-workflow.json https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/239183808/2e29f975-999f-497c-8b6f-a07997c6d015/My-workflow.json?AWSAccessKeyId=ASIA2F3EMEYEUFPUTK7X&Signature=aC%2Fx3YrBoLZXDQngm%2FR%2BBJWasyg%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEFIaCXVzLWVhc3QtMSJGMEQCIEarlMdVSKWjnr59vuAXoKoZKmHZYCB%2BOhk97dUN06%2FKAiAsyQbRCocxW0Rs628qJ3rESDXAb07xhB7Ii9tyX4XcuSrzBAgbEAEaDDY5OTc1MzMwOTcwNSIMcKwNOVpEE0qGajIpKtAE%2BaNM8iTwSUeiWpXVBNM9zho4Uz8gb1h9vOIJebndOffszN5zYpmiDpGtOb1gcshJ%2FByQlv0bfKiOzl%2BFMZZi0ZuWwweRGTXmiYW3yzN2Y7biFsp9ENDCrWkBwD9Jivwz%2FnkSwTJ4bdKTPDH2Of4r2o4GdsymPTGM7RnPoFvGx%2Fn7wiVVsTbo6qXJ38nmLCe4m%2FzI9nXgRNPekKi0ueZ5z%2FSTivTMs1BiD0BKKEpEW0fccA94x5gOi%2B9BWHJ2k99QpV2VDgknMe98oMxRlWN78FVfTWJTf%2Bc%2F%2FjQ%2BEcNDaS5ilHvIlqjWVnj%2BiQpvm%2B84l0m7KKwuNcCe4WK9InoJj2aaYvSagUtKOR1eIrI5F8ooGiKpCUDsWRNQKfXW0j%2BFalTh4lvZJs7%2FedJ3IkJsbhoOHflN6UJCT%2Fw8uO%2B4kk3C4xeW2R40XI3rKEMwD0I4Au8a%2B66SQ0s799es5AruLoWRBqff7dtfGNcYH9nZZ%2FFo0DF%2BKcfDKdEjvqAm1SeZGeLZZPZlh3ZxqKPH6ZIWdcCuF22jFZjVWAXkT4s1ll0M4%2Bn7oyBMlsCFnvLXI7aol4pE7RAbbaj5Ke%2BoV2dtG4Vj8Gn7sTq3lp3uUQNIzm62LOg%2FQ8ilXJjzMRZy8mcshL8zT4BCYsCciyxN9g0uPJ%2FQZIVeYYim7oTkK7l3ilzax2g4YVwUmGkHNGVDFBfpNdIY7IwgAHDGhDzyvEh1sjXaFle2wZeG8%2FqOVvdG6e%2F9MtL7KjMBuSZqdE%2FBnzqsJtM9v6IIQ8fAddaoqKkiaDDIzd%2FOBjqZAT6IKwW0SeqU5XcQ0LP90%2FvyXbTxzfKgaEy%2BA61oB9BHrts%2FR%2FRzGxp3hNkTi3pa2Jymr8yqQB1A6Qa2%2FC0v5sQEMdSllwUCvm5f%2FC9Wl67epww%2BvchMK8Djg0oV2gsZGiqC0nNttnX3UU1No1xb3BBa9qus4k5ba7JxZfTyI8riiSLCfOoHNUUBxOaejJM6eQY1fcIGDuiisg%3D%3D&Expires=1775759319
