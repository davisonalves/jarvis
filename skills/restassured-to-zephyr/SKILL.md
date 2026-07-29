---
name: restassured-to-zephyr
description: executes bash script to upload JUnit XML results to Zephyr Scale using Maven reports and environment variables
---

# How to use
When the user wants to upload JUnit test results to Zephyr Scale, follow this flow:

1. Ask for the Jira/Zephyr project key.
2. Ask for the project directory where the Maven project is located.
3. Ask which report folder should be used:
   - surefire-reports
   - failsafe-reports
4. Ask which Maven command should be used:
   - mvn test
   - mvn integration-test
5. Do not ask for the token directly.
   - Use the environment variable ZEPHYR_TOKEN if available.
   - If not available, tell the user to export one before running.
6. Adjust a bash script that:
   - runs the selected Maven command
   - finds XML files in `target/<report-folder>/TEST*.xml`
   - zips them
   - uploads the zip to Zephyr Scale using the project key
7. Run the bash script and provide feedback to the user about the upload status.

# Bash script example
```
#!/bin/bash

if [ -z $1 ] || [ -z $2 ]
then
  echo "Some or all of the parameters are empty";
  echo "Usage: $0 projectKey token"
  echo -e "\t- projectKey jira project key for tests "
  echo -e "\t- token Public REST API token for Zephyr"
  exit 1
fi

PROJECT_KEY=$1 # provide your project key
TOKEN=$2 # provide your Public REST API token

# Please provide the correct URL according to the product you use
URL="https://api.zephyrscale.smartbear.com/v2/automations/executions/junit?projectKey=${PROJECT_KEY}&autoCreateTestCases=false"
# URL="https://prod-api.zephyr4jiracloud.com/v2/automations/executions/junit?projectKey=${PROJECT_KEY}&autoCreateTestCases=false"

mvn test

zip -D ./target/junit_tests.zip ./target/surefire-reports/TEST*.xml

curl -o -X POST -F "file=@target/junit_tests.zip" -H "Authorization: Bearer ${TOKEN}" $URL
```
