---
name: restassured-to-test-case
description: reads an automated test using Rest Assured and converts it into a functional test case.
---

# How to use
Read the examples of automated tests using Rest Assured and the corresponding id, name and objective output examples for each.

Do not forget to follow the established rules.

## Example of automated test using Rest Assured
```Java
@Test
@DisplayName("View list of active stores")
void TEST_KEY1() {
  RestAssured
    .given()
      .accept("*/*")
    .when()
      .get(requestURI)
    .then()
      .statusCode(200)
      .body("branches", hasSize(2))
      .body("branches.code", containsInAnyOrder(1,2));
}
```

## Example of test name output
View list of active stores

## Example of test id
TEST_KEY1

## Example of test objective output
Verify if the API can retrieve information on active stores.

## Rules
- The test name must always match the content of the @DisplayName annotation.
- The test ID must always be the method name.
- The test objective must always begin with an action verb, such as "Verify," "Ensure," or "Confirm."
- Do not include technical details in the test objective.
- Do not specify the endpoint or HTTP method in the test objective.
- For success scenarios, do not include phrases like "when sending valid values" in the objective; this is redundant, as correct information is already expected in a successful outcome.
- For error scenarios, do not include details like the expected status code or error message in the objective; instead, use phrasing such as "Verify if the API returns an error..."
- Do not write overly long test objectives. The objective should be concise and to the point.