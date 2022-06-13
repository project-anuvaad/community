---
description: Login and auth token
---

# Login and auth token

Before making an API call, the application should call this API first to receive authorization tokens. Once an application has valid token the same can be used to make all subsequent calls, please note token can expire and hence it is good practice to validate the token.

| API endpoint                | Description                                 | API contracts                                                                                                                                                                                          |
| --------------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **/**v1/users/login         | To login the user. User email and password. | [ /v1/users/login contract](https://raw.githubusercontent.com/project-anuvaad/anuvaad/user-management\_feature/anuvaad-api/anuvaad-user-management/user-management/docs/ums\_contract.yaml)            |
| /v1/users/auth-token-search | To check validity of the token              | /[v1/users/auth-token-search contract](https://raw.githubusercontent.com/project-anuvaad/anuvaad/user-management\_feature/anuvaad-api/anuvaad-user-management/user-management/docs/ums\_contract.yaml) |
|                             |                                             |                                                                                                                                                                                                        |
