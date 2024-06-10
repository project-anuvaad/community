## Project Anuvaad Repository Overview

The project Anuvaad repository serves as the primary codebase for the Anuvaad project, aimed at facilitating document processing and translation tasks efficiently.

### Purpose of Folders

- **anuvaad-api**: Houses standalone APIs utilized within the project, such as login and analytics functionalities.
- **anuvaad-fe**: Contains frontend-related code, responsible for the user interface and interaction aspects of the application.
- **chrome-extension**: Hosts code relevant to the Anuvaad Chrome extension, offering additional features and integrations within the Chrome browser environment.
- **anuvaad-nmt-inference [legacy]**: Previously held legacy OpenNMT Python-based inference code. Deprecated and not actively utilized within the current project framework.
- **anuvaad-etl**: Comprises sub-modules dedicated to document processing tasks, enhancing the extraction, transformation, and loading capabilities within the Anuvaad ecosystem.

### Microservice Structure

As an application, the Workflow Manager, in conjunction with independent APIs, forms the foundational architecture of Anuvaad. The Workflow Manager facilitates communication among various modules and orchestrates their interactions. However, Anuvaad's design accommodates diverse use cases, allowing each module to operate autonomously when necessary. For instance, the Tokenizer service can function independently to tokenize an Indic sentence without reliance on other modules.

### Components of Each Microservice

Each microservice within Anuvaad adheres to a consistent structure, comprising the following common elements:

- **Dockerfile**: Provides instructions to build the individual microservice within a Docker container, ensuring portability and consistency across different environments.
- **docs Folder**: Contains documentation outlining the API contracts necessary for running and testing the module independently. This documentation serves as a reference for developers and users alike.
- **config Folder**: Stores module-specific configurations and secrets required for the proper functioning of the microservice. Centralizing configuration management simplifies deployment and maintenance tasks.
- **kafkawrapper**: Defines Kafka/WFM (Workflow Manager) related communication protocols, facilitating seamless integration and communication between modules. In the production environment, the Workflow Manager plays a crucial role in establishing communication channels, rendering standalone APIs redundant.
