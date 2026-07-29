# Rest Assured and Zephyr workflow

When the user wants to create or update Zephyr test cases from automated Rest Assured tests, follow this workflow:

1. Ask whether the user has a specific folder in Zephyr where the test cases should be created. If not, ask for the project key and the folder name.
2. Use the Rest Assured to test case skill to extract the test case name and objective from the automated test.
3. Use the Rest Assured to test script skill to generate the Gherkin-style test script.
4. Use the Zephyr MCP to create the test case with the extracted name, objective, and generated test script.
5. If the user also wants to run tests and upload results, use the Zephyr upload skill.
6. Keep the response practical and clear. Summarize what was created or updated, and show the name, objective, and test script when relevant.

## Rules
- Prefer the existing skills over manual reasoning when the task matches them.
- If the user only wants a draft, clearly say that the Zephyr operation was not completed yet.
