# Project Overview

LibreChat is a free and open-source chat application that emulates the functionality of ChatGPT. It provides a user interface inspired by the original ChatGPT, but with enhanced features and the ability to integrate with multiple AI models. The project is a monorepo containing the main API, a client-side application, and several packages.

The application is built with Node.js and React, and it uses MongoDB as its primary database, MeiliSearch for search, and a vector database for retrieval-augmented generation.

## Key Features

*   **Multi-AI Model Support:** Integrate with various AI models from providers like Anthropic, AWS Bedrock, OpenAI, Google, and more.
*   **Custom Endpoints:** Use any OpenAI-compatible API with LibreChat.
*   **Code Interpreter:** Securely execute code in multiple languages.
*   **Agents & Tools:** Build and share custom AI agents with various capabilities.
*   **Web Search:** Enhance AI context with web search results.
*   **Generative UI:** Create React, HTML, and Mermaid diagrams directly in chat.
*   **Image Generation & Editing:** Generate and edit images using various models.
*   **Multimodal & File Interactions:** Chat with images and files.
*   **Multi-User & Secure Access:** Secure authentication with OAuth2, LDAP, & Email Login Support.
*   **Customizable:** Configure various aspects of the application through the `librechat.yaml` file.

# Building and Running

## Development

To run the application in a development environment, you can use the following commands:

```bash
# Start the backend development server
npm run backend:dev

# Start the client-side development server
npm run frontend:dev
```

## Docker

The project also includes a `docker-compose.yml` file for running the application in a production-like environment. To use it, you can run:

```bash
# Start the application
docker-compose up -d

# Stop the application
docker-compose down
```

# Testing

The project has a suite of tests that can be run with the following commands:

```bash
# Run all tests
npm test

# Run end-to-end tests
npm run e2e
```

# Configuration

The application can be configured through the `librechat.yaml` file. This file allows you to customize various aspects of the application, such as:

*   **File Storage:** Configure different storage strategies (local, S3, Firebase) for various file types.
*   **UI:** Customize the welcome message, privacy policy, and terms of service.
*   **Authentication:** Configure social logins and allowed domains for registration.
*   **AI Endpoints:** Configure various AI models and endpoints, including custom ones.
*   **Rate Limiting:** Set rate limits for file uploads and conversation imports.
*   **Actions and MCP Servers:** Configure allowed domains for actions and define MCP servers.
*   **Memory:** Configure memory settings for user personalization.

# Development Conventions

The project uses ESLint for linting and Prettier for code formatting. There are also pre-commit hooks set up with Husky to ensure that code is properly formatted and linted before being committed.