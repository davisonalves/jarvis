---
name: restassured-and-zephyr-full-worflow
description: orchestrates the end-to-end workflow of converting automated Rest Assured tests into Zephyr test cases, generating test scripts, and uploading JUnit results when requested.
---

# How to use
When the user asks to create, update, or publish test cases in Zephyr based on automated tests, follow this workflow using the existing skills and the Zephyr MCP:

1. Ask if the user have a specific folder in Zephyr where the test cases should be created. If not, ask for the project key and the folder name to create.

2. Use the Rest Assured to test case skill to extract the test case name and objective from the automated test.

3. Use the Rest Assured to test script skill to generate the Gherkin-style test script from the automated test.

4. Use the Zephyr MCP to create the test case in Zephyr with the extracted name, objective, and generated test script.

5. Ask If the user also wants to run tests and upload results, if so, use the Zephyr upload skill

6. Keep the response practical.
   - Summarize what was created or updated.
   - Show the generated name, objective, and test script when relevant.
   - If something is missing, ask for it before continuing.

# Rules
- Prefer the existing skills over manual reasoning when the task matches them.
- If the user only wants a draft, provide it clearly without pretending that the Zephyr operation was completed.
