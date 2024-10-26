#Rule Engine Application



The Rule Engine Application is a Spring Boot-based system that dynamically evaluates user eligibility based on customizable rules using an Abstract Syntax Tree (AST). Users can create, combine, and evaluate rules with attributes like age, department, income, and spending. The application supports dynamic rule management through APIs and validates inputs to ensure accuracy.

Table of Contents
Features
Project Structure
Technologies Used
Setup Instructions
Endpoints and Postman Commands
Creating a Rule
Retrieving All Rules
Evaluating a Rule
Combining Rules
Exception Handling
Future Improvements
Features
Dynamic Rule Management: Create, evaluate, and combine rules using an AST structure.
Validation and Error Handling: Provides structured error messages for invalid inputs.
Three-tier Architecture: Clean separation between the controller, service, and repository layers.
Extensible Data Model: Stores rules and node structures, enabling dynamic modifications.
Project Structure
The main components of the project include:

controller/: Manages HTTP requests and routing.
RuleController: Handles endpoints for rule operations.
NodeController and AttributeController: (if required) manage nodes and attributes.
service/: Contains business logic for rule evaluation and management.
RuleService, NodeService, AttributeService: Service interfaces.
RuleServiceImpl, NodeServiceImpl, AttributeServiceImpl: Service implementations.
model/: Represents core entities and data structures.
Rule, Node, AST: Represent rules, nodes, and AST structure.
ExpressionEvaluator: Evaluates expressions using the AST.
repository/: Manages data persistence.
RuleRepository, NodeRepository, AttributeRepository: Repository interfaces.
dto/: Data Transfer Objects.
RuleDTO, EvaluateRuleRequest: Simplifies data communication.
Technologies Used
Spring Boot: Backend framework.
JPA: Data persistence.
PostgreSQL: Database.
Postman: API testing.
Setup Instructions
Clone the repository:

bash
Copy code
git clone <repository-url>
cd rule-engine-app
Set up PostgreSQL Database:

Create a database named RuleEngineDb.
Run migrations or DDL scripts if any, or let JPA create the tables.
Configure Application Properties: Update src/main/resources/application.properties with your database credentials.

properties
Copy code
spring.datasource.url=jdbc:postgresql://localhost:5432/RuleEngineDb
spring.datasource.username=your_db_username
spring.datasource.password=your_db_password
Build and Run the Application:

bash
Copy code
mvn clean install
mvn spring-boot:run
Test the APIs using Postman: The base URL for the API is http://localhost:8080/api/v1.

Endpoints and Postman Commands
Creating a Rule
Endpoint: POST /api/rules
Description: Creates a new rule in the system using a rule string.
Example Request (Body):
json
Copy code
{
  "ruleString": "age > 25 AND income > 50000"
}
Postman Command:
bash
Copy code
curl -X POST http://localhost:8080/api/rules \
-H "Content-Type: application/json" \
-d '{"ruleString": "age > 25 AND income > 50000"}'
Retrieving All Rules
Endpoint: GET /api/rules
Description: Fetches all existing rules.
Postman Command:
bash
Copy code
curl -X GET http://localhost:8080/api/rules
Evaluating a Rule
Endpoint: POST /api/rules/evaluate
Description: Evaluates a rule against given attribute data.
Example Request (Body):
json
Copy code
{
  "ast": {
    "root": {
      "type": "AND",
      "left": {
        "type": "attribute",
        "value": "age > 25"
      },
      "right": {
        "type": "attribute",
        "value": "income > 50000"
      }
    }
  },
  "data": {
    "age": 30,
    "income": 55000
  }
}
Postman Command:
bash
Copy code
curl -X POST http://localhost:8080/api/v1/rules/evaluate \
-H "Content-Type: application/json" \
-d '{
      "ast": {
        "root": {
          "type": "AND",
          "left": {
            "type": "attribute",
            "value": "age > 25"
          },
          "right": {
            "type": "attribute",
            "value": "income > 50000"
          }
        }
      },
      "data": {
        "age": 30,
        "income": 55000
      }
    }'
Combining Rules
Endpoint: POST /api/rules/combine
Description: Combines two or more rules into a single rule.
Example Request (Body):
json
Copy code
{
  "ruleIds": [1, 2],
  "operation": "OR"
}
Postman Command:
bash
Copy code
curl -X POST http://localhost:8080/api/rules/combine \
-H "Content-Type: application/json" \
-d '{"ruleIds": [1, 2], "operation": "OR"}'
Exception Handling
Invalid Rule Format:
If a rule is provided in an invalid format, the API returns a 400 Bad Request with a descriptive error message.
Missing Data:
When required data is missing, a 400 Bad Request is returned.
Example Responses
Success Response
json
Copy code
{
  "status": "success",
  "message": "Rule evaluated successfully",
  "result": true
}
Error Response
json
Copy code
{
  "status": "error",
  "message": "Invalid rule format"
}
Future Improvements
Add complex rule management with priority levels.
Implement rule versioning.
Build a user-friendly frontend for rule management.
