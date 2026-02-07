# 🤖 LeIA - AI-Powered Assistant (Azure Serverless)

**LeIA** is a scalable, enterprise-grade intelligent assistant built on a **Serverless Architecture** using Microsoft Azure. This project implements real-time token streaming and automated session context management.

## 🏗️ System Architecture

The solution follows cloud-native patterns to ensure high availability, security, and low latency.

### 📊 Logical Flow
Below is the architectural flow designed for this project, representing the interaction between the Azure Function, AI Foundry, and Storage layers.

```mermaid
graph TD
    endpoint_prompts["Endpoint HTTP /api/prompts (Azure Functions - Isolated Worker)"]
    ai_foundry_connector["AI Foundry Connector (Token Generation & Streaming)"]
    context_store["Context Store (Azure Table Storage / Azurite)"]
    key_vault["Key Vault (Secrets Management)"]
    application_insights["Application Insights (Observability)"]
    auth["Authentication & Authorization (MSI)"]
    networking["Networking & Private Endpoints"]

    endpoint_prompts --> ai_foundry_connector
    endpoint_prompts --> context_store
    context_store --> ai_foundry_connector
    ai_foundry_connector --> context_store
    ai_foundry_connector --> application_insights
    endpoint_prompts --> application_insights
    endpoint_prompts --> key_vault
    ai_foundry_connector --> key_vault
    auth --> endpoint_prompts
    networking --> endpoint_prompts
    networking --> ai_foundry_connector
```
  📸 Interface & User Experience
The frontend is a responsive chat interface that consumes the Server-Sent Events (SSE) stream from the backend.

Figure 1: Real-time token streaming and UI responsiveness.

🛠️ Key Components & Technologies
Backend Engineering
Azure Functions (.NET 8 Isolated Worker): High-performance ingestion layer using the latest isolated process model.

C# / .NET 8: Strongly typed logic ensuring reliability and clean architecture.

AI & Data Management
AI Foundry (GPT-4o/mini): Optimized connectors for generating high-quality AI responses.

Azure Table Storage: Partitioned by sessionId for efficient and cost-effective context persistence.

Figure 2: Session history being persisted in Azure Table Storage.

🛡️ Security & Governance
Security was a priority in this architecture, following Kumulus's high standards for enterprise solutions:

Secret Management: All sensitive data (API Keys) are stored in Azure Key Vault.

Managed Identity (MSI): Passwordless authentication between services.

Data Governance: Implemented logic for data retention and PII masking.
