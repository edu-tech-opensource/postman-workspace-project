# Postman Framework

🚀 This repository contains the Postman Framework for testing various endpoints. The framework utilizes Postman collections and environments for organized and automated API testing.

Want to know how to write tests in POSTMAN? 

click here [Postman-Test-Scripts](https://medium.com/@alex_rodriguez_soto/postman-test-scripts-4c6ef7a39f5f)

click here [Newman-Execution-Guide](https://medium.com/@alex_rodriguez_soto/step-by-step-guide-to-running-newman-tests-8f990a639343)

<img width="556" alt="image" src="https://github.com/edu-tech-opensource/postman-workspace-project/assets/151805074/325efc64-cd8c-4229-851c-57af137e92ee">


## API Documentation

### Swagger Documentation

Explore the API endpoints using the Swagger documentation:

- [Design - Projects](https://api.octoperf.com/swagger-ui/index.html#/Design%20-%20Projects/aUsingPOST_25)
  - Endpoint: `/Design - Projects`
  - Method: `POST`
  - Documentation: [Swagger - Design - Projects](https://api.octoperf.com/swagger-ui/index.html#/Design%20-%20Projects/aUsingPOST_25)

## User Interface

Access the user interface for additional interactions:

- [OctoPerf UI](https://api.octoperf.com/ui/access/login)
  - Login Page: [Login](https://api.octoperf.com/ui/access/login)

## Postman Files

- **QA Environment:** [QA.postman_environment.json](workspace/QA.postman_environment.json)
- **Workspace Collection:** [workSpace.postman_collection.json](workspace/workSpace.postman_collection.json)

### Endpoints

1. **User Login**
   - Method: POST
   - Endpoint: `/public/users/login`

2. **Create Workspace**
   - Method: POST
   - Endpoint: `/workspaces`

3. **Get Workspace by ID**
   - Method: GET
   - Endpoint: `/workspaces/{{WorkspaceId}}`

4. **Create Design Project**
   - Method: POST
   - Endpoint: `/design/projects`

5. **Get Design Project by ID**
   - Method: GET
   - Endpoint: `/design/projects/{{projectId}}`

6. **Delete Design Project by ID**
   - Method: DELETE
   - Endpoint: `/design/projects/{{projectId}}`

7. **Delete Workspace by ID**
   - Method: DELETE
   - Endpoint: `/workspaces/{{WorkspaceId}}`

## GitLab CI Configuration

```yaml
stages:
    - verify-set-up
    - test-run

set-up environment:
    stage: verify-set-up
    script:
        - echo "Verify workspace"
        - ls -lh
        - git ls-tree -d origin/master:{Project}

qa-test:
    stage: test-run
    image:
        name: postman/newman
        entrypoint: [""]
    script:
        - newman --version
        - npm install -g newman-reporter-htmlextra
        - newman run workspace/workspaces.postman_collection.json -e workspace/QA_Environment.postman_environment.json -r htmlextra,cli,json --reporter-htmlextra-export target/testHtml.html --reporter-json-export target/testJson.json

    artifacts:
        expire_in: 1 week
        when: always
        paths:
            - target/testHtml.html
            - target/testJson.json
```

## GitLab CI Configuration

```yaml
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - run:
          name: Install Dependencies
          command: |
            sudo npm install -g newman
            sudo npm install -g newman-reporter-htmlextra
            npm install
      - run:
          name: Run Newman tests
          command: |
            newman run workSpace.postman_collection.json -e Test.postman_environment.json -r htmlextra,cli,json --reporter-htmlextra-export target/testHtml.html --reporter-json-export target/testJson.json
      - store_artifacts:
          path: target/testHtml.html
          destination: reports/
      - store_artifacts:
          path: target/testJson.json
          destination: test-results/
```
