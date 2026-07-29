---
name: restassured-to-test-script
description: reads an automated test using Rest Assured and converts it into a gherkin test script.
---

# How to use
Read the examples of automated tests using Rest Assured and the corresponding gherkin test script output examples for each.

Do not forget to follow the established rules.

## Example of automated test using Rest Assured (GET)
```Java
@Test
@DisplayName("View the client details")
void TEST_KEY1() {
  RestAssured
    .given()
      .accept("*/*")
      .queryParam("document", "ABC123")
    .when()
      .get(requestURI)
    .then()
      .statusCode(200)
      .body("client", equalTo("Test Client"))
      .body("document", equalTo("ABC123"));
}
```

## Example of gherkin test script (GET)
```Gherkin
Given the endpoint "/v1/client" is available
And the query parameters are:
  | parameter | value       | 
  | document  | ABC123 |
When a "GET" request is made
Then the response status code must be "200 OK"
And the response body must contain:
 | field    | value       |
 | client   | Test Client |
 | document | ABC123      |
```

## Example of automated test using Rest Assured (POST)
```Java
@Test
@DisplayName("View list of active stores")
void TEST_KEY2() {
  RestAssured
    .given()
      .accept("*/*")
      .contentType("application/json")
      .body(fromResources("order-cpf.json"))
    .when()
      .post(requestUri)
    .then()
      .statusCode(201)
      .body("result.description", equalTo("Order saved successfully."));
}
```

## Example of gherkin test script (POST)
```Gherkin
Given the endpoint "/v1/orders" is available
And the body corresponds to the file "order-cpf.json"
When a "POST" request is made
Then the response status code must be "201 Created"
And the response body must contain:
| field              | value                     |
| result.description | Order saved successfully. |
```

## Example of automated test using Rest Assured (PARAMETERIZED)
```Java
@ParameterizedTest
@CsvSource({"cpf", "cnpj", "alphanumericcnpj"})
@DisplayName("View list of active stores")
void TEST_KEY3(String documentType) {
  RestAssured
    .given()
      .accept("*/*")
      .contentType("application/json")
      .body(fromResources("order-%s.json".formatted(documentType)))
    .when()
      .post(requestUri)
    .then()
      .statusCode(201)
      .body("result.description", equalTo("Order saved successfully."));
}
```

## Example of gherkin test script (PARAMETERIZED)
```Gherkin
# Example 1: documentType = cpf
Given the endpoint "/v1/orders" is available
And the body corresponds to the file "order-cpf.json"
When a "POST" request is made
Then the response status code must be "201 Created"
And the response body must contain:
| field              | value                     |
| result.description | Order saved successfully. |

# Example 2: documentType = cnpj
Given the endpoint "/v1/orders" is available
And the body corresponds to the file "order-cnpj.json"
When a "POST" request is made
Then the response status code must be "201 Created"
And the response body must contain:
| field              | value                     |
| result.description | Order saved successfully. |

# Example 3: documentType = alphanumericcnpj
Given the endpoint "/v1/orders" is available
And the body corresponds to the file "order-alphanumericcnpj.json"
When a "POST" request is made
Then the response status code must be "201 Created"
And the response body must contain:
| field              | value                     |
| result.description | Order saved successfully. |
```

## Example of automated test using Rest Assured (MULTIPLE REQUESTS)
```Java
@Test
@DisplayName("View list of orders filtering by the branch code")
void TEST_KEY4() {
  // Branch = 1000
  RestAssured
    .given()
      .accept("*/*")
      .queryParam("document", "31828625531")
      .queryParam("status", 1)
      .queryParam("branch", 1000)
    .when()
      .get(requestUri)
    .then()
      .statusCode(200)
      .body("content[0].client", equalTo("TEST CLIENT"))
      .body("content[0].document", equalTo("31828625531"))
      .body("content[0].legacyOrderCode", equalTo(211000005703942L)) 
      .body("content[0].branch.toString()", equalTo("1000"));

// Branch = 1009
  RestAssured
    .given()
      .accept("*/*")
      .queryParam("document", "31828625531")
      .queryParam("status", 1)
      .queryParam("branch", 1009)
    .when()
      .get(requestUri)
    .then()
      .statusCode(200)
      .body("content[0].client", equalTo("TEST CLIENT"))
      .body("content[0].document", equalTo("31828625531"))
      .body("content[0].legacyOrderCode", equalTo(211000005703977L))
      .body("content[0].branch.toString()", equalTo("1009"));
}
```

## Example of gherkin test script (MULTIPLE REQUESTS)
```Gherkin
# Request 1: Branch = 1000
Given the endpoint "/v1/orders" is available
And the query parameters are:
| parameter | value       | 
| document  | 31828625531 | 
| status    | 1           | 
| branch    | 1000        |
When a "GET" request is made
Then the response status code must be "200 OK"
And the response body must contain:
| field                      | value           | 
| content[0].client          | TEST CLIENT     | 
| content[0].document        | 31828625531     | 
| content[0].legacyOrderCode | 211000005703942 |
| content[0].branch          | 1000            |

# Request 2: Branch = 1009
Given the endpoint "/v1/orders" is available
And the query parameters are:
| parameter | value       | 
| document  | 31828625531 | 
| status    | 1           | 
| branch    | 1009        |
When a "GET" request is made
Then the response status code must be "200 OK"
And the response body must contain:
| field                      | value           | 
| content[0].client          | TEST CLIENT     | 
| content[0].document        | 31828625531     | 
| content[0].legacyOrderCode | 211000005703977 |
| content[0].branch          | 1009            |
```

## Rules
- The use of Features, Scenarios, Scenario Outlines, or Backgrounds is not permitted in the Gherkin test script.
- The use of tables is encouraged to organize query parameters, headers, and body validations.
- 'Accept' and 'Content-Type' headers must not be included in the Gherkin test script.
- Pay attention to table formatting; values ​​must be correctly aligned to ensure test readability. Use the fields with the most characters as a guide.