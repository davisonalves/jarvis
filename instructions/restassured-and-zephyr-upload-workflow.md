# Zephyr upload workflow
When the user asks to run tests and update the results in Zephyr Scale, prioritize the skill at [skills/upload-junit-test-results-to-zephyr/SKILL.md](skills/upload-junit-test-results-to-zephyr/SKILL.md) before giving manual terminal steps.

Follow this order:
1. Check whether the request is about uploading JUnit test results to Zephyr Scale.
2. If yes, use the Zephyr upload skill automatically.
3. Ask for the required inputs the skill expects:
   - Jira/Zephyr project key
   - project directory
   - report folder: surefire-reports or failsafe-reports
   - Maven command: mvn test or mvn integration-test
4. Do not ask for the token directly; use the environment variables ZEPHYR_TOKEN or ZEPHYR_SCALE_TOKEN.
5. Prefer generating or adapting the script described by the skill instead of only listing manual commands.
6. If the skill is not sufficient for the request, use it as the primary guide and then complement it with additional steps.
