# 🎯 API AI AUTOMATION

Transform your JIRA tickets into automated API tests in seconds! This tool reads JIRA tickets and uses AI to generate comprehensive Postman test collections.

## ✨ What It Does

1. **Connects to JIRA** - Fetches your ticket details automatically
2. **AI Test Generation** - Creates intelligent API tests based on requirements
3. **Postman Ready** - Outputs industry-standard Postman collections
4. **One-Click Testing** - Run tests immediately with Newman

## 🚀 Quick Start

### Prerequisites

- Python 3.7+
- JIRA account with API access
- OpenAI API key

### Installation

1. **Clone or download** this script
2. **Install dependencies:**
   ```bash
   pip install requests python-dotenv langchain-openai
   ```

3. **Create `.env` file** in the same directory:
   ```env
   JIRA_URL=https://yourcompany.atlassian.net
   JIRA_EMAIL=your.email@company.com
   JIRA_API_TOKEN=your_jira_api_token
   JIRA_ISSUE_KEY=PROJECT-123
   OPENAI_API_KEY=sk-your_openai_key
   ```

4. **Run the script:**
   ```bash
   python qa_automation.py
   ```

That's it! Your tests will be saved to `tests/my_tests.json`

## 🔧 Setup Guide

### Getting JIRA API Token

1. Go to [Atlassian Account Settings](https://id.atlassian.com/manage-profile/security/api-tokens)
2. Click "Create API token"
3. Copy the token to your `.env` file

### Getting OpenAI API Key

1. Visit [OpenAI API Keys](https://platform.openai.com/api-keys)
2. Create new secret key
3. Add to your `.env` file

### Finding JIRA Issue Key

- Look at your JIRA ticket URL: `https://company.atlassian.net/browse/PROJ-123`
- The issue key is: `PROJ-123`

## 📋 Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `JIRA_URL` | Your JIRA instance URL | `https://mycompany.atlassian.net` |
| `JIRA_EMAIL` | Your JIRA login email | `john.doe@company.com` |
| `JIRA_API_TOKEN` | JIRA API authentication token | `ATATT3xFfGF0...` |
| `JIRA_ISSUE_KEY` | Specific ticket to process | `PROJ-123` |
| `OPENAI_API_KEY` | OpenAI API key for AI generation | `sk-proj-...` |

## 🎮 Running Tests

After generating tests, you can run them with:

```bash
# Install Newman (Postman CLI)
npm install -g newman

# Run your generated tests
npx newman run tests/my_tests.json
```

## 📁 Output Structure

```
your-project/
├── qa_automation.py
├── .env
└── tests/
    └── my_tests.json  # Generated Postman collection
```

## 🔍 How It Works

1. **Fetch Ticket**: Connects to JIRA API using your credentials
2. **Extract Requirements**: Pulls ticket summary and description
3. **AI Processing**: Sends requirements to OpenAI for test generation
4. **Format Output**: Converts AI response to valid Postman collection
5. **Save Results**: Writes tests to JSON file ready for execution
---
