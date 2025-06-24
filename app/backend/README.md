# Backend Service Documentation

## Overview

This backend is part of an AI-powered search and chat application leveraging Azure OpenAI, Azure Cognitive Search, Azure Blob Storage, and other Azure services. It is built using Python (Quart framework) and is designed for secure, scalable, and extensible deployment on Azure or locally.

## Features
- **Conversational AI**: Multi-turn chat and Q&A using Azure OpenAI models (including GPT-4V and ChatGPT).
- **Document Retrieval**: RAG (Retrieval-Augmented Generation) with Azure Cognitive Search and Blob Storage.
- **User File Uploads**: Secure upload, listing, and deletion of user files to Azure Data Lake Storage.
- **Speech Synthesis**: Optional Azure Speech Service integration for text-to-speech.
- **Authentication**: Azure AD-based authentication and access control.
- **Telemetry**: Integrated with Azure Monitor and OpenTelemetry for observability.
- **Extensible Approaches**: Modular design for custom retrieval and chat approaches.

## Directory Structure
- `main.py`: Entrypoint for the backend app.
- `app.py`: Main Quart app, API routes, and service setup.
- `config.py`: Configuration constants.
- `prepdocs.py`: Document ingestion and processing utilities.
- `requirements.txt`/`requirements.in`: Python dependencies.
- `Dockerfile`: Containerization for deployment.

## Setup & Running

### Prerequisites
- Python 3.11+
- Azure resources: Cognitive Search, Blob Storage, OpenAI, (optionally) Speech Service
- Environment variables for Azure credentials and resource names

### Local Development
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Set required environment variables (see below).
3. Start the backend:
   ```bash
   python main.py
   ```
   Or using the provided shell script:
   ```bash
   ./start.sh
   ```

### Docker
Build and run the backend in a container:
```bash
docker build -t backend-app .
docker run -p 8000:8000 --env-file .env backend-app
```

## Configuration
Set the following environment variables (examples):
- `AZURE_STORAGE_ACCOUNT`, `AZURE_STORAGE_CONTAINER`
- `AZURE_SEARCH_SERVICE`, `AZURE_SEARCH_INDEX`
- `AZURE_OPENAI_SERVICE`, `AZURE_OPENAI_CHATGPT_MODEL`, etc.
- `AZURE_TENANT_ID`, `AZURE_CLIENT_ID`, `AZURE_SERVER_APP_ID`, etc. (for authentication)
- `USE_GPT4V`, `USE_USER_UPLOAD`, `ENABLE_LANGUAGE_PICKER`, etc. (feature toggles)

See `app.py` for the full list of supported variables.

## API Endpoints (selected)
- `POST /ask` — Single-turn Q&A
- `POST /chat` — Multi-turn chat
- `POST /chat/stream` — Streaming chat responses
- `POST /upload` — Upload user files
- `POST /delete_uploaded` — Delete user files
- `GET /list_uploaded` — List user files
- `POST /speech` — Text-to-speech (if enabled)
- `GET /config` — Feature/configuration flags for frontend

## Authentication
- Azure AD authentication is enforced if `AZURE_USE_AUTHENTICATION` is set.
- Access control for files and chat history is supported.

## Telemetry
- Azure Monitor and OpenTelemetry are integrated for distributed tracing and logging.

## Extending
- Add new approaches in the `approaches/` directory and register them in `app.py`.
- Add new endpoints to the Quart app as needed.

## License
See [LICENSE](../../LICENSE).
