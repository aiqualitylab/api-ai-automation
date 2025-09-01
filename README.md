"""
🎯 SUPER SIMPLE QA AUTOMATION
Just run this file and get automatic tests from your JIRA ticket!

What happens:
1. Reads your JIRA ticket
2. AI creates tests
3. Done!
"""

import os
import json
import requests
from dotenv import load_dotenv
from langchain_openai import ChatOpenAI

# Load your settings
load_dotenv()

# Your settings (edit the .env file, not this code!)
jira_url = os.getenv("JIRA_URL")
jira_email = os.getenv("JIRA_EMAIL") 
jira_token = os.getenv("JIRA_API_TOKEN")
ticket_id = os.getenv("JIRA_ISSUE_KEY")
openai_key = os.getenv("OPENAI_API_KEY")

def main():
    print("🚀 Starting...")
    
    # Check if settings exist
    if not all([jira_url, jira_email, jira_token, ticket_id, openai_key]):
        print("❌ Missing settings! Check your .env file")
        print("\nYou need:")
        print("JIRA_URL=https://yourcompany.atlassian.net")
        print("JIRA_EMAIL=your.email@company.com")
        print("JIRA_API_TOKEN=your_token")
        print("JIRA_ISSUE_KEY=PROJECT-123") 
        print("OPENAI_API_KEY=sk-your_key")
        return
    
    try:
        # Step 1: Get JIRA ticket
        print(f"📋 Getting ticket {ticket_id}...")
        url = f"{jira_url.rstrip('/')}/rest/api/3/issue/{ticket_id}"
        response = requests.get(url, auth=(jira_email, jira_token))
        ticket = response.json()
        requirement = ticket["fields"]["summary"]
        print(f"✅ Got: {requirement}")
        
        # Step 2: Ask AI to make tests
        print("🤖 AI is creating tests...")
        ai = ChatOpenAI(temperature=0)
        prompt = f"Create API tests for: {requirement}\n\nMake Postman collection JSON with test scripts. JSON only."
        result = ai.invoke([{"role": "user", "content": prompt}])
        
        # Clean up AI response
        tests_json = result.content.strip()
        if "```" in tests_json:
            tests_json = tests_json.replace("```json", "").replace("```", "").strip()
        
        # Step 3: Save tests
        tests = json.loads(tests_json)
        if isinstance(tests, dict) and "item" not in tests:
            tests = [tests]
        elif isinstance(tests, dict) and "item" in tests:
            tests = tests["item"]
            
        # Make Postman collection
        collection = {
            "info": {"name": "AI Tests", "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"},
            "item": tests
        }
        
        # Save file
        os.makedirs("tests", exist_ok=True)
        filename = "tests/my_tests.json"
        with open(filename, "w") as f:
            json.dump(collection, f, indent=2)
        
        print(f"✅ Saved tests to: {filename}")
        print(f"🎉 Done! Run tests with: npx newman run {filename}")
        
    except Exception as e:
        print(f"❌ Error: {e}")
        print("💡 Check your settings and try again")

if __name__ == "__main__":
    main()
