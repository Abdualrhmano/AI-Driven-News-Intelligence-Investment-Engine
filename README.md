

# AI-Driven News Intelligence & Investment Engine

**AI-Driven News Intelligence & Investment Engine** is an advanced automated system that leverages **AI agents** to analyze real-time news from **NewsAPI**, generate **intelligence analysis** and **investment signals**, and deliver actionable insights via **Telegram** notifications while logging data to **Google Sheets**.[1]

[
[
[
[

## 1. Overview
This **n8n workflow** runs every **4 hours** to:
- Fetch Arabic news on **Business OR War OR Sports OR Technology** using **NewsAPI** (API key: ``).
- Process articles with **Google Gemini** AI agent for:
  - **Intelligence Analysis**
  - **Investment Signals** (with confidence score 1-10)
- Append results to **Google Sheets** (Spreadsheet ID: ``, Sheet: `gid=0`).
- Send notifications to **Telegram** chat (`chatId: `).

**Key Outputs**: Date, Title, Intelligence Analysis, Investment Signal, Source URL.[1]

## 2. System Architecture

```mermaid
graph LR
    A[Schedule Trigger<br/>Every 4 Hours] --> B[HTTP Request<br/>NewsAPI<br/>q=Business|War|Sports|Technology<br/>language=ar]
    B --> C[AI Agent<br/>Google Gemini<br/>Prompt: Title + Description<br/>Output: Analysis + Signal + Confidence]
    C --> D[Google Sheets<br/>Append Row<br/>Columns: Date|Title|Analysis|Signal|URL]
    D --> E[Telegram<br/>Send Message<br/>chatId: ]
    
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
- **Google Gemini/PaLM API** key (``).
- **NewsAPI** key (``).

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

