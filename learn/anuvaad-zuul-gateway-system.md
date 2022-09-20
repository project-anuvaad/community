# Anuvaad Zuul Gateway System



### Overview

This document explains how Netflix Zuul is used as an API Gateway in Anuvaad to perform Authentication, Authorization and API redirection to all inbound API calls. Zuul is an Open Source Project.

### Getting Started

Zuul is an API Gateway developed as an Open Source Project by Netflix. Zuul provides various features to abstract out some of the common operations of the system and provide a strong layer for Authentication, Authorization, API Pre & Post Hooks, API Throttling, Session monitoring and much more. Zuul: Zuul is an edge service that proxies requests to multiple backing services. It provides a unified “front door” to your system, which allows a browser, mobile app, or other user interface to consume services from multiple hosts without managing cross-origin resource sharing (CORS) and authentication for each one.

### Zuul in Anuvaad

Zuul is Anuvaad is a config driven implementation where APIs, Roles, Role Actions are read by Zuul through a file stored in a remote repository.

### Roles

The set of Roles defined in the system, these will be attached to the Users and also mapped with the APIs to provide role based access control (RBAC).

### Actions

Set of APIs exposed in the system by various microservices. Each action is an API which will be mapped against the roles. APIs are of 2 types: Open APIs and Closed APIs. Open APIs can be accessed without authentication, in other words: these APIs are whitelisted. Closed APIs can only be accessed after auth checks.

### role-actions:

Mapping between the roles and actions, Zuul uses this to decide if the User should be allowed to access a particular API. These configs can be found here: [https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-api/anuvaad-zuul-api-gw/dev-configs](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-api/anuvaad-zuul-api-gw/dev-configs)

### Authentication in Anuvaad:

Anuvaad uses JWT auth tokens for authentication and authorization purposes. The same token is also used as the session ID. These tokens are generated and stored securely by a UMS system. Example: [https://www.getpostman.com/collections/d91b48529bc5f0474617](https://www.getpostman.com/collections/d91b48529bc5f0474617)

### Source Code

``[`Source Code for`` `**`Anuvaad Zuul`**](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-api/anuvaad-zuul-api-gw)**``**

Anuvaad Zuul uses 3 Pre Filters namely: Correlation, Auth, Rbac. Correlation: Filter to add a correlation ID to the inbound request. Auth: Filter to perform Authentication check on the inbound request. Rbac: Filter to perform Authorization check on the inbound request. API redirection configuration is provided in the [application.properties](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-api/anuvaad-zuul-api-gw/anuvaad-zuul/src/main/resources/application.properties)

[Zuul repo](https://github.com/Netflix/zuul)

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
