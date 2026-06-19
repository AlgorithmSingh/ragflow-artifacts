# RAGFlow — DeepWiki (Full Export)

> Source: https://deepwiki.com/infiniflow/ragflow  
> Repository: infiniflow/ragflow  
> Indexed commit: d32e05 (13 June 2026)  
> Exported: 2026-06-17  
> Pages: 62


---


## Table of Contents

- [1 Overview](#1-overview)
- [2 Getting Started And Deployment](#2-getting-started-and-deployment)
- [2.1 Docker Compose Deployment](#21-docker-compose-deployment)
- [2.2 Configuration Management](#22-configuration-management)
- [2.3 Document Engine Selection](#23-document-engine-selection)
- [2.4 Build System And Cicd](#24-build-system-and-cicd)
- [2.5 Kubernetes And Helm Deployment](#25-kubernetes-and-helm-deployment)
- [3 System Architecture](#3-system-architecture)
- [3.1 Core Application Services](#31-core-application-services)
- [3.2 Data Storage Architecture](#32-data-storage-architecture)
- [3.3 Task Execution And Queue System](#33-task-execution-and-queue-system)
- [3.4 Component Dynamic Loading](#34-component-dynamic-loading)
- [3.5 Go Server And Native Components](#35-go-server-and-native-components)
- [4 Frontend Application](#4-frontend-application)
- [4.1 Internationalization System](#41-internationalization-system)
- [4.2 Ui Component Architecture](#42-ui-component-architecture)
- [4.3 Theming System](#43-theming-system)
- [4.4 Page Structure And State Management](#44-page-structure-and-state-management)
- [5 Llm Integration System](#5-llm-integration-system)
- [5.1 Llmbundle And Model Types](#51-llmbundle-and-model-types)
- [5.2 Provider Implementations](#52-provider-implementations)
- [5.3 Error Handling And Retry Logic](#53-error-handling-and-retry-logic)
- [5.4 Tenant Configuration And Model Management](#54-tenant-configuration-and-model-management)
- [5.5 Tool Calling And Function Use](#55-tool-calling-and-function-use)
- [6 Document Processing Pipeline](#6-document-processing-pipeline)
- [6.1 Document Parsing Strategies](#61-document-parsing-strategies)
- [6.2 Chunking Methods](#62-chunking-methods)
- [6.3 Content Enhancement And Embedding](#63-content-enhancement-and-embedding)
- [6.4 Vision Processing: Ocr And Layout Recognition](#64-vision-processing-ocr-and-layout-recognition)
- [6.5 Advanced Features: Graphrag And Raptor](#65-advanced-features-graphrag-and-raptor)
- [6.6 Data Source Connectors](#66-data-source-connectors)
- [7 Backend Api System](#7-backend-api-system)
- [7.1 Api Architecture And Sdk](#71-api-architecture-and-sdk)
- [7.2 Authentication And Authorization](#72-authentication-and-authorization)
- [7.3 User And Tenant Management](#73-user-and-tenant-management)
- [7.4 Dataset And Knowledge Base Apis](#74-dataset-and-knowledge-base-apis)
- [7.5 Document And File Management Apis](#75-document-and-file-management-apis)
- [7.6 Chat And Conversation Apis](#76-chat-and-conversation-apis)
- [7.7 Agent And Memory Apis](#77-agent-and-memory-apis)
- [7.8 System Status And Health Monitoring](#78-system-status-and-health-monitoring)
- [8 Agent And Workflow System](#8-agent-and-workflow-system)
- [8.1 Canvas Engine And Dsl](#81-canvas-engine-and-dsl)
- [8.2 Component System Architecture](#82-component-system-architecture)
- [8.3 Built In Components](#83-built-in-components)
- [8.4 Workflow Execution And Streaming](#84-workflow-execution-and-streaming)
- [8.5 State And Variable Management](#85-state-and-variable-management)
- [8.6 Agent Tools And React Loop](#86-agent-tools-and-react-loop)
- [8.7 Canvas Api And Management](#87-canvas-api-and-management)
- [9 Retrieval And Search System](#9-retrieval-and-search-system)
- [9.1 Document Store Abstraction](#91-document-store-abstraction)
- [9.2 Query Processing And Hybrid Search](#92-query-processing-and-hybrid-search)
- [9.3 Reranking And Filtering](#93-reranking-and-filtering)
- [9.4 Response Generation And Citations](#94-response-generation-and-citations)
- [10 Memory System](#10-memory-system)
- [10.1 Memory Types And Storage](#101-memory-types-and-storage)
- [10.2 Memory Management Ui And Api](#102-memory-management-ui-and-api)
- [11 Administration And Operations](#11-administration-and-operations)
- [11.1 Admin Service And Cli](#111-admin-service-and-cli)
- [11.2 Sandbox Code Executor](#112-sandbox-code-executor)
- [11.3 Testing Infrastructure](#113-testing-infrastructure)
- [11.4 Migration And Utility Tools](#114-migration-and-utility-tools)
- [12 Glossary](#12-glossary)


---



<!-- ===== 1-overview ===== -->

# Overview

Relevant source files
- .github/workflows/release.yml
- .github/workflows/tests.yml
- Dockerfile
- Dockerfile.deps
- README.md
- README_ar.md
- README_fr.md
- README_id.md
- README_ja.md
- README_ko.md
- README_pt_br.md
- README_tr.md
- README_tzh.md
- README_zh.md
- docker/.env
- docker/README.md
- docker/docker-compose-base.yml
- docker/infinity_conf.toml
- docs/guides/manage_files.md
- docs/quickstart.mdx
- download_deps.py
- helm/values.yaml
- pyproject.toml
- sdk/python/pyproject.toml
- sdk/python/uv.lock
- uv.lock

## Purpose and Scope

RAGFlow is an open-source Retrieval-Augmented Generation (RAG) engine that integrates deep document understanding with agentic workflow capabilities. It is engineered to transform complex, unstructured data into high-fidelity, production-ready AI systems by combining intelligent document parsing, embedding, and retrieval with configurable LLM-powered agents and workflows. [README.md76-78](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L76-L78)

**Core Characteristics**: RAGFlow specializes in parsing complex document formats such as PDF, DOCX, Excel, and PPT using state-of-the-art deep learning models for document layout and table extraction. It segments documents into meaningful chunks using multiple, template-driven chunking strategies, generates contextual embeddings for these chunks, and stores them in a hybrid vector and keyword index. It supports conversational AI applications backed by traceable, grounded citations. Additionally, RAGFlow includes a visual agent workflow system called Canvas for creating multi-step AI agents with persistent memory. [README.md112-122](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L112-L122) [pyproject.toml4](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L4-L4)

**Key Value Statement**: "Quality in, quality out" — the system prioritizes deep document understanding to reliably find relevant information even from vast, unbounded token spaces. [README.md114-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L114-L118)

**Sources**: [README.md76-122](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L76-L122) [pyproject.toml4](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L4-L4)

## System Architecture

RAGFlow is built upon a multi-tier microservices architecture, optimizing scalability and decoupling synchronous API operations from intensive background processing tasks. The architecture follows a producer-consumer pattern with Redis Streams managing task queues. [docker/.env140-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L140-L146)

### High-Level Architecture Diagram

```
Storage_Layer

Worker_Layer

Service_Logic_Layer

API_Orchestration_Layer

Client_Layer

Processing_Engines

DeepDoc Vision
deepdoc/vision/

GraphRAG
rag/graphrag/

Sandbox Executor
gVisor-based

Web UI
web/

Python SDK
sdk/python/ragflow_sdk

REST API
api/apps/

Quart Server
api/ragflow_server.py
Port 9380

Go Server
cmd/server_main.go
Port 9384

MCP Server
mcp/server/server.py

UserService
api/db/services/user_service.py

DocumentService
api/db/services/document_service.py

LLMBundle
api/db/services/llm_service.py

DialogService
api/db/services/dialog_service.py

TaskExecutor
rag/svr/task_executor.py

Redis Streams
REDIS_HOST

Go Ingestor
cmd/ingestor.go

Document Store
ES / Infinity / OpenSearch / OceanBase

MySQL / PostgreSQL
api/db/db_models.py

Object Storage
minio
```

**Sources**: [README.md141-145](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L141-L145) [docker/.env13-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L13-L159) [docker/docker-compose-base.yml1-230](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L230)

### Three-Tier Architecture

RAGFlow is logically divided into three tiers:

1. **Frontend/API Tier**:
Exposes REST and SDK endpoints via aQuartserver running in asynchronous mode allowing concurrent streaming responses.pyproject.toml100-101pyproject.toml145Supports hybrid deployment with a Go server (cmd/server_main.go) for native components and advanced search handling.docker/.env161-162API proxying is configurable through environment variables (API_PROXY_SCHEME).docker/.env161-1622. **Asynchronous Task Tier**:
Background workers implement document parsing, embedding, and indexing asynchronously.Tasks are queued in Redis Streams and consumed by theTaskExecutoror the Go-basedIngestor..github/workflows/tests.yml132-139Supports an orchestrable ingestion pipeline for complex data flows.README.md973. **Persistence Tier**:
Utilizes a polyglot persistence approach:
  - Relational Database: MySQL (8.0.39) or PostgreSQL manages metadata, user, tenancy, and settings via the Peewee ORM. docker/docker-compose-base.yml186-188 pyproject.toml133
  - Document Store: Pluggable engines including Elasticsearch, Infinity, OpenSearch, and OceanBase for vector and keyword indexing. docker/.env13-20
  - Object Storage: MinIO for raw files and images. docker/docker-compose-base.yml214-215
  - Cache/Queue: Redis (Valkey) for task orchestration and ephemeral state. docker/.env140-146 pyproject.toml121

**Sources**: [pyproject.toml9-169](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L9-L169) [docker/.env13-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L13-L159) [docker/docker-compose-base.yml1-230](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L230)

## Core Components and Services

### System Entity Mapping Diagram

Bridging natural language descriptions of system functions and their implementation code entities, this diagram relates the major conceptual components to their main code locations:

```
Code_Entity_Space

Natural_Language_Concepts

Knowledge Base / Dataset

Document Parsing / Ingestion Pipeline

Hybrid Search, Ranking

AI Agent / Canvas Workflow

DocumentService
api/db/services/document_service.py

TaskExecutor
rag/svr/task_executor.py

Ingestor
cmd/ingestor.go

SearchDealer
internal/service/search/

Canvas
internal/agent/canvas/
```

**Sources**: [.github/workflows/tests.yml132-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L132-L139) [README.md93-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L93-L101)

### Application Entry Points

| Service/Subsystem | Main File(s) | Description |
| --- | --- | --- |
| Python REST API Server | `api/ragflow_server.py` | Quart-based async API server handling REST endpoints and SDK. |
| Go Server | `cmd/server_main.go` | High-performance backend server layer with native components. |
| Task Executor Worker | `rag/svr/task_executor.py` | Python background task worker for document ingestion. |
| Go Ingestor | `cmd/ingestor.go` | Go-based ingestion worker for high-throughput processing. |
| MCP Server | `mcp/server/server.py` | Model Context Protocol server for agent communication. |
| Admin Server | `cmd/admin_server.go` | Go-based administration service. |

**Sources**: [.github/workflows/tests.yml132-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L132-L139) [docker/.env156-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L156-L159) [pyproject.toml68](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L68-L68)

## Key Features

- Deep Document Understanding: Extracts structured knowledge from complex formats (PDF, DOCX, etc.) using layout recognition and OCR. README.md116-117
- Template-Based Chunking: Offers intelligent chunking strategies (naive, book, paper, laws, QA) to ensure contextually relevant segments. README.md120-124
- Agentic Workflow (Canvas): A visual orchestration system for creating multi-step AI agents with tool-calling capabilities and MCP support. README.md99
- Memory System: Supports persistent 'Memory' for AI agents, including episodic and procedural types. README.md93
- Hybrid Search: Combines vector retrieval with BM25 keyword search, supported by fused re-ranking for precision. README.md137-138
- Data Source Connectors: Supports data synchronization from Confluence, S3, Notion, Discord, and Google Drive. README.md95
- Sandbox Code Executor: gVisor-based sandbox for safe execution of Python/JS code within agent workflows. README.md100 docker/docker-compose-base.yml158-161

**Sources**: [README.md75-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L75-L140) [docker/.env13-20](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L13-L20) [docker/docker-compose-base.yml158-184](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L158-L184)

## Technology Stack

### Backend

- Languages: Python (3.13), Go, C++ (for tokenizer and RAGAnalyzer). pyproject.toml8 .github/workflows/tests.yml132-139
- Frameworks: Quart (Python async), Eino (Go workflow framework). pyproject.toml145
- Database/ORM: Peewee (Python), MySQL 8.0. pyproject.toml133 docker/docker-compose-base.yml188
- AI Integration: LiteLLM (unified LLM access), ONNX Runtime (local inference). pyproject.toml163 pyproject.toml77-78

### Infrastructure

- Deployment: Docker Compose, Kubernetes (Helm). README.md153
- OS Base: Ubuntu 24.04. Dockerfile2
- Dependency Management: uv (Python), Go modules. Dockerfile69-83

**Sources**: [pyproject.toml1-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L1-L180) [Dockerfile1-154](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L1-L154) [docker/.env1-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L1-L165)


---



<!-- ===== 2-getting-started-and-deployment ===== -->

# Getting Started and Deployment

Relevant source files
- .github/workflows/release.yml
- .github/workflows/tests.yml
- Dockerfile
- Dockerfile.deps
- README.md
- README_ar.md
- README_fr.md
- README_id.md
- README_ja.md
- README_ko.md
- README_pt_br.md
- README_tr.md
- README_tzh.md
- README_zh.md
- docker/.env
- docker/README.md
- docker/docker-compose-base.yml
- docker/infinity_conf.toml
- docs/guides/manage_files.md
- docs/quickstart.mdx
- download_deps.py
- helm/values.yaml
- pyproject.toml
- sdk/python/pyproject.toml
- sdk/python/uv.lock
- uv.lock

This document provides a high-level overview of RAGFlow's deployment options and initial setup procedures. It outlines the system prerequisites, primary deployment methods, and the essential configuration files used to run RAGFlow. For in-depth instructions and technical details, please refer to the dedicated child pages:

- Docker Compose Deployment — Detailed guide for deploying RAGFlow using Docker Compose, including service architecture, container configuration, and port mappings
- Configuration Management — Explain the configuration system including service_conf.yaml, environment variables, and the relationship between configuration sources
- Document Engine Selection — Guide for choosing and configuring document engines (Elasticsearch, Infinity, OpenSearch) with their tradeoffs
- Build System and CI/CD — Explain the build pipeline, dependency management with uv/poetry, multi-language compilation (Python, Go, C++), and GitHub Actions testing workflow
- Kubernetes and Helm Deployment — Document the Helm chart structure for Kubernetes deployment, including templates for all services

## Deployment Architecture Overview

RAGFlow utilizes a containerized microservices architecture with each major service encapsulated in its own Docker container or Kubernetes pod. This modular design separates core application logic, data infrastructure, and optional extensions.

### Containerized Service Architecture

```
Optional Services

Task & Cache

Data Persistence

Application Layer

Indexing

SQL Data

Blobs

Async Tasks

Optional

ragflow-cpu-1
(Quart Python + Go Server)
Ports: 80, 9380, 9384

es01 / infinity
(Document Store)
Ports: 1200 / 23820

mysql
(Metadata DB)
Port: 3306

minio
(Object Storage)
Port: 9000

redis / nats
(Task Streams & Cache)
Port: 6379 / 4222

sandbox-executor-manager
(Code Execution)
Port: 9385
```

This architecture highlights the clear separation of concerns:

- RAGFlow CPU container: Hosts the core Quart Python-based API server pyproject.toml145 and the Go backend layer docker/.env158-159
- Elasticsearch / Infinity: Used for document indexing and vector-based search docker/.env13-20
- MySQL: Metadata and configuration storage docker/docker-compose-base.yml186-212
- MinIO: Object storage for uploaded documents docker/docker-compose-base.yml214-230
- Redis: Serves as cache and supports asynchronous task queueing via Redis Streams docker/.env140-146

**Sources:** [docker/docker-compose-base.yml1-240](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L240) [docker/.env1-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L1-L175) [README.md141-145](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L141-L145)

## Configuration Management Overview

RAGFlow’s configuration is managed through multiple layers, typically involving environment variables and templated YAML files.

### Configuration Relationships

```
Injects Variables

Generates

Defines Profiles

Mounts

.env file
(Credentials & Profiles)

service_conf.yaml.template
(Base Configuration)

docker-compose.yml
(Service Definitions)

service_conf.yaml
(Generated Runtime Config)
```

- The .env file defines environment variables like DOC_ENGINE, MYSQL_PASSWORD, and RAGFLOW_IMAGE docker/.env20-165
- docker-compose.yml uses profiles (e.g., elasticsearch, infinity) to conditionally start services based on .env settings docker/docker-compose-base.yml3-4

**Sources:** [docker/.env1-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L1-L175) [docker/docker-compose-base.yml1-100](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L100)

## System Prerequisites

RAGFlow requires specific hardware and software environments to run efficiently.

### Hardware Requirements

| Component | Minimum Requirement |
| --- | --- |
| CPU | x86 Architecture, ≥ 4 cores [README.md150](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L150-L150) |
| RAM | ≥ 16 GB [README.md151](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L151-L151) |
| Disk | ≥ 50 GB [README.md152](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L152-L152) |

> **Note:** Official Docker images are built for x86. ARM64 users must build their own images [docs/quickstart.mdx23-26](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/quickstart.mdx?plain=1#L23-L26)

### Software Requirements

- Docker: ≥ 24.0.0 docs/quickstart.mdx33
- Docker Compose: ≥ v2.26.1 docs/quickstart.mdx33
- Python: ≥ 3.13 pyproject.toml8
- gVisor: Required only for the code sandbox feature docs/quickstart.mdx35

### Kernel Configuration

For Elasticsearch or Infinity to function correctly, `vm.max_map_count` must be set to at least 262144 [docs/quickstart.mdx45-52](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/quickstart.mdx?plain=1#L45-L52)

```
sudo sysctl -w vm.max_map_count=262144
```

**Sources:** [README.md148-155](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L148-L155) [docs/quickstart.mdx28-36](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/quickstart.mdx?plain=1#L28-L36) [pyproject.toml1-10](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L1-L10)

## Deployment Methods

### 1. Docker Compose Deployment (Recommended)

This is the standard way to deploy RAGFlow. It uses the `RAGFLOW_IMAGE` specified in `.env` (e.g., `infiniflow/ragflow:v0.26.0`) [docker/.env165](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L165-L165) For details, see [Docker Compose Deployment](/infiniflow/ragflow/2.1-docker-compose-deployment).

### 2. Source-Based Deployment (Development)

Developers can run RAGFlow from source using the `uv` package manager [Dockerfile69-83](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L69-L83) This requires building the Go and C++ components manually using `build.sh` [.github/workflows/tests.yml138-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L138-L139) For details, see [Build System and CI/CD](/infiniflow/ragflow/2.4-build-system-and-cicd).

### 3. Kubernetes and Helm Deployment

For large-scale production, Helm charts are provided to manage deployments on Kubernetes, including persistent volume claims for MySQL, MinIO, and Document Engines. For details, see [Kubernetes and Helm Deployment](/infiniflow/ragflow/2.5-kubernetes-and-helm-deployment).

**Sources:** [README.md160-195](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L160-L195) [Dockerfile137-160](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L137-L160) [.github/workflows/tests.yml132-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L132-L143)

## RAGFlow Data Flow and Code Entities

This diagram bridges the deployment components with the specific code entry points and services.

```
Storage Entities

Go Runtime

Python Runtime (Quart)

Writes Tasks

Reads Tasks

Writes to

Auth/Meta

Internal Logic

ragflow_server.py
(API Entry)

task_executor.py
(Async Processing)

cmd/server_main.go
(Backend Layer)

cmd/ingestor.go
(Data Ingestion)

Redis Streams
(Task Queue)

MySQL
(Metadata)
```

- Python Server: Handles web requests and publishes tasks to Redis streams docker/docker-compose-base.yml232-250
- Task Executor: Consumes tasks from Redis to perform document parsing and embedding docker/.env140-146
- Go Server: Provides high-performance backend services and interacts with the C++ RAGAnalyzer internal/cpp/

**Sources:** [docker/docker-compose-base.yml1-240](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L240) [docker/.env140-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L140-L162) [.github/workflows/tests.yml132-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L132-L143)

## Document Engine Selection

RAGFlow supports a "Polyglot" document engine strategy. You can switch between engines by changing the `DOC_ENGINE` variable in `docker/.env` [docker/.env13-20](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L13-L20):

- elasticsearch: Industry standard for full-text search.
- infinity: High-performance vector database optimized for RAG.
- oceanbase / seekdb: Enterprise-grade distributed SQL + Vector support.

For details, see [Document Engine Selection](/infiniflow/ragflow/2.3-document-engine-selection).

**Sources:** [docker/.env13-20](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L13-L20) [docker/docker-compose-base.yml3-157](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L3-L157)


---



<!-- ===== 2.1-docker-compose-deployment ===== -->

# Docker Compose Deployment

Relevant source files
- README.md
- README_ar.md
- README_fr.md
- README_id.md
- README_ja.md
- README_ko.md
- README_pt_br.md
- README_tr.md
- README_tzh.md
- README_zh.md
- docker/.env
- docker/README.md
- docker/docker-compose.yml
- docker/entrypoint.sh
- docker/launch_backend_service.sh
- docs/guides/manage_files.md
- docs/quickstart.mdx
- internal/entity/tenant_model.go
- internal/entity/tenant_model_instance.go
- mcp/client/client.py
- mcp/client/streamable_http_client.py
- mcp/server/server.py
- tools/scripts/README.md
- tools/scripts/db_schema_sync.py
- tools/scripts/mysql_migration.py

This page provides a detailed technical guide to deploying RAGFlow using Docker Compose. It covers the service architecture, container configurations, environment variables, port mappings, and the deployment process. This guide is intended for technical operators and developers who want to self-host RAGFlow services in a containerized environment.

## Overview

RAGFlow's Docker Compose deployment orchestrates multiple containerized components forming a complex application stack. The deployment consists of:

- A main compose file docker-compose.yml that manages the RAGFlow CPU/GPU application containers docker/docker-compose.yml1-135
- A base compose file docker-compose-base.yml that manages dependency services like Elasticsearch, MySQL, MinIO, Redis, and alternative document engines docker/docker-compose-base.yml1-245
- Use of Docker Compose profiles to selectively enable services based on hardware (CPU/GPU) and chosen document storage engine docker/.env28

This orchestrated stack delivers the full RAGFlow engine functionality including API servers, background task executors, data stores, and optional components like the Admin Server and MCP Server.

Sources: [docker/docker-compose.yml1-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose.yml#L1-L135) [docker/docker-compose-base.yml1-245](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L245) [README.md141-145](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L141-L145) [docs/quickstart.mdx186-203](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/quickstart.mdx?plain=1#L186-L203)

## Service Architecture

The RAGFlow Docker Compose deployment is designed with a clear separation between client interaction, application logic, and persistence layers. This is articulated via multiple containers communicating over Docker networks and configured ports.

### System Component Diagram

```
DocumentEngines (Profiles)

StorageLayer (Persistence)

CodeEntitySpace (RAGFlow Stack)

NaturalLanguageSpace (Client)

Port: SVR_WEB_HTTP_PORT

Proxy requests

Proxy requests

Metadata queries

Document files

Tasks (queues)

Code execution dispatch

Poll & execute tasks

Index and Search

Index and Search

Query

Query

Admin API calls

Context management

User / API Client

nginx (web proxy)
Configuration: ragflow.conf.python

ragflow-cpu / ragflow-gpu
(Python Quart server: ragflow_server.py)

task_executor.py
(Background Worker processes)

Go Server
(cmd/server_main.go)

Admin Server
(admin_server.py)

MCP Server
(mcp/server/server.py)

sandbox-executor-manager
(Code Execution)

mysql (MySQL 8.0.39)
DB: rag_flow

minio (Object Storage)
S3 Compatible

redis (Valkey 8)
Streams / Cache

es01 (Elasticsearch 8.11.3)

infinity (Infinity v0.7.0)

opensearch01 (OpenSearch 2.19.1)

oceanbase (OceanBase 4.4.1)

seekdb (SeekDB latest)
```

**Key Points:**

- The nginx container acts as a reverse proxy, routing external HTTP requests to the Python backend based on the API_PROXY_SCHEME docker/entrypoint.sh188-206
- The ragflow-cpu or ragflow-gpu containers run the main API server (ragflow_server.py) and task executors (task_executor.py) docker/docker-compose.yml5-109
- The Admin Server and MCP Server are optional components enabled via runtime flags in the command section docker/docker-compose.yml12-31
- Persistent data is stored in dedicated containers: mysql for metadata, minio for object storage, and redis for task streaming docker/docker-compose-base.yml176-245

Sources: [docker/docker-compose.yml1-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose.yml#L1-L135) [docker/docker-compose-base.yml1-245](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L245) [docker/entrypoint.sh188-206](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L188-L206) [README.md141-145](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L141-L145)

## Core Service Components

### Document Storage Engines (Profile-Based)

RAGFlow supports multiple document storage backends. Docker Compose profiles control which engine runs [docker/.env10-28](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L10-L28):

| Service Name | Docker Image | Profile | Default Port Exposed | Description |
| --- | --- | --- | --- | --- |
| `es01` | `elasticsearch:8.11.3` | `elasticsearch` | `${ES_PORT}:9200` | Default full-text and vector search [docker/.env31-38](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L31-L38) |
| `infinity` | `infiniflow/infinity:v0.7.0` | `infinity` | `${INFINITY_HTTP_PORT}:23820` | High-performance vector DB [docker/.env67-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L67-L72) |
| `opensearch01` | `opensearchproject/opensearch:2.19.1` | `opensearch` | `${OS_PORT}:1201` | OpenSearch alternative [docker/.env44-52](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L44-L52) |
| `oceanbase` | `oceanbase/oceanbase-ce:4.4.1.0` | `oceanbase` | `${OCEANBASE_PORT}:2881` | Distributed DB with vector support [docker/.env75-84](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L75-L84) |
| `seekdb` | `oceanbase/seekdb:latest` | `seekdb` | `${SEEKDB_PORT}:2881` | Lightweight OceanBase variant [docker/.env96-105](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L96-L105) |

Sources: [docker/docker-compose-base.yml2-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L2-L146) [docker/.env10-28](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L10-L28) [docker/.env31-105](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L31-L105)

### Persistent Data Services

1. **MySQL (`mysql` service)**: Hosts primary metadata such as datasets and users [docker/.env109-115](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L109-L115)
Image:mysql:8.0.39Health checks usemysqladmin pingdocker/docker-compose-base.yml195-2012. **MinIO (`minio` service)**: Provides S3-compatible object storage for raw documents [docker/.env125-132](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L125-L132)
Image:pgsty/minio:RELEASE.2026-03-25T00-00-00ZCredentials:MINIO_USER,MINIO_PASSWORDdocker/.env133-1383. **Redis (`redis` service)**: Utilized for task queues via Redis Streams [docker/.env140-144](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L140-L144)
Image:valkey/valkey:8docker/docker-compose-base.yml226

Sources: [docker/docker-compose-base.yml176-245](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L176-L245) [docker/.env109-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L109-L146)

## Configuration System

RAGFlow uses a template-based system to generate its runtime service configuration (`service_conf.yaml`) from environment variables provided in `.env`.

### Configuration Generation Flow

```
CodeEntitySpace (Initialization)

NaturalLanguageSpace (Config)

Load env vars

Parse & replace placeholders

Generate final config

Configures server

Configures workers

.env File

service_conf.yaml.template

entrypoint.sh

service_conf.yaml

ragflow_server.py

task_executor.py
```

### Entrypoint Script Logic

The `entrypoint.sh` script performs critical initialization [docker/entrypoint.sh1-216](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L1-L216):

- Template Replacement: Iterates through service_conf.yaml.template and replaces ${VAR:-DEFAULT} patterns with environment values docker/entrypoint.sh161-182
- Nginx Configuration: Selects the appropriate config (hybrid, go, or python) based on API_PROXY_SCHEME docker/entrypoint.sh188-206
- Service Launch: Conditionally starts the web server, task executors, or MCP server based on arguments like --enable-mcpserver docker/entrypoint.sh72-158

Sources: [docker/entrypoint.sh1-216](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L1-L216) [docker/.env158-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L158-L162)

## Deployment Process

### Prerequisites

Host system requirements [docs/quickstart.mdx28-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/quickstart.mdx?plain=1#L28-L35):

- CPU: ≥ 4 cores (x86)
- RAM: ≥ 16 GB
- Disk: ≥ 50 GB
- Docker: ≥ 24.0.0 & Docker Compose ≥ v2.26.1
- Kernel Parameter: vm.max_map_count must be ≥ 262144 for Elasticsearch/Infinity docs/quickstart.mdx45-53

### Standard Deployment Steps

1. **Clone and Navigate**: `git clone https://github.com/infiniflow/ragflow.git cd ragflow/docker` [docs/quickstart.mdx187-192](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/quickstart.mdx?plain=1#L187-L192)
2. **Configure Environment**: Update `.env` with desired `DOC_ENGINE` and `RAGFLOW_IMAGE` [docker/.env20-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L20-L165)
3. **Launch**: `docker compose up -d` [docs/quickstart.mdx203-204](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/quickstart.mdx?plain=1#L203-L204)

Sources: [README.md183-203](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L183-L203) [docs/quickstart.mdx28-204](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/quickstart.mdx?plain=1#L28-L204) [README_zh.md162-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/README_zh.md?plain=1#L162-L181)

## Network and Port Mappings

| Variable | Default | Target | Description |
| --- | --- | --- | --- |
| `SVR_WEB_HTTP_PORT` | `80` | Nginx | Public HTTP endpoint [docker/.env153](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L153-L153) |
| `SVR_HTTP_PORT` | `9380` | Python API | Internal Python backend [docker/.env155](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L155-L155) |
| `ADMIN_SVR_HTTP_PORT` | `9381` | Admin API | Admin management endpoint [docker/.env156](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L156-L156) |
| `SVR_MCP_PORT` | `9382` | MCP Server | Model Context Protocol port [docker/.env157](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L157-L157) |
| `GO_HTTP_PORT` | `9384` | Go Server | Internal Go backend [docker/.env158](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L158-L158) |

Sources: [docker/.env151-160](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L151-L160) [docker/docker-compose.yml32-39](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose.yml#L32-L39)

## Volume and Data Persistence

| Volume / Bind Mount | Mapping | Purpose |
| --- | --- | --- |
| `./ragflow-logs` | `/ragflow/logs` | Log persistence [docker/docker-compose.yml41](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose.yml#L41-L41) |
| `./service_conf.yaml.template` | `/ragflow/conf/service_conf.yaml.template` | Configuration source [docker/docker-compose.yml45](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose.yml#L45-L45) |
| `./entrypoint.sh` | `/ragflow/entrypoint.sh` | Custom startup logic [docker/docker-compose.yml46](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose.yml#L46-L46) |
| `mysql_data` | `/var/lib/mysql` | Database storage [docker/docker-compose-base.yml202](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L202-L202) |
| `minio_data` | `/data` | Object storage [docker/docker-compose-base.yml223](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L223-L223) |

Sources: [docker/docker-compose.yml40-46](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose.yml#L40-L46) [docker/docker-compose-base.yml1-245](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L245)


---



<!-- ===== 2.2-configuration-management ===== -->

# Configuration Management

Relevant source files
- README.md
- README_ar.md
- README_fr.md
- README_id.md
- README_ja.md
- README_ko.md
- README_pt_br.md
- README_tr.md
- README_tzh.md
- README_zh.md
- api/apps/__init__.py
- api/settings.py
- api/utils/api_utils.py
- conf/service_conf.yaml
- docker/.env
- docker/README.md
- docker/service_conf.yaml.template
- docs/guides/manage_files.md
- docs/quickstart.mdx
- test/testcases/test_web_api/test_system_app/test_apps_init_unit.py
- web/src/components/document-preview/md/index.tsx
- web/src/constants/markdown-remark-plugins.ts

This page explains RAGFlow's configuration system, including how environment variables, service configuration files, and runtime settings interact to configure the application and its subsystems. For deployment procedures using these configurations, see **Docker Compose Deployment (2.1)**. For document engine-specific configuration, see **Document Engine Selection (2.3)**.

## Configuration Architecture

RAGFlow uses a multi-tier configuration system that separates environment variables, service configuration files (YAML), and runtime settings. This layered approach enables flexible and dynamic configuration management adaptable to development, testing, and production environments.

The following diagram illustrates the configuration loading and application initialization flow:

### Configuration Processing Flow

```
Application Components

Tier 3: Runtime Settings

Tier 2: Service Configuration

Tier 1: Environment Variables

Variable substitution via Docker Entrypoint

Generates

docker/.env
System environment variables

Runtime Environment Variables (e.g., DOC_ENGINE, MYSQL_PASSWORD)

docker/service_conf.yaml.template

docker/entrypoint.sh
Variable substitution

conf/service_conf.yaml
Generated from template

settings.init_settings()

common.settings module

api.apps: app.config

ragflow_server.py

task_executor.py

API Applications (api.apps)
```

**Explanation:**

- **Tier 1: Environment Variables** The `.env` file inside the `docker/` directory defines environment variables (e.g., `MYSQL_PASSWORD`, `DOC_ENGINE`) injected into containers during Docker Compose startup [docker/.env20-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L20-L146) These variables control system-wide parameters and service selection.
- **Tier 2: Service Configuration** A YAML template file `docker/service_conf.yaml.template` contains placeholders for variables. The entrypoint script performs variable substitution on this template at container startup to produce `conf/service_conf.yaml`, the concrete service configuration file [docker/service_conf.yaml.template1-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/service_conf.yaml.template#L1-L173)
- **Tier 3: Runtime Settings** The Python `common.settings` module initializes the runtime configuration by loading from the YAML file and environment variables [api/apps/__init__.py40](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L40-L40) In the Quart application, these are further mapped to `app.config` for features like session management and timeouts [api/apps/__init__.py73-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L73-L85)

Sources: [docker/.env1-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L1-L165) [docker/service_conf.yaml.template1-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/service_conf.yaml.template#L1-L173) [api/apps/__init__.py40-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L40-L85) [api/utils/api_utils.py49](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L49-L49)

## Environment Variables

Environment variables are the primary means to configure deployment-specific settings and secrets. The file `docker/.env` defines defaults for these variables used in the Docker Compose environment.

### Important Environment Variables Categories

| Category | Variable | Default | Description |
| --- | --- | --- | --- |
| **Document Engine** | `DOC_ENGINE` | `elasticsearch` | Selects search backend: `elasticsearch`, `infinity`, `oceanbase`, `opensearch`, or `seekdb` [docker/.env20](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L20-L20) |
| **Database** | `MYSQL_PASSWORD` | `infini_rag_flow` | MySQL root password [docker/.env111](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L111-L111) |
|  | `MYSQL_PORT` | `3306` | MySQL internal port [docker/.env118](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L118-L118) |
| **Object Storage** | `MINIO_USER` | `rag_flow` | MinIO access user [docker/.env135](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L135-L135) |
|  | `MINIO_PASSWORD` | `infini_rag_flow` | MinIO password [docker/.env138](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L138-L138) |
| **Cache and Queue** | `REDIS_PASSWORD` | `infini_rag_flow` | Redis password [docker/.env146](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L146-L146) |
| **Server Ports** | `SVR_HTTP_PORT` | `9380` | RAGFlow service HTTP port [docker/.env155](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L155-L155) |
|  | `ADMIN_SVR_HTTP_PORT` | `9381` | Admin service HTTP port [docker/.env156](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L156-L156) |
|  | `GO_HTTP_PORT` | `9384` | Go server HTTP port [docker/.env158](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L158-L158) |
| **Application** | `API_PROXY_SCHEME` | `python` | Defines server mode: `python` or hybrid Go backend [docker/.env162](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L162-L162) |

### Additional Deployment Control Variables

- DEVICE: Selects inference backend device (cpu or gpu) docker/.env26
- COMPOSE_PROFILES: Dynamically constructed from DOC_ENGINE and DEVICE to select Docker Compose services docker/.env28
- QUART_RESPONSE_TIMEOUT: Configures Quart server response timeout (default 600s) api/apps/__init__.py73

Sources: [docker/.env20-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L20-L165) [api/apps/__init__.py73-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L73-L85)

## Service Configuration Template

The file `docker/service_conf.yaml.template` is the master configuration blueprint defining all configurable parameters in YAML format with variable placeholders.

### Core Configuration Sections

- ragflow: Network settings and service HTTP ports.
- mysql: Connection parameters including host, port, user, password, and connection limits.
- minio: Object storage credentials and paths.
- es/os/infinity/oceanbase: Specific connection parameters for the chosen document engine.
- redis: Redis connection info including password and host.

### Diagram: Document Engine Configuration Mapping

```
elasticsearch

infinity

opensearch

oceanbase

Environment Variable DOC_ENGINE

es: hosts, username, password

infinity: uri, db_name

os: hosts, username, password

oceanbase: scheme, config (host, port, user, pwd)

common.settings: init_settings()
```

This abstraction allows modular support for multiple engines without changing application code. The chosen configuration is loaded by `common.settings` during system startup [api/apps/__init__.py40](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L40-L40)

Sources: [docker/service_conf.yaml.template1-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/service_conf.yaml.template#L1-L173) [docker/.env20](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L20-L20) [api/apps/__init__.py40](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L40-L40)

## Configuration Loading and Initialization

### Configuration Resolution Priority

The final configuration used at runtime follows this precedence order:

1. Runtime Environment Variables: Direct OS environment variables (e.g., QUART_RESPONSE_TIMEOUT).
2. `.env` File Variables: Variables defined in docker/.env, loaded into the container environment.
3. YAML Configuration File: The generated conf/service_conf.yaml with values substituted from environment variables.

### Diagram: Configuration Entity Mapping

```
Code Entity Space

Natural Language Space

'Database Password'

'Search Engine Type'

'HTTP Response Timeout'

'Secret Key'

MYSQL_PASSWORD (env)

DOC_ENGINE (env)

QUART_RESPONSE_TIMEOUT (env)

SECRET_KEY (env)

conf/service_conf.yaml: mysql.password

docker/.env: DOC_ENGINE

api/apps/init.py: app.config['RESPONSE_TIMEOUT']

settings.get_secret_key()
```

This diagram illustrates the linkage from natural language concepts to environment variables and then to specific code entities or configuration keys.

Sources: [docker/.env111](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L111-L111) [docker/.env20](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L20-L20) [api/apps/__init__.py73](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L73-L73) [api/apps/__init__.py84-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L84-L85)

## Key Runtime Parameters

| Parameter | Source | Usage |
| --- | --- | --- |
| `SECRET_KEY` | `settings.get_secret_key()` | Used for JWT signing and session security [api/apps/__init__.py84-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L84-L85) |
| `SESSION_TYPE` | `app.config` | Set to `redis` for distributed session management [api/apps/__init__.py79](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L79-L79) |
| `MAX_CONTENT_LENGTH` | `os.environ` | Limits the size of file uploads (default 1GB) [api/apps/__init__.py81-83](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L81-L83) |
| `LOGIN_DISABLED` | `app.config` | Optional toggle for development/debug modes [api/apps/__init__.py77](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L77-L77) |

Sources: [api/apps/__init__.py77-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L77-L85) [docker/.env1-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L1-L165)

# Summary

RAGFlow’s configuration management system uses environment variables combined with a service YAML template configured at container startup. The runtime Python config layer further unifies these sources for consistent service initialization, while the Quart application layer maps specific settings to server behavior such as timeouts and session security.

**References for further details:**

- docker/service_conf.yaml.template — master configuration template
- docker/.env — environment variable defaults
- api/apps/__init__.py — Quart application configuration initialization
- api/utils/api_utils.py — common utility for accessing system settings

Sources: [docker/.env1-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L1-L165) [docker/service_conf.yaml.template1-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/service_conf.yaml.template#L1-L173) [api/apps/__init__.py40-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L40-L85) [api/utils/api_utils.py49](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L49-L49)


---



<!-- ===== 2.3-document-engine-selection ===== -->

# Document Engine Selection

Relevant source files
- common/asyncio_utils.py
- common/doc_store/doc_store_base.py
- common/doc_store/es_conn_base.py
- common/doc_store/infinity_conn_base.py
- common/doc_store/infinity_conn_pool.py
- common/settings.py
- conf/infinity_mapping.json
- rag/graphrag/general/smoke.py
- rag/graphrag/light/smoke.py
- rag/graphrag/search.py
- rag/graphrag/utils.py
- rag/svr/cache_file_svr.py
- rag/utils/azure_sas_conn.py
- rag/utils/azure_spn_conn.py
- rag/utils/es_conn.py
- rag/utils/infinity_conn.py
- rag/utils/minio_conn.py
- rag/utils/ob_conn.py
- rag/utils/opensearch_conn.py
- rag/utils/oss_conn.py
- rag/utils/redis_conn.py
- rag/utils/s3_conn.py
- test/unit_test/rag/graphrag/test_graphrag_utils.py
- test/unit_test/rag/utils/test_opensearch_doc_meta.py
- test/unit_test/rag/utils/test_opensearch_hybrid_search.py

This page provides a technical guide for choosing, configuring, and understanding the document engines supported by RAGFlow. Document engines are the core of RAGFlow's retrieval capabilities, providing hybrid search (full-text + vector) and metadata filtering.

## Overview

RAGFlow abstracts document storage through a unified interface, allowing it to support multiple backends. The choice of engine impacts search performance, hardware compatibility (x86 vs ARM), and resource utilization. The selection is controlled primarily via the `DOC_ENGINE` environment variable, which influences both the infrastructure and the application logic.

### Data Flow and Architecture

The document engine sits between the ingestion pipeline (Task Executor) and the retrieval pipeline (RAGFlow Server). The engine implementations provide specialized connection classes that handle search, CRUD, and metadata operations.

**Engine Integration Diagram:**

```
Physical_Storage_Engines

Connection_Implementations

Python_Application_Space

DOC_ENGINE

uses

uses

Factory Pattern

Factory Pattern

Factory Pattern

Factory Pattern

ragflow_server.py

task_executor.py

common/settings.py

common/doc_store/doc_store_base.py

rag/utils/es_conn.py
ESConnection

rag/utils/infinity_conn.py
InfinityConnection

rag/utils/opensearch_conn.py
OSConnection

rag/utils/ob_conn.py
OBConnection

Elasticsearch

Infinity

OpenSearch

OceanBase / SeekDB
```

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L85-L90" min=85 max=90 file-path="common/settings.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L61-L62" min=61 max=62 file-path="rag/utils/es_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L29-L30" min=29 max=30 file-path="rag/utils/infinity_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L64-L65" min=64 max=65 file-path="rag/utils/opensearch_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L31-L33" min=31 max=33 file-path="rag/utils/ob_conn.py">Hii</FileRef>`

## Supported Engines and Tradeoffs

| Engine | `DOC_ENGINE` Value | Best For | Implementation Class |
| --- | --- | --- | --- |
| **Elasticsearch** | `elasticsearch` | Production stability, wide compatibility | `ESConnection` |
| **Infinity** | `infinity` | High-performance RAG, low latency | `InfinityConnection` |
| **OpenSearch** | `opensearch` | AWS-compatible deployments | `OSConnection` |
| **OceanBase** | `oceanbase` | Large-scale distributed storage | `OBConnection` |

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L85-L88" min=85 max=88 file-path="common/settings.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L62-L62" min=62 file-path="rag/utils/es_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L30-L30" min=30 file-path="rag/utils/infinity_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L65-L65" min=65 file-path="rag/utils/opensearch_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L32-L32" min=32 file-path="rag/utils/ob_conn.py">Hii</FileRef>`

### 1. Elasticsearch

Elasticsearch is a common default for RAGFlow with a mature ecosystem and broad compatibility.

- Implementation: The ESConnection class inherits from ESConnectionBase and uses the singleton pattern to ensure a single connection per process <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L61-L62" min=61 max=62 file-path="rag/utils/es_conn.py">Hii</FileRef>.
- Search Logic: Supports hybrid search by combining BM25 textual scoring (MatchTextExpr) and dense vector similarity (MatchDenseExpr), among other expressions <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L25-L25" min=25 file-path="rag/utils/es_conn.py">Hii</FileRef>.
- Pagination: Implements deep pagination support beyond 10,000 hits limit using a _search_with_search_after method that iterates batches of 1,000 results to page through large result sets safely <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L31-L32" min=31 max=32 file-path="rag/utils/es_conn.py">Hii</FileRef>, <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L75-L139" min=75 max=139 file-path="rag/utils/es_conn.py">Hii</FileRef>.
- Feature Updates: Supports atomic incremental adjustments to the pagerank_fea field for chunk feedback with a Painless script (_PAGERANK_FEA_ADJUST_SCRIPT) <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L36-L58" min=36 max=58 file-path="rag/utils/es_conn.py">Hii</FileRef>.
- Robustness: Handles connection retries and timeouts gracefully with configurable timeout settings.

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L25-L139" min=25 max=139 file-path="rag/utils/es_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L36-L62" min=36 max=62 file-path="rag/utils/es_conn.py">Hii</FileRef>`

### 2. Infinity

Infinity is a high-performance RAG-oriented document engine specializing in hybrid search with custom optimizations.

- Implementation: Exposes InfinityConnection class inheriting from InfinityConnectionBase and following the singleton pattern <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L29-L30" min=29 max=30 file-path="rag/utils/infinity_conn.py">Hii</FileRef>.
- Field Conversion: Maps high-level application fields such as important_kwd, docnm_kwd to internal storage field names used in hybrid indexes and supports multiple analyzers like rag-coarse, rag-fine <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L44-L59" min=44 max=59 file-path="rag/utils/infinity_conn.py">Hii</FileRef>.
- Table-per-Knowledgebase: Uses separate tables per knowledge base for fast filtering. It removes kb_id filters for chunk tables and dynamically maps to tables named {indexName}_{kb_id} <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L155-L165" min=155 max=165 file-path="rag/utils/infinity_conn.py">Hii</FileRef>.
- Data Schema: Controlled via conf/infinity_mapping.json supporting types like varchar with complex analyzers and integer/float fields for ranking and filters <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/infinity_mapping.json#L1-L44" min=1 max=44 file-path="conf/infinity_mapping.json">Hii</FileRef>.
- Concurrency Resilience: Implements retry with exponential backoff on RocksDB "Resource busy" errors (code 9003) during concurrent catalog operations such as CREATE/DROP TABLE <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/infinity_conn_base.py#L37-L145" min=37 max=145 file-path="common/doc_store/infinity_conn_base.py">Hii</FileRef>.
- Search Flow: Provides a search() method that interacts with the Infinity connection pool and converts the returned results to pandas DataFrame <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L94-L125" min=94 max=125 file-path="rag/utils/infinity_conn.py">Hii</FileRef>.

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L29-L165" min=29 max=165 file-path="rag/utils/infinity_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/infinity_conn_base.py#L37-L145" min=37 max=145 file-path="common/doc_store/infinity_conn_base.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/infinity_mapping.json#L1-L44" min=1 max=44 file-path="conf/infinity_mapping.json">Hii</FileRef>`

### 3. OpenSearch

OpenSearch offers an AWS-compatible open source fork of Elasticsearch.

- Implementation: OSConnection class wraps an OpenSearch Python SDK client with singleton guarantee <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L64-L65" min=64 max=65 file-path="rag/utils/opensearch_conn.py">Hii</FileRef>.
- Version Check: Enforces minimum OpenSearch version 2.x for compatibility <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L88-L93" min=88 max=93 file-path="rag/utils/opensearch_conn.py">Hii</FileRef>.
- Hybrid Search Pipeline: Automatically initializes a normalization-processor pipeline (requires OpenSearch >= 2.10) to merge BM25 and KNN scores using arithmetic mean with configurable weights <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L106-L152" min=106 max=152 file-path="rag/utils/opensearch_conn.py">Hii</FileRef>.
- Feature Updates: Similar to Elasticsearch, it uses a script to adjust pagerank_fea for ranking feedback <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L37-L59" min=37 max=59 file-path="rag/utils/opensearch_conn.py">Hii</FileRef>.

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L37-L152" min=37 max=152 file-path="rag/utils/opensearch_conn.py">Hii</FileRef>`

### 4. OceanBase

OceanBase utilizes a distributed relational database approach to store RAG chunks and metadata.

- Schema: Uses SQLAlchemy ORM column definitions extensively to define fields, including fields specifically for GraphRAG (entity_kwd, relationships) and RAPTOR hierarchical clustering (raptor_kwd, raptor_layer_int) <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L45-L99" min=45 max=99 file-path="rag/utils/ob_conn.py">Hii</FileRef>.
- Full-text Search Fields: Explicit full-text search columns defined separately for original content (content_with_weight) and tokenized variants (title_tks, important_tks) with weights for ranking <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L116-L131" min=116 max=131 file-path="rag/utils/ob_conn.py">Hii</FileRef>.
- Search Result Model: Uses Pydantic models to represent search results <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L144-L147" min=144 max=147 file-path="rag/utils/ob_conn.py">Hii</FileRef>.
- SQLAlchemy & MySQL Types: Supports complex SQL types for large texts and arrays, enabling flexible storage of rich document chunks <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L25-L30" min=25 max=30 file-path="rag/utils/ob_conn.py">Hii</FileRef>.

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L45-L147" min=45 max=147 file-path="rag/utils/ob_conn.py">Hii</FileRef>`

## Storage Layer Abstraction

Beyond the document engine, RAGFlow abstracts physical file storage (MinIO, S3, Azure, etc.) through the `StorageFactory` and specific connection classes.

- Implementation: The StorageFactory creates instances of storage providers like RAGFlowMinio or RAGFlowS3 based on the STORAGE_IMPL setting <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L198-L212" min=198 max=212 file-path="common/settings.py">Hii</FileRef>.
- Bucket Handling: Decorators like @use_default_bucket and @use_prefix_path ensure that tenant-specific files are correctly mapped within physical buckets <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/minio_conn.py#L52-L89" min=52 max=89 file-path="rag/utils/minio_conn.py">Hii</FileRef>.

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L198-L212" min=198 max=212 file-path="common/settings.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/minio_conn.py#L52-L89" min=52 max=89 file-path="rag/utils/minio_conn.py">Hii</FileRef>`

## Search Operation Data Flow (Infinity Example)

This sequence diagram illustrates the flow from application-level retrieval logic down to the Infinity storage interaction, bridging "Natural Language Space" to "Code Entity Space".

```
Infinity_Server
InfinityConnectionPool
InfinityConnection
Retrieval_Logic
Infinity_Server
InfinityConnectionPool
InfinityConnection
Retrieval_Logic
rag/utils/infinity_conn.py:94-125
rag/utils/infinity_conn.py:44-59
common/doc_store/infinity_conn_pool.py (singleton connection pool)
Handles kb_id separation for chunk tables (indexName_kb_id)
loop
[For each index_name]
search(select_fields, condition, match_expressions)
convert_select_fields(output_fields)
get_conn()
inf_conn_instance
get_database(self.dbName)
get_table(table_name)
pd.DataFrame (search results)
(pd.DataFrame, total_hits)
```

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L44-L59" min=44 max=59 file-path="rag/utils/infinity_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L94-L125" min=94 max=125 file-path="rag/utils/infinity_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L160-L167" min=160 max=167 file-path="rag/utils/infinity_conn.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/infinity_conn_base.py#L150-L165" min=150 max=165 file-path="common/doc_store/infinity_conn_base.py">Hii</FileRef>`

## Technical Configuration via Settings

The selection of document engine is controlled by the environment variable `DOC_ENGINE`. This variable influences which connection class the `common/settings.py` module instantiates and exports as `docStoreConn`.

**Settings Initialization Diagram:**

```
Logic

Initialization

elasticsearch

infinity

oceanbase

opensearch

os.getenv('DOC_ENGINE')

common/settings.py

ESConnection

InfinityConnection

OBConnection

OSConnection
```

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L85-L90" min=85 max=90 file-path="common/settings.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L28-L32" min=28 max=32 file-path="common/settings.py">Hii</FileRef>`

# Summary

This page detailed the different supported document engines in RAGFlow: Elasticsearch, Infinity, OpenSearch, and OceanBase. It covered implementation details, data flow, configuration, and important tradeoffs. The choice of engine depends on deployment needs, performance requirements, and compatibility. RAGFlow provides a uniform abstraction over these engines to allow flexible deployment while maintaining consistent APIs for ingestion, search, and metadata management.

Use the `DOC_ENGINE` environment setting to select and configure the engine at deployment time, and consult the relevant connection class for advanced tuning or troubleshooting.

**End of page.**


---



<!-- ===== 2.4-build-system-and-cicd ===== -->

# Build System and CI/CD

Relevant source files
- .github/workflows/release.yml
- .github/workflows/tests.yml
- Dockerfile
- Dockerfile.deps
- build.sh
- docker/docker-compose-base.yml
- docker/infinity_conf.toml
- download_deps.py
- helm/values.yaml
- internal/development.md
- internal/ingestion/parser/doc_parser.go
- internal/ingestion/parser/docx_parser.go
- internal/ingestion/parser/pdf_parser.go
- internal/ingestion/parser/ppt_parser.go
- internal/ingestion/parser/pptx_parser.go
- internal/ingestion/parser/type.go
- internal/ingestion/parser/xls_parser.go
- internal/ingestion/parser/xlsx_parser.go
- internal/service/file.go
- internal/utility/file.go
- pyproject.toml
- sdk/python/pyproject.toml
- sdk/python/uv.lock
- uv.lock

## Overview

The RAGFlow build system employs state-of-the-art tools and a multi-language compilation process designed for reproducibility, fast dependency management, and integration testing. It supports:

- Python dependency management using the uv package manager with lock file support to ensure consistent builds across environments.
- A multi-stage Docker build process that produces lean runtime containers while compiling complex dependencies and frontend assets.
- Compilation of native components including the Go backend server and C++ tokenizer (RAGAnalyzer), integrated into the build pipeline.
- Dependency pre-caching scripts to facilitate offline builds and mirror acceleration.
- A GitHub Actions CI/CD workflow with static checks, multi-language builds, integration tests across multiple document store backends, and automated release.

This page details the full build lifecycle including tools, data flow, and key commands with diagrams linking the natural language system to the actual code artifacts.

## Package Management with uv

RAGFlow uses [uv](https://github.com/infiniflow/ragflow/blob/d32e05d5/uv) a fast Python package installer and dependency resolver written in Rust, chosen for efficient installs and robust lock files supporting Python `3.13`.

### Project Configuration

The `pyproject.toml` defines the project's metadata, Python version compatibility, and core runtime dependencies which include over 150 Python packages covering web frameworks, LLM SDKs, NLP tools, and cloud SDKs [pyproject.toml1-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L1-L180)

```
[project]
name = "ragflow"
version = "0.26.0"
requires-python = ">=3.13,<3.14"
dependencies = [
    "audioop-lts>=0.2.1",
    "anthropic==0.76.0",
    "infinity-sdk==0.7.0",
    "agentrun-sdk>=0.0.16,<1.0.0",
    "litellm==1.82.5",
    # ... 150+ dependencies
]
 
[dependency-groups]
test = [
    "hypothesis>=6.132.0",
    "pytest>=8.3.5",
    "pytest-asyncio>=1.3.0",
    "pytest-playwright>=0.7.2",
    # ... test-only dependencies
]
```

Key details include:

- Python version locking ensures compatibility with 3.13 currently pyproject.toml8
- Extensive dependencies for diverse functionality, such as AI models, database drivers, and utilities, are specified with exact versions pyproject.toml9-180
- The test dependency group contains tools for mocking and testing to be used only in CI and development pyproject.toml182-199

### Lock File

The `uv.lock` file pins exact versions and hashes of all dependencies and transitive packages, marked with platform-specific constraints for macOS and Linux architectures [uv.lock1-8](https://github.com/infiniflow/ragflow/blob/d32e05d5/uv.lock#L1-L8) This ensures reproducible environments regardless of platform differences.

### uv Installation in Docker

During Docker build, `uv` is installed from prebuilt tarballs embedded in the dependency image to skip repetitive compilation.

Steps for `uv` installation in the Docker base stage [Dockerfile69-83](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L69-L83):

1. Configuration for Python package registry mirrors (Aliyun) when NEED_MIRROR=1 Dockerfile71-77
2. Architecture-detection to select x86_64 or aarch64 uv binary Dockerfile78-79
3. Extraction of pre-downloaded uv from a dependency image Dockerfile80-82
4. Execution of uv python install 3.13 to prepare the Python runtime environment Dockerfile83

This approach accelerates builds and ensures deterministic Python environments.

**Sources:** [pyproject.toml1-199](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L1-L199) [uv.lock1-100](https://github.com/infiniflow/ragflow/blob/d32e05d5/uv.lock#L1-L100) [Dockerfile69-83](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L69-L83)

## Multi-Stage Docker Build

To produce optimized production images that combine Python, Go, and C++ components, RAGFlow uses a multi-stage Dockerfile based on Ubuntu 24.04 [Dockerfile1-2](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L1-L2)

### Build Pipeline Diagram

```
Stage: production

Stage: builder

Stage: base [ubuntu:24.04]

System Libs: libgl1, pkg-config, default-jdk, libnss3, ghostscript, etc.

Node.js 20, uv Python manager installation

ODBC Drivers: msodbcsql17/18

Infinity Resource Git clone to /usr/share/infinity/resource

uv sync --frozen (Install Python deps)

npm run build (Vite frontend)

C++ and Go build via build.sh script

Copy .venv from builder

Copy web/dist from builder

Copy Python source code (rag/, api/, agent/, etc.)

entrypoint.sh for container start
```

This build strategy segments:

- The base stage installs general system dependencies, Python manager (uv), Node.js, and ODBC drivers Dockerfile45-134
- The builder stage runs Python dependency sync (uv sync), builds frontend assets with Node, and compiles native Go and C++ components Dockerfile138-160
- The production stage copies only runtime components to the final minimal image, including virtualenv, compiled web assets, and source code.

### Multi-Language Compilation

RAGFlow integrates native components compiled during the build:

- C++ Tokenizer (RAGAnalyzer): Built inside a privileged Docker container infiniflow/infinity_builder:ubuntu22_clang20 using ./build.sh --cpp, compiling C++ source that provides tokenization and analysis tests.yml132-138
- Go Server: Backend services in Go (including the ingestor and server) are compiled using ./build.sh --go tests.yml139
- Python C Extensions: Dependencies requiring native extensions (e.g., opencv-python, pyclipper) are managed via uv during the builder stage pyproject.toml79-87

**Sources:** [Dockerfile1-160](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L1-L160) [.github/workflows/tests.yml132-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L132-L142) [pyproject.toml79-87](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L79-L87)

## Dependency Pre-caching Script

A helper Python script (`download_deps.py`) exists to pre-download heavy external resources needed by RAGFlow. This permits offline Docker builds and accelerates cache usage.

### Functionality

- Downloads precompiled binaries, model files, and data resources either from official mirrors or China-accessible mirror URLs download_deps.py21-45
- Caches linguistic resources like NLTK corpora (wordnet, punkt, punkt_tab) download_deps.py80-83
- Caches large models from HuggingFace repositories such as InfiniFlow/deepdoc to avoid repeated download download_deps.py48-51
- Downloads Tika server JAR files necessary for document parsing download_deps.py26-27
- Downloads Chrome testing binaries to support Selenium browser automation download_deps.py29-30

### Example URLs and Targets

| Resource | Source | Target Path in Image |
| --- | --- | --- |
| libssl1.1 Ubuntu packages | archive.ubuntu.com or Tsinghua mirrors | Installed in OS [Dockerfile129-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L129-L134) |
| Apache Tika server JAR | Maven central or Huawei Cloud mirror | `/ragflow/tika-server-standard-3.3.0.jar` [Dockerfile22](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L22-L22) |
| HuggingFace Models | HuggingFace repositories | `/ragflow/rag/res/deepdoc` [Dockerfile16](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L16-L16) |
| NLTK corpora | NLTK official download | `/root/nltk_data` [Dockerfile21](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L21-L21) |
| Chromium binaries | Google APIs or npm mirror | `/opt/chrome` [Dockerfile122](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L122-L122) |
| uv binaries | GitHub releases of uv | `/usr/local/bin/uv` [Dockerfile81](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L81-L81) |

The script accepts a `--china-mirrors` flag to select alternate mirrors [download_deps.py62](https://github.com/infiniflow/ragflow/blob/d32e05d5/download_deps.py#L62-L62)

**Sources:** [download_deps.py1-88](https://github.com/infiniflow/ragflow/blob/d32e05d5/download_deps.py#L1-L88) [Dockerfile11-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L11-L134)

## CI/CD Workflow (GitHub Actions)

RAGFlow's continuous integration and deployment pipeline is configured through GitHub Actions workflows, primarily in `.github/workflows/tests.yml` and `.github/workflows/release.yml`.

### Tests Workflow Overview

- Triggered by code pushes, pull requests, and scheduled daily runs tests.yml5-24
- Runs on self-hosted runners tagged ragflow-test tests.yml37
- Includes:
Static lint checking for Python using theRufflintertests.yml94-99Building Go and C++ native components using Docker-in-Docker build containerstests.yml132-142Integration tests against multiple document storage backends: Elasticsearch, Infinity, and OpenSearch.

### CI/CD to Code Entity Mapping Diagram

```
Codebase Entities

GitHub Actions CI Workflow [.github/workflows/tests.yml]

Trigger: push/pull_request/schedule

Static check with Ruff

Go and C++ build via build.sh

Docker image build

Python SDK tests

pyproject.toml tool.ruff config

cmd/server_main.go

build.sh

Dockerfile

sdk/python/pyproject.toml
```

### Automated Release Workflow

The `release.yml` workflow triggers for Git tags matching version patterns (`v*.*.*`) [release.yml10-11](https://github.com/infiniflow/ragflow/blob/d32e05d5/release.yml#L10-L11) It performs:

- Docker image build and push to Docker Hub infiniflow/ragflow release.yml85-91
- Building and publishing the Python SDK (ragflow-sdk) to PyPI using uv build and uv publish release.yml93-96
- Building and publishing the Python CLI (ragflow-cli) to PyPI release.yml98-101

**Sources:** [.github/workflows/tests.yml1-150](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L1-L150) [.github/workflows/release.yml1-102](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/release.yml#L1-L102) [sdk/python/pyproject.toml1-9](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/pyproject.toml#L1-L9)

## Testing Infrastructure

RAGFlow tests use the `pytest` framework, with configuration and priority markers defined for the SDK [sdk/python/pyproject.toml26-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/pyproject.toml#L26-L31)

### Service Environment for Tests

The `docker/docker-compose-base.yml` defines common core services used in CI and development:

- Elasticsearch (es01) docker-compose-base.yml2-38
- Infinity vector store (infinity) docker-compose-base.yml76-101
- OpenSearch (opensearch01) docker-compose-base.yml40-74
- MySQL database (mysql) docker-compose-base.yml186-212
- MinIO object storage (minio) docker-compose-base.yml214-228
- Sandbox executor manager (sandbox-executor-manager) docker-compose-base.yml158-184

### System Component Diagram

```
Code Logic

Containerized Services [docker/docker-compose-base.yml]

es01 (Elasticsearch)

infinity (Vector Store)

mysql (Database)

minio (Object Storage)

sandbox-executor-manager

internal/service/FileService

internal/dao/FileDAO

internal/engine/ (DocStore abstraction)
```

The `FileService` in Go [internal/service/file.go39](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/file.go#L39-L39) interacts with the `FileDAO` [internal/service/file.go40](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/file.go#L40-L40) and storage layers, which are verified during integration tests using the services defined in the compose file.

**Sources:** [docker/docker-compose-base.yml1-230](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L230) [internal/service/file.go39-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/file.go#L39-L50) [sdk/python/pyproject.toml26-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/pyproject.toml#L26-L31)


---



<!-- ===== 2.5-kubernetes-and-helm-deployment ===== -->

# Kubernetes and Helm Deployment

Relevant source files
- .github/workflows/release.yml
- .github/workflows/tests.yml
- Dockerfile
- Dockerfile.deps
- docker/docker-compose-base.yml
- docker/infinity_conf.toml
- download_deps.py
- helm/.helmignore
- helm/Chart.yaml
- helm/README.md
- helm/templates/_helpers.tpl
- helm/templates/elasticsearch-config.yaml
- helm/templates/elasticsearch.yaml
- helm/templates/env.yaml
- helm/templates/infinity.yaml
- helm/templates/ingress.yaml
- helm/templates/minio.yaml
- helm/templates/mysql-config.yaml
- helm/templates/mysql.yaml
- helm/templates/opensearch-config.yaml
- helm/templates/opensearch.yaml
- helm/templates/ragflow.yaml
- helm/templates/ragflow_config.yaml
- helm/templates/redis.yaml
- helm/templates/tests/test-connection.yaml
- helm/values.yaml
- pyproject.toml
- sdk/python/pyproject.toml
- sdk/python/uv.lock
- uv.lock

The RAGFlow Helm chart provides a production-grade Kubernetes deployment solution for the entire RAGFlow system. It encapsulates all key subsystems and dependencies—such as Redis, MySQL, MinIO, Elasticsearch, and Infinity—into Kubernetes-native resources with configuration flexible through the `values.yaml` file. This orchestrated deployment aligns with RAGFlow's multi-tier architecture, streamlining scaling, updates, persistence, and networking in Kubernetes environments.

## Deployment Architecture

The Helm chart abstracts deployment details by packaging each backend component and service into dedicated Kubernetes workloads:

- StatefulSets for stateful storage services (Redis, MySQL, MinIO, Elasticsearch, Infinity).
- A Deployment for the core application (ragflow), including the main Python backend, the task executor, and an optional Admin Service.
- ConfigMaps and Secrets for environment configuration.
- Optional Ingress resources for external traffic routing.

This design ensures stable network identities, persistent storage, and precise resource management fitting a cloud-native architecture.

### Mapping Kubernetes Resources to RAGFlow Components

The following diagram relates key Helm chart templates and Kubernetes manifests to the RAGFlow components and their corresponding runtime artifacts:

**Diagram: Kubernetes to Code Entity Mapping**

```
Code Entity Space

Kubernetes Space

populates

injects into

executes

generates

runs

runs

provides

provides

provides

provides

provides

routes traffic

overrides

values.yaml

env.yaml (Secret)

ragflow.yaml (Deployment)

infinity.yaml (StatefulSet)

redis.yaml (StatefulSet)

mysql.yaml (StatefulSet)

minio.yaml (StatefulSet)

elasticsearch.yaml (StatefulSet)

ingress.yaml (Ingress)

service_conf.yaml

.env / Environment Variables

ragflow_server.py

task_executor.py

MySQL

Infinity

Valkey Redis

MinIO Object Storage

Elasticsearch

entrypoint.sh
```

*This diagram illustrates how configuration values flow into environment secrets, which then influence the deployment manifests. The main RAGFlow deployment runs the application entrypoint script, which dynamically generates service configuration and runs core application processes.*

**Sources:** [helm/values.yaml1-123](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L1-L123) [helm/templates/redis.yaml1-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml#L1-L135) [helm/values.yaml13-70](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L13-L70)

## Helm Chart Structure

The Helm chart follows standard conventions with organized templates for each service and component. Key template files and their responsibilities include:

| Component | Kubernetes Resource | Purpose & Notes |
| --- | --- | --- |
| **RAGFlow** | `Deployment` | Main backend service running Python Quart application and task executor. Supports Admin Service on port 9381 [helm/values.yaml112-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L112-L118) |
| **Redis** | `StatefulSet` | Cache/queue backend using `valkey/valkey:8` [helm/values.yaml228-229](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L228-L229) Configured with `allkeys-lru` policy [helm/templates/redis.yaml63](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml#L63-L63) |
| **MySQL** | `StatefulSet` | Relational DB for metadata. Uses `mysql:8.0.39` [helm/values.yaml212-213](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L212-L213) |
| **MinIO** | `StatefulSet` | S3-compatible object store. Uses `quay.io/minio/minio` [helm/values.yaml196-197](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L196-L197) |
| **Elasticsearch** | `StatefulSet` | Document search engine. Uses version `8.11.3` [helm/values.yaml141-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L141-L142) |
| **Infinity** | `StatefulSet` | Default AI-native doc engine for vector search. Uses `infiniflow/infinity:v0.7.0` [helm/values.yaml126-127](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L126-L127) |

**Sources:** [helm/values.yaml124-247](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L124-L247) [helm/templates/redis.yaml22-104](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml#L22-L104) [helm/values.yaml77-123](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L77-L123)

## Data Flow and Service Interaction

Within Kubernetes, the services interact to facilitate the RAG pipeline:

- Application Layer: ragflow pods expose HTTP APIs and background executors.
- Task Queue: Redis (specifically Valkey) manages task distribution via Streams.
- Persistence: MySQL stores structured metadata; MinIO stores raw files.
- Search Engine: Depending on DOC_ENGINE helm/values.yaml19-21 either Infinity, Elasticsearch, or OpenSearch handles vector/full-text retrieval.

**Diagram: Service Interconnectivity**

```
Persistence Layer

Application Layer

Frontend Access

Redis Streams

Vector Search

Metadata

Files

Task Queue

Ingress

ragflow-svc (ClusterIP)

ragflow-pod (Deployment)

redis-pod (StatefulSet)

infinity-pod (StatefulSet)

mysql-pod (StatefulSet)

minio-pod (StatefulSet)
```

**Sources:** [helm/values.yaml13-21](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L13-L21) [docker/docker-compose-base.yml186-228](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L186-L228) [helm/templates/redis.yaml106-121](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml#L106-L121)

## Configuration and Customization

### `values.yaml` Customization

The `values.yaml` file is the primary interface for deployment configuration:

- Engine Selection: Toggle between search backends using env.DOC_ENGINE (options: infinity, elasticsearch, opensearch) helm/values.yaml14-21
- Batch Processing: Configure DOC_BULK_SIZE (default 4) and EMBEDDING_BATCH_SIZE (default 16) for performance tuning helm/values.yaml71-75
- Resource Management: Each component has resources blocks for CPU/Memory requests/limits helm/values.yaml159-162
- Persistence: Redis persistence can be toggled with retentionPolicy (available in K8s 1.32+) to control PVC behavior during scaling or deletion helm/values.yaml235-241

### Image Management

Images are managed via a global repository prefix if needed [helm/values.yaml4-11](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L4-L11) The default RAGFlow image is `infiniflow/ragflow:v0.26.0` [helm/values.yaml79-80](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L79-L80)

**Sources:** [helm/values.yaml1-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L1-L82) [helm/templates/redis.yaml79-88](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml#L79-L88)

## Infrastructure as Code Details

### Redis StatefulSet Implementation

The Redis template [helm/templates/redis.yaml](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml) utilizes a `StatefulSet` with a headless service [helm/templates/redis.yaml17](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml#L17-L17) to ensure stable network identities. It enforces password authentication using the `${REDIS_PASSWORD}` environment variable [helm/templates/redis.yaml63](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml#L63-L63)

### Document Engine Resource Profiles

Elasticsearch and OpenSearch templates default to high-performance profiles, requesting 4 CPUs and 16Gi of memory [helm/values.yaml160-189](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L160-L189) In contrast, Infinity is optimized for a smaller footprint but high vector throughput [helm/values.yaml135](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L135-L135)

### Build System Integration

The Kubernetes deployment relies on images built via the project's `Dockerfile`. This multi-stage build [Dockerfile1-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L1-L152) incorporates:

1. Dependencies: Downloaded via download_deps.py, including NLTK data and HuggingFace models (InfiniFlow/deepdoc) Dockerfile11-23
2. Environment: Managed by uv for Python 3.13 Dockerfile69-83
3. Native Components: C++ RAGAnalyzer and Go-based ingestors/servers pyproject.toml1-180

## Bridging Natural Language to Code Entities

**Diagram: Configuration Lifecycle**

```
Container Runtime

Helm Configuration

injects

overrides

overrides

configures

values.yaml

ragflow.service_conf

llm_factories

entrypoint.sh

local.service_conf.yaml

llm_factories.json

uv Python 3.13 Environment
```

**Sources:** [helm/values.yaml87-92](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L87-L92) [Dockerfile83](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L83-L83) [pyproject.toml8](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L8-L8)

# Summary

The RAGFlow Helm deployment provides a robust framework for running the RAG engine at scale. By leveraging `StatefulSets` for storage and a flexible `Deployment` for application logic, it ensures that high-level system requirements (like vector search and document parsing) are met by specific code entities (`ragflow_server.py`, `task_executor.py`) running in a correctly configured Kubernetes environment.

**Sources:** [helm/values.yaml1-266](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/values.yaml#L1-L266) [helm/templates/redis.yaml1-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/helm/templates/redis.yaml#L1-L135) [Dockerfile1-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/Dockerfile#L1-L152)


---



<!-- ===== 3-system-architecture ===== -->

# System Architecture

Relevant source files
- admin/server/admin_server.py
- admin/server/auth.py
- admin/server/config.py
- admin/server/routes.py
- admin/server/services.py
- agent/component/list_operations.py
- api/db/__init__.py
- api/db/db_models.py
- api/db/joint_services/user_account_service.py
- api/db/services/dialog_service.py
- api/db/services/doc_metadata_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/system_settings_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/ragflow_server.py
- api/utils/configs.py
- api/utils/file_utils.py
- api/utils/health_utils.py
- conf/system_settings.json
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- internal/admin/service_variables_test.go
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/task_executor.py
- test/testcases/test_web_api/test_canvas_app/test_list_operations_unit.py
- web/src/pages/agent/canvas/node/list-operations-node.tsx
- web/src/pages/agent/form/list-operations-form/index.tsx

## Purpose and Scope

This document provides an in-depth overview of RAGFlow's layered architecture, describing how the API gateway, service layer, worker layer, and storage layer interact to deliver document-based conversational AI capabilities. For specific implementation details:

- Core application services and request routing: see Core Application Services
- Storage layer design and database selection: see Data Storage Architecture
- Asynchronous task processing: see Task Execution and Queue System
- Agent and tool component loading: see Component Dynamic Loading
- Go-based native service layer: see Go Server and Native Components

For frontend architecture, see [Frontend Application](/infiniflow/ragflow/4-frontend-application). For LLM integration patterns, see [LLM Integration System](/infiniflow/ragflow/5-llm-integration-system). For document processing pipelines, see [Document Processing Pipeline](/infiniflow/ragflow/6-document-processing-pipeline).

## Overview

RAGFlow employs a **five-layer microservices architecture** that separates concerns across client interaction, API routing, business logic, asynchronous processing, and data persistence. The system is designed for horizontal scalability with stateless API servers and distributed worker processes communicating through Redis-backed message queues.

The architecture follows these principles:

- Separation of concerns: API servers handle synchronous requests; workers run long-lived asynchronous tasks such as document parsing, embedding generation, and knowledge graph construction managed via Redis Streams rag/svr/task_executor.py133-139
- Pluggable components: Document stores (Elasticsearch, Infinity, OceanBase, or OpenSearch), parsers (e.g., naive, paper, book), and LLM providers are swappable via configuration rag/svr/task_executor.py114-131
- Multi-tenancy: All data is scoped by tenant_id with row-level security enforced in the ORM layer api/db/services/knowledgebase_service.py52-68
- Extensible service layer: Business logic lives within service classes encapsulating database and backend logic api/db/services/document_service.py41-72
- Native complement: A dedicated Go server and C++ components provide optimized backend services layered alongside the Python backend rag/nlp/search.py30-34

**Sources:**
 [api/db/db_models.py152-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L152-L185) [rag/svr/task_executor.py114-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L114-L140) [api/db/services/document_service.py41-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L41-L72) [api/ragflow_server.py17-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L17-L159) [api/db/services/knowledgebase_service.py52-68](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L52-L68)

## Five-Layer Architecture

### System Component Diagram

This diagram illustrates RAGFlow's modular subsystems, their interactions, and code entity mapping between the natural language system description and source code modules:

```
Layer 5: Storage Layer

Layer 4: Worker Layer

Layer 3: Service Layer

Layer 2: API Gateway

Layer 1: Client Layer

HTTP

HTTP

HTTP

Enqueues Tasks

Consumes Tasks

Web UI
web/src/

Python SDK
api/apps/sdk/

Direct HTTP API
/api/v1/*

Quart API Server
api/ragflow_server.py

Go API Server
cmd/server_main.go

@login_required
api/apps/init.py

UserService
api/db/services/user_service.py

DocumentService
api/db/services/document_service.py

KnowledgebaseService
api/db/services/knowledgebase_service.py

LLMBundle
api/db/services/llm_service.py

DialogService
api/db/services/dialog_service.py

Task Executor
rag/svr/task_executor.py

Redis Streams Queue
rag/utils/redis_conn.py

MySQL/PostgreSQL
api/db/db_models.py

Document Store (ES/Infinity/OceanBase)
rag/nlp/search.py

Object Storage (MinIO, S3)
api/db/services/document_service.py

Redis Cache
rag/utils/redis_conn.py
```

**Sources:**
 [rag/svr/task_executor.py104](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L104-L104) [api/db/services/document_service.py41-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L41-L72) [api/db/db_models.py152-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L152-L185) [rag/nlp/search.py38-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L38-L41) [api/ragflow_server.py17-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L17-L159)

## Core Execution Flows

### Document Upload and Asynchronous Task Processing

A user uploads a document via the API server, which validates and stores metadata via the service layer [api/db/services/document_service.py114-122](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L114-L122) A corresponding processing task is enqueued into the Redis Streams queue (`SVR_CONSUMER_GROUP_NAME`) to be picked up by background workers [api/db/services/document_service.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L32-L32)

Workers in the worker layer consume tasks from the Redis queue, invoke a parser selected from a factory based on the document's configured parser type [rag/svr/task_executor.py114-131](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L114-L131) and update task progress in the database [rag/svr/task_executor.py165-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L185)

Tasks include document chunking, embedding computation, indexing to the configured document store, and optional knowledge graph processing [rag/svr/task_executor.py133-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L133-L139)

```
DocStore
"task_executor.py (Parser Factory)"
"Redis Streams (SVR_CONSUMER_GROUP_NAME)"
"TaskService"
"DocumentService"
"ragflow_server.py (Quart App)"
Client
DocStore
"task_executor.py (Parser Factory)"
"Redis Streams (SVR_CONSUMER_GROUP_NAME)"
"TaskService"
"DocumentService"
"ragflow_server.py (Quart App)"
Client
loop
[Background Processing by
Worker]
POST /v1/document/upload
check_doc_health()
create_processing_task()
XADD task
XREADGROUP consume task
FACTORY[parser_id].chunk()
set_progress()
index chunks & embeddings
```

**Sources:**
 [api/db/services/document_service.py114-122](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L114-L122) [rag/svr/task_executor.py115-132](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L115-L132) [rag/svr/task_executor.py165-197](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L197) [api/db/services/task_service.py58-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L58-L143)

## Core Components by Layer

### Layer 1: Client Layer

Provides external interfaces for users and programmatic clients.

| Component | Technology | Code Location | Responsibility |
| --- | --- | --- | --- |
| Web UI | React | `web/src/` | Web-based chat interface and management console |
| Python SDK | Python | `api/apps/sdk/` | Programmatic access to API endpoints |
| Direct API | REST/HTTP | `api/utils/api_utils.py` | Utility methods for API request handling and validation |

**Sources:**
 [api/utils/api_utils.py29-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L29-L30)

### Layer 2: API Gateway

Manages synchronous API requests with multiple server implementations:

- Python Quart Server: Primary server implementing most business logic, API routing, and agent orchestration. Entry point is api/ragflow_server.py api/ragflow_server.py35-169
- Go Server: High-performance native server layer, optimized for search, tokenizer, and internal service operations (cmd/server_main.go).
- Auth Middleware: Decorator-based authentication using @login_required in api/apps/__init__.py.

Periodic background threads within the Python server update document processing progress using distributed locks in Redis [api/ragflow_server.py53-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L53-L69)

**Sources:**
 [api/ragflow_server.py17-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L17-L159) [rag/nlp/search.py34-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L34-L41)

### Layer 3: Service Layer

Encapsulates business logic, ORM models, and database transactions:

- DocumentService: Manages document metadata, status, and progress tracking, including health checks before processing api/db/services/document_service.py41-110
- KnowledgebaseService: Handles multi-tenant dataset (knowledge base) management and access control api/db/services/knowledgebase_service.py32-152
- LLMBundle: Abstracts interactions with various large language model providers api/db/services/llm_service.py
- DialogService: Manages conversation state, RAG retrieval parameters, and optional Internet search integration api/db/services/dialog_service.py138-176

Implementation leverages the `peewee` ORM with model definitions in `api/db/db_models.py` [api/db/db_models.py152-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L152-L185)

**Sources:**
 [api/db/services/document_service.py41-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L41-L110) [api/db/services/knowledgebase_service.py32-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L32-L152) [api/db/services/dialog_service.py138-176](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L138-L176) [api/db/db_models.py152-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L152-L185)

### Layer 4: Worker Layer

Background worker processes fetch and consume tasks asynchronously from Redis Streams (`SVR_CONSUMER_GROUP_NAME`). The central worker code is in `task_executor.py` [rag/svr/task_executor.py103](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L103-L103)

- Parser components are dynamically selected from a FACTORY based on parser type (e.g., naive, paper, book) rag/svr/task_executor.py114-131
- Supports multiple pipeline task types such as dataflow, raptor, graphrag, mindmap, and memory rag/svr/task_executor.py133-139
- The worker keeps track of task progress and handles cancellation and retries rag/svr/task_executor.py165-197
- Workers interact with the document store and object storage to save chunk embeddings and raw blobs respectively rag/nlp/search.py38-41

**Sources:**
 [rag/svr/task_executor.py115-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L115-L140) [rag/svr/task_executor_refactor/task_manager.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor_refactor/task_manager.py) [rag/utils/redis_conn.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/redis_conn.py)

### Layer 5: Storage Layer

RAGFlow employs a polyglot persistence strategy with the following primary storage backends:

| Storage Type | Example Engines | Role and Usage | Code Reference |
| --- | --- | --- | --- |
| **Relational DB** | MySQL, PostgreSQL | User data, document metadata, task progress tracking | `api/db/db_models.py:152-185]()` |
| **Document Store** | Elasticsearch, Infinity, OceanBase | Vector embeddings, text chunk indexes | `rag/nlp/search.py:38-41]()` |
| **Object Storage** | MinIO, AWS S3 | Raw document files, thumbnails, images | `api/db/services/document_service.py:48]()` |
| **Cache and Queue** | Redis | Task queues, distributed locks, caching session states | `rag/utils/redis_conn.py:91]()` |

This polyglot approach allows RAGFlow to optimize storage for specific data types: relational metadata in MySQL/PostgreSQL, large vector data in scalable document stores, and raw binary assets in object storage.

**Sources:**
 [api/db/db_models.py152-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L152-L185) [rag/nlp/search.py38-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L38-L41) [api/db/services/document_service.py48-123](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L48-L123) [rag/utils/redis_conn.py22-145](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/redis_conn.py#L22-L145)

## Bridging Natural Language Concepts to Code Entities

### Diagram: Natural Language Space to Code Entity Space for Document Processing Flow

```
Document Upload

Background Task Processing

Storage Layer

ragflow_server.py

DocumentService (document_service.py)

TaskService (task_service.py)

task_executor.py

REDIS_CONN (redis_conn.py)

Document Store Interface (e.g. ES/Inifnity)

MinIO Object Storage
```

### Diagram: Mapping API Gateway and Service Layer Components to Code

```
API Server (request routing, auth)

Service Layer (business logic)

Quart API Server (ragflow_server.py)

Go Server (cmd/server_main.go)

@login_required (api/apps/init.py)

UserService (user_service.py)

KnowledgebaseService (knowledgebase_service.py)

LLMBundle (llm_service.py)

DialogService (dialog_service.py)
```

## Summary

RAGFlow's system architecture provides a modular, scalable platform to deliver retrieval-augmented generation with multi-tenant document ingestion, asynchronous task processing, and pluggable storage engines. The architecture is carefully layered to maintain separation of concerns, enable easy component swapping, and optimize performance through a hybrid Python-Go backend.

For detailed exploration of each subsystem, please consult the linked child pages:

- Core Application Services
- Data Storage Architecture
- Task Execution and Queue System
- Component Dynamic Loading
- Go Server and Native Components

**Sources:**
 [rag/svr/task_executor.py1-200](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L1-L200) [api/db/services/document_service.py41-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L41-L110) [api/db/db_models.py152-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L152-L185) [rag/nlp/search.py1-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L1-L140) [api/ragflow_server.py1-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L1-L159) [api/db/services/dialog_service.py130-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L130-L180)


---



<!-- ===== 3.1-core-application-services ===== -->

# Core Application Services

Relevant source files
- admin/server/admin_server.py
- admin/server/auth.py
- admin/server/config.py
- admin/server/routes.py
- admin/server/services.py
- agent/component/list_operations.py
- api/apps/__init__.py
- api/db/joint_services/user_account_service.py
- api/db/services/doc_metadata_service.py
- api/db/services/system_settings_service.py
- api/ragflow_server.py
- api/settings.py
- api/utils/api_utils.py
- api/utils/configs.py
- api/utils/health_utils.py
- conf/service_conf.yaml
- conf/system_settings.json
- docker/service_conf.yaml.template
- internal/admin/service_variables_test.go
- test/testcases/test_web_api/test_canvas_app/test_list_operations_unit.py
- test/testcases/test_web_api/test_system_app/test_apps_init_unit.py
- web/src/components/document-preview/md/index.tsx
- web/src/constants/markdown-remark-plugins.ts
- web/src/pages/agent/canvas/node/list-operations-node.tsx
- web/src/pages/agent/form/list-operations-form/index.tsx

## Purpose and Scope

This document details the core application services within RAGFlow responsible for implementing business logic, managing API requests, and initializing the main application server. It covers the API server initialization via `api/ragflow_server.py`, the setup and configuration of the Quart application in `api/apps/__init__.py`, and the administrative backend in `admin/server/admin_server.py`. It also explains the service layer pattern, data flow, and security mechanisms.

For complementary information on persistence or task execution, see [Data Storage Architecture (3.2)](/infiniflow/ragflow/3.2-data-storage-architecture) and [Task Execution and Queue System (3.3)](/infiniflow/ragflow/3.3-task-execution-and-queue-system).

## API Server Initialization

RAGFlow's backend server starts in `api/ragflow_server.py`, which orchestrates environment setup, configuration loading, database initialization, and launches the Quart web server.

### Server Startup Workflow

1. Early Bootstrapping: Sets LITELLM_LOCAL_MODEL_COST_MAP to avoid blocking startup on external network calls api/ragflow_server.py26 It initializes root logging and a fault handler api/ragflow_server.py79-80
2. Configuration: Loads global settings and prints the configuration for audit api/ragflow_server.py96-97
3. Database Initialization: Calls init_web_db() to create Peewee tables and init_web_data() to seed system data api/ragflow_server.py105-106
4. Plugin Loading: Invokes GlobalPluginManager.load_plugins() to register extensible components api/ragflow_server.py134
5. Background Coordination: Launches a thread update_progress() using a RedisDistributedLock to periodically refresh document parsing status in the database api/ragflow_server.py53-69
6. Server Run: Starts the Quart application (api/apps/__init__.py:app) on the configured host and port api/ragflow_server.py169

### Startup Workflow Diagram

```
Start: api/ragflow_server.py

Set Env LITELLM_LOCAL_MODEL_COST_MAP

Initialize fault handler & root logger

Load settings & show_configs()

init_web_db() & init_web_data()

GlobalPluginManager.load_plugins()

Start update_progress() thread (Redis Lock)

Run Quart App: api/apps/init.py:app
```

**Sources:** [api/ragflow_server.py17-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L17-L175) [api/apps/__init__.py61-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L61-L85)

## Quart Application Setup

The main HTTP API is built on the Quart ASGI framework, providing asynchronous support for long-running LLM tasks.

- Timeouts: RESPONSE_TIMEOUT and BODY_TIMEOUT are set to 600 seconds (default 60s) to accommodate slow LLM responses api/apps/__init__.py73-74
- Security: Uses quart_cors for cross-origin support and CustomJSONEncoder for serializing complex objects like ORM models api/apps/__init__.py62-68
- Session: Configured to use Redis as the backend to enable shared state across distributed instances api/apps/__init__.py79-80
- Error Handling: A global handler server_error_response standardizes error formats for the frontend api/apps/__init__.py69

**Sources:** [api/apps/__init__.py61-86](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L61-L86) [api/utils/api_utils.py133-151](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L133-L151)

## Service Layer Architecture

The backend utilizes a service layer pattern to separate business logic from the data access layer (Peewee ORM).

### Core Service Classes

| Service Class | File Path | Responsibility |
| --- | --- | --- |
| `UserService` | [api/db/services/user_service.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/user_service.py) | Authentication, tenant mapping, and user profiles [api/apps/__init__.py27](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L27-L27) |
| `DocumentService` | [api/db/services/document_service.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py) | Document metadata and parsing progress [api/ragflow_server.py37](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L37-L37) |
| `DocMetadataService` | [api/db/services/doc_metadata_service.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/doc_metadata_service.py) | Sole source of truth for document metadata in ES/Infinity [api/db/services/doc_metadata_service.py16-21](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/doc_metadata_service.py#L16-L21) |
| `LLMFactoriesService` | [api/db/services/tenant_llm_service.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/tenant_llm_service.py) | Managing LLM provider configurations [api/utils/api_utils.py46](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L46-L46) |
| `UserMgr` | [admin/server/services.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/services.py) | Admin-specific user management and API key lifecycle [admin/server/services.py40](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/services.py#L40-L40) |

### Data Flow Diagram

```
Data Store Layer

Service Layer

Controller Layer

api/apps/init.py:app (Quart)

admin/server/routes.py:admin_bp (Flask)

UserService

DocumentService

DocMetadataService

UserMgr

MySQL/Postgres (Peewee)

Doc Engine (ES/Infinity)
```

**Sources:** [api/db/services/doc_metadata_service.py66-132](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/doc_metadata_service.py#L66-L132) [admin/server/services.py40-108](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/services.py#L40-L108) [api/apps/__init__.py112-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L112-L143)

## Request Handling and Security

### Authentication Flow

The system supports multiple authentication types: `JWT`, `API`, and `BETA` [api/apps/__init__.py96-98](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L96-L98)

1. Header Check: _load_user checks for the Authorization header api/apps/__init__.py153
2. JWT Validation: If present, the token is decoded using Serializer and checked against the access_token in the database api/apps/__init__.py187-200
3. Session Fallback: If no header exists, _load_user_from_session attempts to resolve the user via the _user_id in the session cookie api/apps/__init__.py112-143

### Request Validation

The `validate_request` decorator ensures incoming payloads contain required arguments and valid values before the controller executes [api/utils/api_utils.py154-177](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L154-L177)

**Sources:** [api/apps/__init__.py112-200](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L112-L200) [api/utils/api_utils.py60-177](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L60-L177)

## Administration Service

The admin backend (`admin/server/admin_server.py`) provides specialized management APIs separate from the main RAG server.

- Framework: Uses Flask with flask_login for session-based admin authentication admin/server/admin_server.py26-27
- Default Admin: init_default_admin ensures a superuser (admin@ragflow.io) exists upon first boot admin/server/auth.py90-105
- Routes: The admin_bp blueprint defines endpoints for user creation, password updates, and system health checks admin/server/routes.py35-150
- Health Monitoring: health_utils.py provides probes for MySQL, Redis, and Document Engines (ES/Infinity/OceanBase) api/utils/health_utils.py34-70

**Sources:** [admin/server/admin_server.py41-83](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/admin_server.py#L41-L83) [admin/server/auth.py39-87](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/auth.py#L39-L87) [api/utils/health_utils.py34-133](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/health_utils.py#L34-L133)

## Code Entity Association

### Mapping Natural Language to Code Entities

This diagram bridges the gap between high-level system concepts and specific code identifiers.

```
Code Entities

System Concepts

API Entry Point

App Config

User Logic

Doc Metadata

Admin Routes

api/ragflow_server.py

api/apps/init.py:app

api/db/services/user_service.py:UserService

api/db/services/doc_metadata_service.py:DocMetadataService

admin/server/routes.py:admin_bp
```

**Sources:** [api/ragflow_server.py35](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L35-L35) [api/db/services/doc_metadata_service.py66](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/doc_metadata_service.py#L66-L66) [admin/server/routes.py35](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/routes.py#L35-L35)

### Implementation of Core Utilities

The following diagram maps common tasks to their respective utility functions.

```
Code Utilities

Functionality

JSON Parsing

Request Validation

Health Check

Admin Auth

api/utils/api_utils.py:get_request_json

api/utils/api_utils.py:validate_request

api/utils/health_utils.py:check_db

admin/server/auth.py:setup_auth
```

**Sources:** [api/utils/api_utils.py90-154](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L90-L154) [api/utils/health_utils.py34](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/health_utils.py#L34-L34) [admin/server/auth.py39](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/auth.py#L39-L39)


---



<!-- ===== 3.2-data-storage-architecture ===== -->

# Data Storage Architecture

Relevant source files
- common/asyncio_utils.py
- common/doc_store/doc_store_base.py
- common/doc_store/es_conn_base.py
- common/doc_store/infinity_conn_base.py
- common/doc_store/infinity_conn_pool.py
- common/settings.py
- conf/infinity_mapping.json
- rag/graphrag/general/smoke.py
- rag/graphrag/light/smoke.py
- rag/graphrag/search.py
- rag/graphrag/utils.py
- rag/svr/cache_file_svr.py
- rag/utils/__init__.py
- rag/utils/azure_sas_conn.py
- rag/utils/azure_spn_conn.py
- rag/utils/es_conn.py
- rag/utils/infinity_conn.py
- rag/utils/minio_conn.py
- rag/utils/ob_conn.py
- rag/utils/opensearch_conn.py
- rag/utils/oss_conn.py
- rag/utils/redis_conn.py
- rag/utils/s3_conn.py
- test/unit_test/rag/graphrag/test_graphrag_utils.py
- test/unit_test/rag/utils/test_opensearch_doc_meta.py
- test/unit_test/rag/utils/test_opensearch_hybrid_search.py

## Purpose and Scope

This document details the polyglot persistence strategy employed by RAGFlow. The system integrates multiple storage technologies—MySQL/PostgreSQL as relational databases, document stores (Elasticsearch, Infinity, OceanBase, or OpenSearch), object storage (MinIO/S3/OSS/GCS/Azure Blob), and Redis for caching and coordination. It covers the implementation details, data flow, key classes and functions, and how these components interoperate to support retrieval-augmented generation (RAG) workflows.

## Storage Layer Overview

RAGFlow's storage architecture is designed as a multi-tier system that uses different storage engines optimized for specific tasks:

- Relational Databases (MySQL/PostgreSQL): Store structured metadata and user/tenant data with ACID guarantees.
- Document Stores (Elasticsearch/Infinity/OceanBase/OpenSearch): Provide vector-based semantic search and full-text search (FTS) within indexed knowledge chunks.
- Object Storage (MinIO/S3/OSS/Azure/GCS): Efficiently handle large binary blobs such as uploaded documents and extracted assets.
- Redis/Valkey: Acts as a message broker (Streams) and cache for task coordination and transient data.

The layered design allows modular deployment and scalability. Each storage type is abstracted behind interface classes to allow switching implementations via configuration.

### System Hierarchy and Code Entity Integration

The following diagram maps high-level storage concepts to specific Python classes and system variables.

```
Physical Storage Engines

Storage Abstraction (common/settings.py)

Application Layer

Object Storage (STORAGE_IMPL)

Document Store (DOC_ENGINE)

Task Executor
rag/svr/task_executor.py

DocumentService
api/db/services/document_service.py

docStoreConn Singleton
common/settings.py

STORAGE_IMPL Singleton
common/settings.py

REDIS_CONN Singleton
rag/utils/redis_conn.py

ESConnection
rag/utils/es_conn.py

InfinityConnection
rag/utils/infinity_conn.py

OBConnection
rag/utils/ob_conn.py

OSConnection
rag/utils/opensearch_conn.py

MySQL/PostgreSQL (Metadata)

RAGFlowMinio
rag/utils/minio_conn.py

RAGFlowS3
rag/utils/s3_conn.py

RAGFlowOSS
rag/utils/oss_conn.py

AzureBlob (SPN/SAS)
rag/utils/azure_spn_conn.py

RAGFlowGCS
rag/utils/gcs_conn.py

OpenDALStorage
rag/utils/opendal_conn.py

Redis/Valkey (Streams & Cache)
```

**Sources:** [common/settings.py73-95](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L73-L95) [common/settings.py117-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L117-L134) [common/settings.py198-212](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L198-L212) [rag/utils/redis_conn.py61-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/redis_conn.py#L61-L112)

## Document Store Layer

### Overview

The document store forms the core retrieval backend, supporting hybrid semantic and keyword search over knowledge chunks. It indexes vector embeddings and textual metadata with specialized analyzers.

### Supported Engines and Key Implementations

| Engine | Key Class / File | Special Features |
| --- | --- | --- |
| **Infinity** | `InfinityConnection` [rag/utils/infinity_conn.py30](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L30-L30) | Table-per-KB separation, `rag-coarse` and `rag-fine` analyzers, exponential backoff on meta contention [common/doc_store/infinity_conn_base.py106-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/infinity_conn_base.py#L106-L146) |
| **Elasticsearch** | `ESConnection` [rag/utils/es_conn.py62](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L62-L62) | `search_after` pagination [rag/utils/es_conn.py75-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L75-L139) atomic Pagerank adjustments via scripts [rag/utils/es_conn.py36-58](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L36-L58) |
| **OceanBase** | `OBConnection` [rag/utils/ob_conn.py34](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L34-L34) | Vector search via `pyobvector`, SQLAlchemy integration, specialized FTS column weights [rag/utils/ob_conn.py119-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L119-L134) |
| **OpenSearch** | `OSConnection` [rag/utils/opensearch_conn.py65](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L65-L65) | Hybrid search pipeline initialization (BM25 + KNN) [rag/utils/opensearch_conn.py108-158](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L108-L158) |

### Metadata & Schema

Schema definitions for the document store are maintained in JSON mapping files. For example, `conf/infinity_mapping.json` defines fields like:

- kb_id: Secondary index with low cardinality conf/infinity_mapping.json4
- content: Tokenized using rag-coarse and rag-fine conf/infinity_mapping.json16
- pagerank_fea: Integer field for rank feature boosting conf/infinity_mapping.json29

**Sources:** [conf/infinity_mapping.json1-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/infinity_mapping.json#L1-L44) [rag/utils/infinity_conn.py62-88](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L62-L88) [rag/utils/ob_conn.py54-102](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L54-L102) [common/doc_store/infinity_conn_base.py148-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/infinity_conn_base.py#L148-L180)

## Object Storage Layer

RAGFlow abstracts object storage to support diverse cloud and on-premise environments via a `StorageFactory` [common/settings.py198-212](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L198-L212)

### Key Implementations

- MinIO (`RAGFlowMinio`): Supports single-bucket mode via decorators use_default_bucket and use_prefix_path rag/utils/minio_conn.py52-90 It includes health checks that verify connectivity via list_buckets or existence checks rag/utils/minio_conn.py120-142
- AWS S3 (`RAGFlowS3`): Uses boto3 and supports custom endpoint_url, region_name, and signature_version rag/utils/s3_conn.py71-96
- Azure Blob (`RAGFlowAzureSpnBlob` / `RAGFlowAzureSasBlob`): Supports Service Principal (SPN) and Shared Access Signature (SAS) authentication rag/utils/azure_spn_conn.py34-57
- OpenDAL (`OpenDALStorage`): Provides a generic interface that can even use a MySQL table (LONGBLOB) as a storage backend if configured rag/utils/opendal_conn.py10-137

**Sources:** [rag/utils/minio_conn.py42-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/minio_conn.py#L42-L142) [rag/utils/s3_conn.py28-103](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/s3_conn.py#L28-L103) [rag/utils/azure_spn_conn.py34-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/azure_spn_conn.py#L34-L60) [common/settings.py133-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L133-L134)

## Redis Coordination and Caching Layer

Redis (or Valkey) is essential for task management and system-wide state synchronization.

- Task Queues: RAGFlow uses Redis Streams. Queue names are generated based on priority and task type, such as te.1.common for high priority common/settings.py136-154
- Atomic Operations: The RedisDB class registers Lua scripts for atomic token bucket rate limiting and conditional deletion rag/utils/redis_conn.py64-112
- Persistence: Redis stores the system's SECRET_KEY common/settings.py179-196

**Sources:** [rag/utils/redis_conn.py61-145](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/redis_conn.py#L61-L145) [common/settings.py136-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L136-L159)

## Data Flow: Natural Language Space to Code Entities

The following diagram illustrates how a user request for a document flows through the storage abstractions to the concrete storage engines.

```
Storage Engines

Code Entity Space

Natural Language Space

'Search annual report'

'upload.pdf'

rag.nlp.search.index

rag/svr/task_executor.py

rag/utils/minio_conn.py
RAGFlowMinio.put

rag/utils/infinity_conn.py
InfinityConnection.search

Infinity DB

MinIO Object Store

MySQL Metadata
```

**Sources:** [common/settings.py42](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L42-L42) [rag/utils/minio_conn.py146-157](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/minio_conn.py#L146-L157) [rag/utils/infinity_conn.py94-107](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L94-L107) [common/settings.py73-74](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L73-L74)

## Summary of Storage Configuration

The persistence strategy is governed by `common/settings.py`, which initializes the singletons used throughout the application.

| Variable | Config Source | Default / Example |
| --- | --- | --- |
| `DATABASE_TYPE` | `DB_TYPE` Env | `mysql` [common/settings.py73](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L73-L73) |
| `DOC_ENGINE` | `DOC_ENGINE` Env | `elasticsearch` [common/settings.py85](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L85-L85) |
| `STORAGE_IMPL_TYPE` | `STORAGE_IMPL` Env | `MINIO` [common/settings.py133](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L133-L133) |
| `REDIS` | `service_conf.yaml` | `host: 127.0.0.1:6379` [rag/utils/redis_conn.py27-34](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/redis_conn.py#L27-L34) |

**Sources:** [common/settings.py73-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L73-L134) [rag/utils/redis_conn.py27-34](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/redis_conn.py#L27-L34)


---



<!-- ===== 3.3-task-execution-and-queue-system ===== -->

# Task Execution and Queue System

Relevant source files
- api/db/__init__.py
- api/db/db_models.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- common/metadata_utils.py
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/task_executor.py
- rag/svr/task_executor_refactor/chunk_post_processor.py
- rag/svr/task_executor_refactor/chunk_service.py
- rag/svr/task_executor_refactor/dataflow_service.py
- rag/svr/task_executor_refactor/embedding_service.py
- rag/svr/task_executor_refactor/raptor_service.py
- rag/svr/task_executor_refactor/task_context.py
- rag/svr/task_executor_refactor/task_handler.py
- test/unit_test/common/test_metadata_filter_operators.py
- test/unit_test/rag/svr/task_executor_refactor/conftest.py
- test/unit_test/rag/svr/task_executor_refactor/test_chunk_builder.py
- test/unit_test/rag/svr/task_executor_refactor/test_chunk_post_processor.py

## Purpose and Scope

This document details RAGFlow's background task processing architecture. The system offloads computationally intensive operations—such as document parsing, embedding, and Knowledge Graph construction—to asynchronous workers. It utilizes Redis Streams for reliable message delivery and a task lifecycle management system to handle progress tracking, cancellation, and retries. The system includes a refactored task executor architecture in `rag/svr/task_executor_refactor/` and a Go-based ingestion worker for high-performance processing.

Sources: [rag/svr/task_executor.py1-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L1-L40) [api/db/services/task_service.py58-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L58-L72)

## Architecture Overview

RAGFlow implements a producer-consumer pattern. The API layer (producers) creates task records in MySQL and pushes task IDs into Redis Streams. Dedicated worker processes (consumers) monitor these streams, retrieve task metadata, and execute specialized pipelines using a refactored `TaskManager`.

### Request to Execution Flow

The following diagram bridges the "Natural Language Space" (user actions) to the "Code Entity Space" (specific functions and classes).

**Diagram: Task Dispatch and Processing**

```
Execution Engines (rag/app/)

Refactored Executor (rag/svr/task_executor_refactor/)

Message Broker (rag/utils/redis_conn.py)

Task Service (api/db/services/task_service.py)

API Layer (api/db/services/document_service.py)

DocumentService.get_by_kb_id()

TaskService.get_task()

TaskService.update_progress()

REDIS_CONN (RedisDB)

Stream: ragflow_dataflow

TaskManager (task_manager.py)

RecordingContext (recording_context.py)

Task Worker Loop

FACTORY (Parser Selection)

naive.py

paper.py
```

Sources: [rag/svr/task_executor.py18-20](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L18-L20) [api/db/services/document_service.py126-150](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L126-L150) [api/db/services/task_service.py75-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L75-L143) [rag/utils/redis_conn.py38-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/redis_conn.py#L38-L40)

## Redis Streams Queue System

RAGFlow leverages **Redis Streams** to manage task distribution. Unlike simple lists, streams provide consumer groups and message acknowledgement (ACK), ensuring tasks are not lost if a worker crashes.

### Queue Definitions and Task Types

The system maps internal task types to specific Redis Streams. The `SVR_CONSUMER_GROUP_NAME` is used to coordinate multiple workers. [common/constants.py103](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/constants.py#L103-L103)

| Task Type | Pipeline Mapping | Stream Name | Purpose |
| --- | --- | --- | --- |
| `dataflow` | `PipelineTaskType.PARSE` | `ragflow_dataflow` | Standard document parsing, OCR, and chunking. |
| `raptor` | `PipelineTaskType.RAPTOR` | `ragflow_raptor` | Recursive summarization for hierarchical retrieval. |
| `graphrag` | `PipelineTaskType.GRAPH_RAG` | `ragflow_graphrag` | Entity extraction and Knowledge Graph construction. |
| `mindmap` | `PipelineTaskType.MINDMAP` | `ragflow_mindmap` | Generating visual mind maps from document context. |
| `memory` | `PipelineTaskType.MEMORY` | `ragflow_memory` | Summarizing and indexing conversation history. |

Sources: [rag/svr/task_executor.py133-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L133-L140) [api/db/services/document_service.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L32-L32)

### Message Lifecycle

1. Consumption: The TaskManager in the refactored executor blocks on Redis Stream reads. rag/svr/task_executor.py18
2. Task Loading: The worker extracts the task_id and calls TaskService.get_task(task_id) to fetch parameters from MySQL, including document location and parser configuration. api/db/services/task_service.py75-143
3. Acknowledgement: Once a task is processed or fails definitively, it is acknowledged in Redis to prevent re-delivery. rag/utils/redis_conn.py38-40

## Worker Process Architecture

### Startup and Initialization

Workers are initialized with a specific index (`TE_IDX`) and a heartbeat mechanism to report status.

- Heartbeat: Controlled by WORKER_HEARTBEAT_TIMEOUT (default 120s). rag/svr/task_executor.py154
- Environment: Workers set LITELLM_LOCAL_MODEL_COST_MAP="True" to avoid network-blocked imports of model costs. rag/svr/task_executor.py27
- Recording Context: Uses RecordingContext to track execution metrics like time and token usage via timed_with_recording. rag/svr/task_executor.py19-20

### Concurrency and Rate Limiting

To prevent resource exhaustion (OOM or API rate limits), workers use several specialized limiters defined in `rag/svr/task_executor_limiter.py`.

**Diagram: Resource Limiter Mapping**

```
Operations

Limiters (rag/svr/task_executor_limiter.py)

task_limiter

chunk_limiter

embed_limiter

minio_limiter

kg_limiter

Main Task Processing

Document Chunking

LLM Embedding Calls

Storage (MinIO/S3) Access

Graph Construction
```

Sources: [rag/svr/task_executor.py95-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L95-L101)

## Task Execution Pipelines

### The Parser Factory

For `dataflow` tasks, the system uses a `FACTORY` dictionary to route documents to specific parsing logic based on the `ParserType`. [rag/svr/task_executor.py114-131](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L114-L131)

| Parser Type | Code Module | Use Case |
| --- | --- | --- |
| `NAIVE` | `rag.app.naive` | General text/markdown parsing. |
| `PAPER` | `rag.app.paper` | Academic papers with citation/reference handling. |
| `LAWS` | `rag.app.laws` | Structured legal documents. |
| `MANUAL` | `rag.app.manual` | Technical manuals with hierarchical sections. |
| `TABLE` | `rag.app.table` | Excel/CSV data extraction. |
| `QA` | `rag.app.qa` | Pre-formatted question-answer pairs. |

### RAPTOR Processing

The `raptor` pipeline handles hierarchical summarization logic.

- Tree Building: Configured via tree_builder (default RAPTOR_TREE_BUILDER). rag/raptor.py168
- Clustering: Supports GMM_CLUSTERING_METHOD and AHC_CLUSTERING_METHOD to group related content. rag/raptor.py38-43
- Utilities: Functions like collect_raptor_chunk_ids and should_skip_raptor manage the state of the summarization tree. rag/svr/task_executor.py48-55

## Lifecycle Management and Error Handling

### Progress Tracking

Workers update progress via `set_progress(task_id, prog, msg)`. This function:

1. Checks for cancellation via has_canceled(task_id). rag/svr/task_executor.py169
2. Formats a timestamped message and updates the progress percentage. rag/svr/task_executor.py177-183
3. Updates the Task table via TaskService.update_progress. rag/svr/task_executor.py185

### Cancellation Mechanism

The `TaskService.has_canceled` function checks the task status in the database. If a task is marked as canceled, `set_progress` raises a `TaskCanceledException`, which terminates the current execution pipeline. [rag/svr/task_executor.py189-192](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L189-L192) [api/db/services/task_service.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L32-L32)

### Retry Strategy

Tasks are resilient to transient failures:

- Retry Limit: TaskService.get_task increments a retry_count. If it reaches 3, the task is abandoned with an error message and progress is set to -1. api/db/services/task_service.py130-141
- Connection Management: Database connections are closed via close_connection() after progress updates to prevent connection leaks in long-running workers. rag/svr/task_executor.py187

Sources: [rag/svr/task_executor.py165-197](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L197) [api/db/services/task_service.py75-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L75-L143)


---



<!-- ===== 3.4-component-dynamic-loading ===== -->

# Component Dynamic Loading

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/retrieval.py
- api/apps/llm_app.py
- api/db/init_data.py
- api/db/services/llm_service.py
- common/mcp_tool_call_conn.py
- conf/llm_factories.json
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- rag/prompts/generator.py
- web/src/pages/user-setting/setting-model/constant.ts

## Purpose and Scope

This document explains RAGFlow's dynamic class discovery and loading mechanism for agent components, workflow nodes, and LLM model providers. The system automatically discovers, registers, and provides runtime lookup for component classes located in the `agent/component/`, `agent/tools/`, and `rag/llm/` packages. This plugin-like architecture enables the `Graph` engine in `agent/canvas.py` and the LLM factory layer to instantiate components and models by name without hardcoding imports, facilitating easy extension of the system's capabilities.

For information about component base classes and lifecycle, see [8.2 Component System Architecture](https://github.com/infiniflow/ragflow/blob/d32e05d5/8.2 Component System Architecture) For details on individual model implementations, see [5.1 LLMBundle and Model Types](https://github.com/infiniflow/ragflow/blob/d32e05d5/5.1 LLMBundle and Model Types)

## Architecture Overview

The component dynamic loading system consists of three primary mechanisms:

1. Module Discovery: Automatic scanning of Python files in target directories (like agent/component/ or rag/llm/) at import time.
2. Class Extraction: Inspection-based registration of classes into global registries and namespaces.
3. Lazy Lookup: Runtime resolution of classes by string identifiers using functions like component_class() for agents or mapping dictionaries for LLMs.

### Component Discovery Architecture

Title: Component Discovery and Registration Flow

```
Runtime_Lookup

Registry

Component_Files

Package_Initialization

agent/component/init.py
Module Entry Point

_import_submodules()
Directory Scanner

_extract_classes_from_module()
Class Inspector

.py files
(exclude __ and base*)

llm.py

agent_with_tools.py

categorize.py

... etc

__all_classes dict
{'LLM': LLM class,
'Categorize': Categorize class,
...}

globals() namespace
Exported symbols

component_class(class_name)
Search Function

Search order:
1. agent.component
2. agent.tools
3. rag.flow

Return class object
or raise assertion
```

The loading process occurs during package initialization, populating the registry with all available component classes. The `component_class()` function then provides O(1) lookup during workflow execution in `Graph.load()`.

**Sources:** [agent/component/__init__.py22-59](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/__init__.py#L22-L59) [agent/canvas.py96-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L96-L111)

## Component Discovery Process

### Directory Scanning

The `_import_submodules()` function scans the `agent/component/` directory for Python modules at package initialization time. It ignores special files and base classes to prevent circular dependencies.

Title: Module Discovery Algorithm

```
Yes (skip)

No (process)

Success

Exception

Yes

No

Package Import:
import agent.component

os.listdir(_package_path)
Get all files in directory

Filter file:
• Starts with '__'?
• Ends with '.py'?
• Starts with 'base'?

Skip file

module_name = filename[:-3]
Remove .py extension

importlib.import_module()
Dynamic import

_extract_classes_from_module()
Register classes

ImportError:
Print warning, continue

More files?

Registry populated
```

**Key implementation details:**

- Directory Access: Uses os.path.dirname(__file__) to locate the package path agent/component/__init__.py22
- Filtering: Skips files starting with __, files not ending in .py, and files starting with base agent/component/__init__.py26-28
- Dynamic Import: Uses importlib.import_module with relative package naming agent/component/__init__.py32

**Sources:** [agent/component/__init__.py22-36](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/__init__.py#L22-L36)

## LLM Provider Dynamic Loading

A similar dynamic loading pattern is applied to LLM providers in `rag/llm/__init__.py`. This ensures that any new model implementation (Chat, Embedding, Rerank, etc.) is automatically registered in the `FACTORY` system.

### Model Type Registration

The system defines several global dictionaries to hold model implementations:

- ChatModel rag/llm/__init__.py142
- EmbeddingModel rag/llm/__init__.py144
- RerankModel rag/llm/__init__.py145
- CvModel rag/llm/__init__.py143

### The Loading Loop

The package initialization iterates through `MODULE_MAPPING` to populate these dictionaries [rag/llm/__init__.py152-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L152-L161) It inspects each module for classes inheriting from `Base` or `LiteLLMBase` and uses a special attribute `_FACTORY_NAME` to register them [rag/llm/__init__.py171-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L171-L180)

Title: LLM Model Provider Discovery

```
Global_Registries

Extraction_Logic

Model_Files

chat_model.py

embedding_model.py

rerank_model.py

inspect.getmembers(module)

Is subclass of Base?

Get _FACTORY_NAME attribute

ChatModel = {'OpenAI': OpenAIChat, ...}

EmbeddingModel = {'OpenAI': OpenAIEmbed, ...}
```

**Sources:** [rag/llm/__init__.py142-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L142-L180)

## Component Lookup and Instantiation

### `component_class()` Implementation

The `component_class()` function is the primary entry point for the `Graph` engine to resolve a string name (from the frontend or DSL) into a functional Python class [agent/component/__init__.py51-59](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/__init__.py#L51-L59)

### Search Path Priority

The function searches three module namespaces in specific order to resolve the class:

Title: Search Path Priority Logic

```
Found

Not found

Found

Not found

Found

Not found

component_class('LLM')

Try: agent.component
getattr(module, 'LLM')

Try: agent.tools
getattr(module, 'LLM')

Try: rag.flow
getattr(module, 'LLM')

Return class object

AssertionError:
Can't import LLM
```

1. `agent.component`: Standard workflow nodes (e.g., LLM, Categorize, Message).
2. `agent.tools`: Specialized tools used by the Agent component (e.g., Retrieval).
3. `rag.flow`: Specialized flow components or legacy definitions.

**Sources:** [agent/component/__init__.py51-59](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/__init__.py#L51-L59)

## Data Flow: From DSL to Runtime Object

The dynamic loading system bridges the gap between static JSON DSL (Domain Specific Language) and runtime Python entities.

### DSL to Code Mapping

Title: Natural Language Space to Code Entity Space Mapping

```

```

### Dynamic Loading in Graph Initialization

When a `Graph` is initialized in `agent/canvas.py`, it iterates through the components defined in the DSL:

1. Parameter Class Resolution: Calls component_class(name + "Param") to find the parameter schema class agent/canvas.py101
2. Parameter Update: Calls param.update(cpn["obj"]["params"]) to populate the object with DSL values agent/canvas.py103
3. Component Class Resolution: Calls component_class(name) to find the execution class agent/canvas.py109
4. Instantiation: Creates the component instance passing the graph reference, ID, and parameters: component_class(name)(self, k, param) agent/canvas.py109

### Agent Tool Loading

The `Agent` component uses this same mechanism to dynamically load tools. In its `__init__`, it iterates through `self._param.tools` and calls `_load_tool_obj`, which uses `component_class` to resolve both the tool class and its parameter class.

**Sources:** [agent/canvas.py96-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L96-L111) [agent/component/__init__.py51-59](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/__init__.py#L51-L59) [agent/component/base.py127-188](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L127-L188)


---



<!-- ===== 3.5-go-server-and-native-components ===== -->

# Go Server and Native Components

Relevant source files
- cmd/admin_server.go
- cmd/server_main.go
- go.mod
- go.sum
- internal/admin/handler.go
- internal/admin/password.go
- internal/admin/router.go
- internal/admin/service.go
- internal/cli/admin_command.go
- internal/common/error_code.go
- internal/common/json_types.go
- internal/common/json_types_test.go
- internal/common/metadata_utils.go
- internal/common/metadata_utils_test.go
- internal/common/smtp_config.go
- internal/common/status_message.go
- internal/dao/database.go
- internal/dao/document.go
- internal/dao/document_test.go
- internal/dao/file2document.go
- internal/dao/kb.go
- internal/dao/memory.go
- internal/dao/task.go
- internal/dao/task_test.go
- internal/dao/user.go
- internal/dao/user_tenant.go
- internal/engine/elasticsearch/chunk.go
- internal/engine/elasticsearch/chunk_helpers_test.go
- internal/engine/elasticsearch/chunk_test.go
- internal/engine/elasticsearch/client.go
- internal/engine/elasticsearch/kg_test.go
- internal/engine/infinity/chunk.go
- internal/engine/types/types.go
- internal/handler/chat_session.go
- internal/handler/chunk.go
- internal/handler/chunk_test.go
- internal/handler/datasets.go
- internal/handler/dify_retrieval_handler.go
- internal/handler/document.go
- internal/handler/document_test.go
- internal/handler/error.go
- internal/handler/error_response_test.go
- internal/handler/file.go
- internal/handler/kb.go
- internal/handler/memory.go
- internal/handler/memory_message_test.go
- internal/handler/searchbot.go
- internal/handler/searchbot_test.go
- internal/handler/streaming.go
- internal/handler/streaming_test.go
- internal/handler/system.go
- internal/handler/user.go
- internal/router/router.go
- internal/server/config.go
- internal/service/chat_session.go
- internal/service/chat_session_test.go
- internal/service/dataset.go
- internal/service/dataset_document_metadata_config_test.go
- internal/service/document.go
- internal/service/document_test.go
- internal/service/enrich_metadata_test.go
- internal/service/file2document.go
- internal/service/kb.go
- internal/service/llm.go
- internal/service/memory.go
- internal/service/memory_message_test.go
- internal/service/metadata.go
- internal/service/metadata_filter.go
- internal/service/metadata_filter_test.go
- internal/service/nlp/query_builder_test.go
- internal/service/nlp/reranker.go
- internal/service/nlp/reranker_normalize_test.go
- internal/service/nlp/retrieval.go
- internal/service/user.go
- internal/utility/captcha_png.go
- internal/utility/otp.go
- internal/utility/smtp.go
- internal/utility/token.go

The Go-based infrastructure in RAGFlow serves as a high-performance service layer that complements the Python backend. It handles administrative tasks, system-wide management, and provides high-speed native components for performance-critical operations like tokenization and search query building.

## Go Server Architecture

The Go server (documented in `cmd/server_main.go`) initializes the core system components, including the database, document engine, and cache, before launching the Gin-based web server.

### System Initialization Flow

The initialization process follows a strict sequence to ensure all dependencies are available before the server starts accepting requests.

1. Logger & Config: Initializes the Zap logger and loads the configuration via Viper cmd/server_main.go77-84
2. Database (MySQL): Connects via GORM and performs auto-migrations for core models cmd/server_main.go116-118
3. Document Engine: Initializes the connection to the configured backend (Elasticsearch or Infinity) cmd/server_main.go121-124
4. Redis Cache: Establishes a connection for distributed locking and session management cmd/server_main.go127-130
5. Storage Factory: Initializes the storage layer (MinIO, S3, or OSS) cmd/server_main.go132-134
6. Native Tokenizer: Initializes the C++ based rag_analyzer using the specified dictionary path cmd/server_main.go150-160
7. Query Builder: Sets up the global NLP query builder using the tokenizer's dictionary path for synonym support cmd/server_main.go164-166

Sources: [cmd/server_main.go64-171](https://github.com/infiniflow/ragflow/blob/d32e05d5/cmd/server_main.go#L64-L171) [internal/dao/database.go1-100](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/dao/database.go#L1-L100)

### Go Server Component Bridge

The following diagram illustrates how the Go server entities map to the system's operational logic.

**Diagram: Go Server Component Mapping**

```
Code Entity Space

Natural Language Space

API Request

User Management

Search Query

router.Router.Setup

handler.UserHandler

service.UserService

dao.UserDAO

handler.SearchHandler

service.SearchService

nlp.InitQueryBuilderFromTokenizer

tokenizer.Init
```

Sources: [cmd/server_main.go183-215](https://github.com/infiniflow/ragflow/blob/d32e05d5/cmd/server_main.go#L183-L215) [internal/router/router.go108-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/router/router.go#L108-L175) [internal/service/user.go52-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/user.go#L52-L61)

## Internal Service Layer

The Go backend implements a standard Controller-Service-DAO (Data Access Object) pattern. The service layer contains the business logic that ensures compatibility with the Python backend.

### Key Services and Functions

| Service | Key Functionality | Source |
| --- | --- | --- |
| `UserService` | Handles registration, login, and password hashing (PBKDF2/Scrypt). | [internal/service/user.go109-240](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/user.go#L109-L240) |
| `DocumentService` | Manages document metadata, thumbnails, and lifecycle operations. | [internal/service/document.go44-54](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/document.go#L44-L54) |
| `ChunkService` | Handles retrieval tests and chunk-level operations. | [cmd/server_main.go188](https://github.com/infiniflow/ragflow/blob/d32e05d5/cmd/server_main.go#L188-L188) |
| `SearchService` | Orchestrates hybrid search and interacts with the document engine. | [cmd/server_main.go195](https://github.com/infiniflow/ragflow/blob/d32e05d5/cmd/server_main.go#L195-L195) |
| `MemoryService` | Manages persistent memory and message history for agents. | [cmd/server_main.go197](https://github.com/infiniflow/ragflow/blob/d32e05d5/cmd/server_main.go#L197-L197) |
| `TenantService` | Manages multi-tenant configurations and default model settings. | [internal/service/user.go196-206](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/user.go#L196-L206) |
| `AdminService` | Provides superuser-level management of tasks and users. | [internal/admin/service.go46-66](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L46-L66) |

Sources: [cmd/server_main.go183-200](https://github.com/infiniflow/ragflow/blob/d32e05d5/cmd/server_main.go#L183-L200) [internal/admin/service.go46-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L46-L91)

## Ingestion and Admin Servers

Beyond the main server, RAGFlow utilizes specialized Go components for background tasks and administration.

### Ingestion Manager

The `IngestionTaskDAO` manages the lifecycle of document ingestion tasks. It tracks task progress, status (unstart, running, done, fail), and step checkpoints [internal/admin/service.go104-148](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L104-L148) Tasks can be stopped or removed via the `StopIngestionTasks` and `RemoveIngestionTasks` methods [internal/admin/service.go150-186](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L150-L186)

### Admin Handler

The `internal/admin/handler.go` provides endpoints for superusers to monitor the system.

- Task Listing: Lists all ingestion tasks including user info and current step internal/admin/handler.go348-354
- User Management: Allows admins to create, delete, and list users across the platform internal/admin/handler.go209-275
- Authentication: Implements a dedicated admin login that enforces IsSuperuser checks internal/admin/handler.go116-185

Sources: [internal/admin/handler.go39-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L39-L50) [internal/admin/service.go104-186](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L104-L186)

## Native Components (C++ and NLP)

The Go layer acts as a wrapper for high-performance native components required for Natural Language Processing.

### RAGAnalyzer Tokenizer

The tokenizer is a performance-critical component bridged from C++ to Go.

- Initialization: Loads resources from a path defined by RAGFLOW_DICT_PATH or /usr/share/infinity/resource cmd/server_main.go150-159
- Implementation: Accessed through the tokenizer package which manages the pool of analyzers cmd/server_main.go49

### NLP Query Builder

The `nlp.QueryBuilder` transforms natural language queries into structured search requests.

- Synonym Handling: Initialized using the tokenizer's dictionary path to ensure consistent wordnet access cmd/server_main.go164-166

## Data Flow: Document Retrieval

The following diagram describes the flow of a document retrieval request through the Go components.

**Diagram: Document Retrieval Flow**

```
Storage/Engine

Service Layer

Request Entry

GET /api/v1/documents/:id

handler.DocumentHandler.GetDocumentByID

service.DocumentService.GetDocumentByID

dao.DocumentDAO.GetByID

MySQL: document table

engine.DocEngine.Get
```

Sources: [internal/router/router.go184-187](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/router/router.go#L184-L187) [internal/handler/document.go129-155](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/handler/document.go#L129-L155) [internal/service/document.go44-54](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/document.go#L44-L54) [internal/dao/document.go37-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/dao/document.go#L37-L44)

### Metadata Management

The `DocumentService` manages complex document metadata, including the ability to aggregate parsing status counts (unstart, running, cancel, done, fail) for datasets [internal/dao/document.go175-213](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/dao/document.go#L175-L213) This mirrors the Python backend's logic to ensure consistency across the dual-language architecture.

Sources: [internal/dao/document.go175-213](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/dao/document.go#L175-L213) [internal/service/document.go44-54](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/document.go#L44-L54)


---



<!-- ===== 4-frontend-application ===== -->

# Frontend Application

Relevant source files
- web/package-lock.json
- web/package.json
- web/src/app.tsx
- web/src/components/ui/badge.tsx
- web/src/components/ui/button.tsx
- web/src/components/ui/radio-group.tsx
- web/src/components/ui/radio.tsx
- web/src/components/ui/segmented.tsx
- web/src/constants/common.ts
- web/src/locales/config.ts
- web/src/locales/de.ts
- web/src/locales/en.ts
- web/src/locales/es.ts
- web/src/locales/fr.ts
- web/src/locales/id.ts
- web/src/locales/it.ts
- web/src/locales/ja.ts
- web/src/locales/pt-br.ts
- web/src/locales/ru.ts
- web/src/locales/vi.ts
- web/src/locales/zh-traditional.ts
- web/src/locales/zh.ts
- web/src/pages/datasets/dataset-card.tsx
- web/src/pages/home/applications.tsx
- web/src/pages/home/banner.tsx
- web/src/pages/home/datasets.tsx
- web/src/pages/home/index.tsx
- web/tailwind.config.js
- web/tailwind.css

The RAGFlow frontend is a modern React-based web application built with Vite that provides the user interface for all system features including dataset management, chat assistants, agent workflows, and search interfaces. The application emphasizes internationalization (supporting 13+ languages), theming (light/dark mode), and a component-driven architecture using a hybrid UI library approach.

For detailed information about specific frontend subsystems, see [Internationalization System](/infiniflow/ragflow/4.1-internationalization-system), [UI Component Architecture](/infiniflow/ragflow/4.2-ui-component-architecture), [Theming System](/infiniflow/ragflow/4.3-theming-system), and [Page Structure and State Management](/infiniflow/ragflow/4.4-page-structure-and-state-management). For backend API integration, see [Backend API System](/infiniflow/ragflow/7-backend-api-system) and [API Architecture and SDK](/infiniflow/ragflow/7.1-api-architecture-and-sdk).

## Technology Stack

The frontend is built on the following core technologies:

| Category | Technology | Version | Purpose |
| --- | --- | --- | --- |
| Build Tool | Vite | 7.2.7 | Development server and production bundler [[web/package.json192](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L192-L192)] |
| Framework | React | 18.2.0 | UI component library [[web/package.json104](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L104-L104)] |
| Routing | React Router | 7.10.1 | Client-side routing [[web/package.json118](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L118-L118)] |
| UI Libraries | Ant Design | 5.x | Rich component library (Icons/Charts) [[web/package.json32-34](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L32-L34)] |
|  | Radix UI | 1.x | Headless accessible primitives [[web/package.json42-67](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L42-L67)] |
|  | Tailwind CSS | 3.x | Utility-first styling [[web/package.json188](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L188-L188)] |
| State Management | React Query | 5.40.0 | Server state caching [[web/package.json69](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L69-L69)] |
|  | Zustand | 4.5.2 | Client state management [[web/package.json137](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L137-L137)] |
| i18n | i18next | 23.7.16 | Translation framework [[web/package.json89](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L89-L89)] |
| Visualization | React Flow | 12.3.6 | Node-based agent canvas editor [[web/package.json75](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L75-L75)] |
| Type System | TypeScript | 5.9.3 | Static type checking [[web/package.json191](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L191-L191)] |

**Sources:** [[web/package.json1-200](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L1-L200)]

## Application Entry Point and Provider Hierarchy

### Provider Architecture

The application uses a nested provider hierarchy to supply global context to all components. The `AppContainer` serves as the top-level entry point in `app.tsx`, wrapping the application in a series of functional providers.

Title: Frontend Provider Hierarchy

```
Toast_Notifications

Global_Services

AppContainer
(app.tsx)

RootProvider
(app.tsx)

TooltipProvider
(app.tsx)

QueryClientProvider
(app.tsx)

ThemeProvider
(app.tsx)

Root
(app.tsx)

RouterProviderWrapper
(app.tsx)

queryClient
(app.tsx)

next-themes
(package.json:99)

i18next
(package.json:89)

Sonner
(package.json:128)
```

**Sources:** [[web/src/app.tsx1-113](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/src/app.tsx#L1-L113)], [[web/package.json1-138](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L1-L138)]

### Key Provider Responsibilities

- `QueryClientProvider`: Configures React Query with a queryClient to manage server-side state fetching and caching [web/package.json69].
- `ThemeProvider`: Manages the application theme using next-themes [web/package.json99]. It provides the infrastructure for light and dark mode switching by toggling classes that correspond to CSS variables defined in tailwind.css [web/tailwind.css6-135].
- `TooltipProvider`: Provides accessibility context for Radix UI tooltips via @radix-ui/react-tooltip [web/package.json67].
- `Sonner`: A toast notification library used for global application feedback [web/package.json128].

**Sources:** [[web/package.json1-138](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L1-L138)], [[web/tailwind.css1-216](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/tailwind.css#L1-L216)]

## State Management Architecture

RAGFlow utilizes a hybrid state management strategy to optimize for different data lifecycles:

- Server State: Managed via @tanstack/react-query [web/package.json69]. This handles fetching, caching, and synchronizing data with the backend API.
- Client State: Managed via Zustand [web/package.json137]. This is used for complex UI state such as the Agent workflow editor and shared application state that does not require persistent server synchronization.
- Persistent State: User preferences like language and theme are stored in local storage and synchronized with i18next and next-themes [web/package.json89-99].

**Sources:** [[web/package.json69-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L69-L137)]

## UI Component Architecture

The frontend follows a multi-tier component strategy:

1. Primitives: Radix UI headless primitives (e.g., Accordion, Dialog, Popover, Select) provide the base for accessible UI components [web/package.json42-67].
2. Visualization Integration: Integration with @xyflow/react for the node-based agent canvas [web/package.json75] and @antv/g6 for graph visualization [web/package.json34].
3. Document Preview: Extensive support for document types (PDF, Excel, PPTX, Docx) via specialized preview libraries such as @js-preview/excel, pptx-preview, and react-pdf-highlighter [web/package.json35-115].

For more details, see [UI Component Architecture](/infiniflow/ragflow/4.2-ui-component-architecture).

**Sources:** [[web/package.json32-115](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L32-L115)]

## Internationalization (i18n)

The system supports 13+ languages through `i18next` and `react-i18next` [[web/package.json89-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/package.json#L89-L112)].

- Supported Languages: Includes English (en.ts), Simplified Chinese (zh.ts), Traditional Chinese (zh-traditional.ts), Russian (ru.ts), Indonesian (id.ts), Spanish (es.ts), Vietnamese (vi.ts), Japanese (ja.ts), Portuguese (Brazil) (pt-br.ts), German (de.ts), French (fr.ts), Italian (it.ts), and Turkish [web/src/locales/en.ts27-42].
- Key Hierarchy: Translations are organized by feature areas such as common, login, header, knowledgeDetails, and memories [web/src/locales/en.ts3-185].

For more details, see [Internationalization System](/infiniflow/ragflow/4.1-internationalization-system).

**Sources:** [[web/src/locales/en.ts1-190](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/src/locales/en.ts#L1-L190)], [[web/src/locales/zh.ts1-235](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/src/locales/zh.ts#L1-L235)]

## Theming System

Theming is implemented using Tailwind CSS and CSS variables. The system supports a native dark mode via the `.dark` selector [[web/tailwind.css137](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/tailwind.css#L137-L137)].

- Semantic Colors: Defines variables like --primary, --accent, --state-success, and --background in :root for light mode [web/tailwind.css7-120] and overrides them within the .dark class [web/tailwind.css138-216].
- Utility Integration: Tailwind utility classes are mapped to these variables, allowing for dynamic UI updates when the theme changes [web/tailwind.css1-3].

For more details, see [Theming System](/infiniflow/ragflow/4.3-theming-system).

**Sources:** [[web/tailwind.css1-216](https://github.com/infiniflow/ragflow/blob/d32e05d5/[web/tailwind.css#L1-L216)]


---



<!-- ===== 4.1-internationalization-system ===== -->

# Internationalization System

Relevant source files
- web/package-lock.json
- web/package.json
- web/src/app.tsx
- web/src/components/ui/badge.tsx
- web/src/components/ui/button.tsx
- web/src/components/ui/radio-group.tsx
- web/src/components/ui/radio.tsx
- web/src/components/ui/segmented.tsx
- web/src/constants/common.ts
- web/src/locales/config.ts
- web/src/locales/de.ts
- web/src/locales/en.ts
- web/src/locales/es.ts
- web/src/locales/fr.ts
- web/src/locales/id.ts
- web/src/locales/it.ts
- web/src/locales/ja.ts
- web/src/locales/pt-br.ts
- web/src/locales/ru.ts
- web/src/locales/vi.ts
- web/src/locales/zh-traditional.ts
- web/src/locales/zh.ts
- web/src/pages/datasets/dataset-card.tsx
- web/src/pages/home/applications.tsx
- web/src/pages/home/banner.tsx
- web/src/pages/home/datasets.tsx
- web/src/pages/home/index.tsx
- web/tailwind.config.js
- web/tailwind.css

This document describes the internationalization (i18n) system used in the RAGFlow web frontend. The system provides multi-language support across the entire user interface using `i18next` for translation management, supporting 13+ languages through a hierarchical namespace structure and dynamic loading.

## Overview

The RAGFlow internationalization system is built on `i18next` and `react-i18next`. It manages translations for a wide range of languages, including English, Simplified Chinese, Traditional Chinese, Indonesian, Japanese, Spanish, Vietnamese, Russian, Portuguese (Brazil), German, French, Italian, Bulgarian, Arabic, and Turkish. The system uses a dynamic loading strategy where the English reference bundle serves as the primary source of truth for keys.

**Sources:** [web/src/locales/en.ts25-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L25-L41) [web/src/locales/zh.ts26-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/zh.ts#L26-L31) [web/src/locales/vi.ts17-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/vi.ts#L17-L41) [web/src/locales/de.ts26-33](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/de.ts#L26-L33) [web/package.json89-92](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/package.json#L89-L92)

## Supported Languages

The system supports the following languages, defined in the translation files and localized for the language selection UI:

| Language | Key | Reference File |
| --- | --- | --- |
| English | `english` | `en.ts` |
| Simplified Chinese | `chinese` | `zh.ts` |
| Traditional Chinese | `traditionalChinese` | `zh-traditional.ts` |
| Russian | `russian` | `ru.ts` |
| Indonesian | `indonesia` | `id.ts` |
| Japanese | `japanese` | `ja.ts` |
| Vietnamese | `vietnamese` | `vi.ts` |
| Portuguese (Brazil) | `portugueseBr` | `pt-br.ts` |
| German | `german` | `de.ts` |
| Italian | `italian` | `it.ts` |
| Spanish | `spanish` | `es.ts` |
| French | `french` | `fr.ts` |

**Sources:** [web/src/locales/en.ts26-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L26-L41) [web/src/locales/zh.ts26-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/zh.ts#L26-L31) [web/src/locales/vi.ts17-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/vi.ts#L17-L41) [web/src/locales/de.ts26-33](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/de.ts#L26-L33) [web/src/locales/id.ts18-23](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/id.ts#L18-L23) [web/src/locales/ja.ts18-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/ja.ts#L18-L40)

## System Architecture and Data Flow

The i18n system bridges the gap between raw translation data and React UI components through a centralized configuration and a dynamic loading mechanism.

```
UILayer

CodeEntitySpace (i18n Engine)

NaturalLanguageSpace (Translation Files)

web/src/locales/en.ts (Reference)

web/src/locales/zh.ts

web/src/locales/ja.ts

web/src/locales/ru.ts

...other locales

i18next Instance

i18next-browser-languagedetector

react-i18next

useTranslation Hook

t() function

common.language
```

**Title:** I18n Data Flow and Entity Mapping

### Key Components

- i18next: The core library providing the translation engine web/package.json89
- react-i18next: React bindings that provide the useTranslation hook and Trans component for UI integration web/package.json90
- i18next-browser-languagedetector: Automatically detects user language based on browser settings or cookies web/package.json90
- Translation Bundles: Static TypeScript files (e.g., en.ts, zh.ts, ja.ts) that export a nested object containing all UI strings web/src/locales/en.ts1-2 web/src/locales/ja.ts1-2

**Sources:** [web/package.json89-90](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/package.json#L89-L90) [web/src/locales/en.ts1-2](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L1-L2) [web/src/locales/ja.ts1-2](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/ja.ts#L1-L2)

## Translation Key Hierarchy

Translations are organized into feature-based namespaces to maintain a manageable hierarchy.

```
TranslationKeyHierarchy

translation (Root)

common

login

header

skills

memories / memory

knowledgeList / knowledgeDetails

confirm, back, delete, language

createSpace, uploadSkill, skillName

createMemory, memoryType, llm

createKnowledgeBase, metadata, chunkMethod
```

**Title:** Key Hierarchy and Feature Organization

### Feature-Specific Organization

1. Common Namespace: Contains generic UI labels like confirm, cancel, delete, and the names of all supported languages for the language switcher web/src/locales/en.ts3-82 It also includes specialized labels for the MCP (Model Context Protocol) integration web/src/locales/en.ts73-79
2. Skills System: Manages translations for the skill space, including space creation, skill uploading, and metadata validation messages web/src/locales/en.ts125-203 It supports both local uploads and Git imports web/src/locales/zh.ts208-222
3. Memory System: Divided into memories (list view/creation) and memory (individual memory configuration and message management). It includes specific keys for memory types like raw, semantic, episodic, and procedural web/src/locales/de.ts112-183 web/src/locales/ru.ts119-203
4. Knowledge Base: Managed via knowledgeList for the dashboard and knowledgeDetails for specific dataset configurations web/src/locales/zh.ts94-243 This section includes complex instructions for PDF parsers and chunking methods web/src/locales/zh-traditional.ts147-173
5. Login/Auth: Handles all strings for sign-in, sign-up, and branding descriptions web/src/locales/en.ts84-108

**Sources:** [web/src/locales/en.ts3-203](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L3-L203) [web/src/locales/zh.ts94-243](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/zh.ts#L94-L243) [web/src/locales/de.ts112-183](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/de.ts#L112-L183) [web/src/locales/ru.ts119-203](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/ru.ts#L119-L203) [web/src/locales/zh-traditional.ts147-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/zh-traditional.ts#L147-L173)

## Technical Implementation Details

### Interpolation and Variables

The system supports dynamic content using double curly braces. For example:

- skills.filesCount: '{{count}} files' (e.g., in Chinese: '{{count}} 个文件') web/src/locales/en.ts155 web/src/locales/zh.ts137
- knowledgeDetails.redo: 'Do you want to clear the existing {{chunkNum}} chunks?' (e.g., in Traditional Chinese: '是否清空已有 {{chunkNum}}個 chunk？') web/src/locales/zh-traditional.ts178

### Complex Key Structures

For features like the **Memory System**, keys are deeply nested to reflect the UI structure:

- memory.config.memorySizeTooltip: Provides detailed technical explanations of memory calculation including dimension byte sizes web/src/locales/de.ts161-162 web/src/locales/ru.ts177-178
- memory.messages.delMessageWarn: Warning text for destructive actions in the message list web/src/locales/de.ts148-149

### Metadata Management

The `skills.metadata` section demonstrates how the system handles complex configuration UIs, with keys for information categories (`basic`, `emoji`, `skillKey`) and requirements (`requiredBins`, `requiredEnv`) [web/src/locales/en.ts185-195](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L185-L195) Validation messages are also localized to provide clear feedback during skill parsing, such as `invalid_frontmatter` or `total_size_exceeded` [web/src/locales/zh.ts182-205](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/zh.ts#L182-L205)

**Sources:** [web/src/locales/en.ts155-195](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L155-L195) [web/src/locales/zh.ts137-205](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/zh.ts#L137-L205) [web/src/locales/de.ts148-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/de.ts#L148-L162) [web/src/locales/ru.ts177-178](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/ru.ts#L177-L178) [web/src/locales/zh-traditional.ts178](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/zh-traditional.ts#L178-L178)


---



<!-- ===== 4.2-ui-component-architecture ===== -->

# UI Component Architecture

Relevant source files
- .gitignore
- common/data_source/file_types.py
- web/src/assets/svg/file-icon/mdx.svg
- web/src/components/auto-keywords-form-field.tsx
- web/src/components/chunk-method-dialog/hooks.ts
- web/src/components/chunk-method-dialog/index.tsx
- web/src/components/chunk-method-dialog/use-default-parser-values.ts
- web/src/components/delimiter-form-field.tsx
- web/src/components/document-preview/ppt-preview.tsx
- web/src/components/entity-types-form-field.tsx
- web/src/components/excel-to-html-form-field.tsx
- web/src/components/file-upload.tsx
- web/src/components/floating-chat-widget-markdown.tsx
- web/src/components/floating-chat-widget.tsx
- web/src/components/llm-setting-items/slider.tsx
- web/src/components/markdown-content/index.module.less
- web/src/components/markdown-content/index.tsx
- web/src/components/max-token-number-from-field.tsx
- web/src/components/message-history-window-size-item.tsx
- web/src/components/message-item/index.module.less
- web/src/components/next-markdown-content/index.module.less
- web/src/components/next-markdown-content/index.tsx
- web/src/components/next-message-item/group-button.tsx
- web/src/components/next-message-item/index.module.less
- web/src/components/originui/number-input.tsx
- web/src/components/page-rank-form-field.tsx
- web/src/components/parse-configuration/graph-rag-form-fields.tsx
- web/src/components/parse-configuration/raptor-form-fields.tsx
- web/src/components/pdf-drawer/hooks.ts
- web/src/components/pdf-drawer/index.tsx
- web/src/components/rerank.tsx
- web/src/components/similarity-slider/index.tsx
- web/src/components/slider-input-form-field.tsx
- web/src/components/switch-fom-field.tsx
- web/src/components/top-n-item.tsx
- web/src/components/ui/input.tsx
- web/src/components/ui/select.tsx
- web/src/components/use-knowledge-graph-item.tsx
- web/src/constants/form.ts
- web/src/hooks/parser-config-utils.ts
- web/src/interfaces/database/dataset.ts
- web/src/interfaces/database/document.ts
- web/src/interfaces/request/document.ts
- web/src/pages/agent/flow-tooltip.tsx
- web/src/pages/agent/form/categorize-form/dynamic-categorize.tsx
- web/src/pages/agent/form/categorize-form/dynamic-example.tsx
- web/src/pages/agent/form/categorize-form/index.tsx
- web/src/pages/agent/form/categorize-form/use-form-schema.ts
- web/src/pages/agent/form/rewrite-question-form/index.tsx
- web/src/pages/agent/form/variable-aggregator-form/name-input.tsx
- web/src/pages/dataset/dataset-setting/configuration/common-item.tsx
- web/src/pages/dataset/dataset-setting/configuration/naive.tsx
- web/src/pages/dataset/dataset-setting/form-schema.ts
- web/src/pages/dataset/dataset-setting/hooks.ts
- web/src/pages/dataset/dataset-setting/index.tsx
- web/src/pages/dataset/dataset-setting/saving-button.tsx
- web/src/pages/dataset/dataset/use-change-document-parser.ts
- web/src/pages/dataset/index.tsx
- web/src/pages/dataset/sidebar/index.tsx
- web/src/pages/datasets/dataset-creating-dialog.tsx
- web/src/pages/datasets/hooks.ts
- web/src/pages/next-chats/utils.ts
- web/src/pages/next-search/markdown-content/index.tsx
- web/src/stories/number-input.stories.ts

This page documents the UI component architecture of the RAGFlow web frontend. The system leverages a multi-layered approach combining **Ant Design** for specialized layouts, **Radix UI** for accessible low-level primitives, and **Tailwind CSS** for utility-first styling. A specialized integration with **React Flow** powers the visual agent workflow editor, while **React Hook Form** and **Zod** provide a unified data layer for complex configurations.

## Architecture Overview

RAGFlow's frontend architecture is built on a "best-of-breed" component strategy:

1. Ant Design (AntD): Used for specialized UI components and the extensive icon library.
2. Radix UI: Provides the accessible foundation for custom-styled primitive components like Select, Popover, Dialog, and Command. web/src/components/ui/select.tsx3-5
3. Tailwind CSS: The primary styling engine, using CSS variables to bridge design tokens with Radix-based custom components. web/src/components/ui/input.tsx72-81
4. React Flow (@xyflow/react): The engine for the graph-based Agent Canvas and workflow editing.
5. React Hook Form & Zod: Unified form state management and schema validation. web/src/pages/agent/form/categorize-form/index.tsx5-7 web/src/pages/dataset/dataset-setting/form-schema.ts5-41

### Component Stack Diagram

The following diagram illustrates how different libraries and custom components collaborate within the application shell and form systems.

**Diagram: UI Component Hierarchy and Data Flow**

```
UI_Primitives

Form_Management

Agent_Workflow_Engine

Agent Canvas

@xyflow/react

CategorizeForm [web/src/pages/agent/form/categorize-form/index.tsx]

LargeModelFormField [web/src/components/large-model-form-field.tsx]

Application_Shell

AppContainer

RootProvider

ThemeProvider

QueryClientProvider

useForm [react-hook-form]

zodResolver [@hookform/resolvers/zod]

FormSchema [Zod]

Form Context

FormControl [web/src/components/ui/form.tsx]

Input [web/src/components/ui/input.tsx]

SelectTrigger

@radix-ui/react-select

Tailwind CSS
```

Sources: [web/src/components/ui/select.tsx1-13](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/select.tsx#L1-L13) [web/src/components/ui/input.tsx1-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/input.tsx#L1-L31) [web/src/pages/agent/form/categorize-form/index.tsx21-47](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/form/categorize-form/index.tsx#L21-L47) [web/src/pages/dataset/dataset-setting/form-schema.ts5-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/dataset-setting/form-schema.ts#L5-L41)

## Core Component Implementation

### 1. Unified Input System

The `Input` component integrates Tailwind styling with functional extensions like password toggles and search prefixes.

- Implementation: Uses React.forwardRef to allow integration with react-hook-form. web/src/components/ui/input.tsx18-31
- Number Handling: A specialized NumberInput ensures type safety by converting string inputs to numbers automatically. web/src/components/ui/input.tsx222-237
- Performance Optimization: BlurInput (memoized as InnerBlurInput) optimizes performance by maintaining local state and only triggering the onChange callback on the onBlur event. web/src/components/ui/input.tsx176-216
- Search Integration: SearchInput provides a pre-configured search field with a Search icon prefix. web/src/components/ui/input.tsx163-172

### 2. Accessible Selects and Modals

RAGFlow uses Radix UI primitives to build accessible selection and overlay components.

- Select Implementation: Wraps SelectPrimitive from Radix to provide consistent styling via Tailwind. web/src/components/ui/select.tsx13-58
- Clearable Select: The SelectTrigger supports an allowClear prop, which displays an X icon to reset the value. web/src/components/ui/select.tsx25-57
- Dialog System: Uses Radix Dialog primitives for modals such as ChunkMethodDialog and DatasetCreatingDialog. web/src/components/chunk-method-dialog/index.tsx1-7 web/src/pages/datasets/dataset-creating-dialog.tsx4-10

### 3. Shared Form Field Components

The system utilizes a library of shared form components to ensure consistency across the application, particularly in the Agent and Dataset configuration views.

- `SliderInputFormField`: A reusable component for number inputs with an accompanying slider, supporting both horizontal and vertical layouts. web/src/components/slider-input-form-field.tsx34-137
- `SimilaritySliderFormField`: Manages similarity thresholds and vector/keyword similarity weights. web/src/components/similarity-slider/index.tsx46-148
- `ChunkMethodItem`: Provides a SelectWithSearch for choosing document parsing methods. web/src/pages/dataset/dataset-setting/configuration/common-item.tsx58-97
- `EmbeddingModelItem`: Integrates ModelTreeSelect to allow users to select embedding models, with special handling for edited datasets. web/src/pages/dataset/dataset-setting/configuration/common-item.tsx154-202

**Diagram: Form Field Component Relationships**

```
Shared_Fields

Form_Controls

Data_Validation

formSchema [web/src/pages/dataset/dataset-setting/form-schema.ts]

FormSchema [web/src/components/chunk-method-dialog/index.tsx]

DatasetSettings [web/src/pages/dataset/dataset-setting/index.tsx]

ChunkMethodDialog [web/src/components/chunk-method-dialog/index.tsx]

EmbeddingModelItem [web/src/pages/dataset/dataset-setting/configuration/common-item.tsx]

MaxTokenNumberFormField [web/src/components/max-token-number-from-field.tsx]

LayoutRecognizeFormField [web/src/components/layout-recognize-form-field.tsx]

RaptorFormFields [web/src/components/parse-configuration/raptor-form-fields.tsx]
```

Sources: [web/src/components/slider-input-form-field.tsx34-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/slider-input-form-field.tsx#L34-L137) [web/src/components/similarity-slider/index.tsx46-148](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/similarity-slider/index.tsx#L46-L148) [web/src/pages/dataset/dataset-setting/configuration/common-item.tsx58-202](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/dataset-setting/configuration/common-item.tsx#L58-L202) [web/src/components/chunk-method-dialog/index.tsx76-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/chunk-method-dialog/index.tsx#L76-L165) [web/src/pages/dataset/dataset-setting/form-schema.ts5-132](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/dataset-setting/form-schema.ts#L5-L132)

## Metadata and Dataset Configuration

### 1. Dynamic Parsing Configuration

The `ChunkMethodDialog` serves as the primary interface for configuring how documents are processed. It dynamically renders fields based on the selected `parser_id`.

- Logic: Uses useWatch to monitor the parser_id and toggle visibility for fields like MaxTokenNumberFormField or EntityTypesFormField. web/src/components/chunk-method-dialog/index.tsx182-209
- Integration: Combines multiple sub-forms including RaptorFormFields and GraphRagItems. web/src/pages/dataset/dataset-setting/index.tsx1-3

### 2. Metadata Management System

Metadata management is handled via a dedicated modal system that supports bulk operations and field-level configuration.

- Data Transformation: The util object in use-manage-modal.ts provides functions to convert between raw API metadata and IMetaDataTableData used by the UI. web/src/pages/dataset/components/metedata/hooks/use-manage-modal.ts33-136
- State Management: useMetadataOperations tracks pending updates and deletions before they are committed to the backend via kbUpdateMetaData or updateDocumentsMetadata. web/src/pages/dataset/components/metedata/hooks/use-manage-modal.ts138-231

**Diagram: Metadata Data Flow**

```
User_Interface

UI_State

Transformation_Layer

Backend_Data

IMetaDataReturnType

util.changeToMetaDataTableData [web/src/pages/dataset/components/metedata/hooks/use-manage-modal.ts]

IMetaDataTableData

useMetadataOperations [web/src/pages/dataset/components/metedata/hooks/use-manage-modal.ts]

ManageMetadataModal [web/src/pages/dataset/components/metedata/manage-modal.tsx]
```

Sources: [web/src/pages/dataset/components/metedata/hooks/use-manage-modal.ts33-231](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/components/metedata/hooks/use-manage-modal.ts#L33-L231) [web/src/pages/dataset/components/metedata/manage-modal.tsx52-105](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/components/metedata/manage-modal.tsx#L52-L105) [web/src/components/chunk-method-dialog/index.tsx177-213](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/chunk-method-dialog/index.tsx#L177-L213)

## Floating Chat Widget

The `FloatingChatWidget` provides an embeddable chat interface that can be customized via URL parameters. It demonstrates the use of custom color normalization and CSS generation.

- Customization: Supports widget_accent_color, widget_background_color, and widget_text_color parameters. web/src/components/floating-chat-widget.tsx116-128
- Color Logic: Uses darkenHexColor to derive hover and gradient variants from the primary accent color. web/src/components/floating-chat-widget.tsx30-55
- PDF Integration: Integrates PdfSheet and useClickDrawer for document previewing within the chat interface. web/src/components/floating-chat-widget.tsx2-3 web/src/components/floating-chat-widget.tsx207-213

Sources: [web/src/components/floating-chat-widget.tsx30-136](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/floating-chat-widget.tsx#L30-L136) [web/src/components/floating-chat-widget.tsx207-213](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/floating-chat-widget.tsx#L207-L213)

## Design Tokens and Styling

The UI uses a centralized token system defined via Tailwind CSS. The configuration uses CSS variables to support light and dark modes.

| Token Category | Tailwind Variable | Implementation Example |
| --- | --- | --- |
| **Backgrounds** | `var(--bg-input)` | Used in `Input` and `SelectTrigger` [web/src/components/ui/input.tsx74](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/input.tsx#L74-L74) [web/src/components/ui/select.tsx35](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/select.tsx#L35-L35) |
| **Borders** | `var(--border-button)` | Default border for interactive elements [web/src/components/ui/input.tsx74](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/input.tsx#L74-L74) [web/src/components/ui/select.tsx35](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/select.tsx#L35-L35) |
| **Text Colors** | `var(--text-primary)` | Standard body and input text [web/src/components/ui/input.tsx74](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/input.tsx#L74-L74) |
| **Accents** | `var(--accent-primary)` | Primary brand color for focus states [web/src/components/ui/input.tsx76](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/input.tsx#L76-L76) |

Sources: [web/src/components/ui/input.tsx72-81](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/input.tsx#L72-L81) [web/src/components/ui/select.tsx31-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/components/ui/select.tsx#L31-L41)


---



<!-- ===== 4.3-theming-system ===== -->

# Theming System

Relevant source files
- web/package-lock.json
- web/package.json
- web/public/iconfont.js
- web/src/app.tsx
- web/src/components/icon-font.tsx
- web/src/components/svg-icon.tsx
- web/src/components/ui/badge.tsx
- web/src/components/ui/button.tsx
- web/src/components/ui/radio-group.tsx
- web/src/components/ui/radio.tsx
- web/src/components/ui/segmented.tsx
- web/src/components/ui/table.tsx
- web/src/constants/common.ts
- web/src/constants/llm.ts
- web/src/constants/setting.ts
- web/src/locales/config.ts
- web/src/pages/datasets/dataset-card.tsx
- web/src/pages/home/applications.tsx
- web/src/pages/home/banner.tsx
- web/src/pages/home/datasets.tsx
- web/src/pages/home/index.tsx
- web/src/pages/user-setting/constants.tsx
- web/src/pages/user-setting/index.tsx
- web/src/pages/user-setting/sidebar/index.tsx
- web/src/utils/common-util.ts
- web/tailwind.config.js
- web/tailwind.css

## Purpose and Scope

This document describes RAGFlow's frontend theming system, which provides dark/light mode support through CSS custom properties (CSS variables) integrated with Tailwind CSS. The system enables consistent, themeable user interfaces across the application through a semantic color token system and React-based theme management.

The system is designed to support high-density data views required by RAG operations, such as the visual agent canvas, document parsing configurations, and multi-language chat interfaces.

## Architecture Overview

The theming system consists of three layers: CSS custom properties that define color values, Tailwind CSS configuration that maps these properties to utility classes, and a React context provider that manages theme state.

### System Flow Diagram

"Natural Language Space" to "Code Entity Space" mapping of the theming lifecycle.

```
Application Layer

Runtime Layer

Configuration Layer

Theme Definition Layer

var(--bg-base)

className='bg-bg-base'

toggle 'dark' class

Theme state provider

toggle 'dark' class

Theme Context

selector match

web/tailwind.css
CSS Custom Properties
:root and .dark

ThemeEnum
(web/src/constants/common.ts)
Dark | Light | System

web/tailwind.config.js
module.exports.theme.extend.colors

ThemeProvider
(web/src/app.tsx)

RootProvider
(web/src/app.tsx)

React Components
(e.g., web/src/components/ui/button.tsx)

document.documentElement
class='dark'
```

**Sources:** [web/tailwind.css5-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.css#L5-L135) [web/tailwind.config.js5-184](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L5-L184) [web/src/app.tsx89-94](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L89-L94) [web/src/constants/common.ts210-214](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/constants/common.ts#L210-L214)

## CSS Custom Properties System

RAGFlow defines all theme colors as CSS custom properties in two root-level scopes: `:root` for light mode and `.dark` for dark mode. The system uses a selector-based approach where adding the `dark` class to `document.documentElement` switches all variables to their dark mode values [web/tailwind.config.js6](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L6-L6)

### Variable Scoping Strategy

The theme system defines variables in [web/tailwind.css5-216](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.css#L5-L216):

```
:root {
  /* Light mode variables */
  --background: 0 0% 100%;
  --bg-base: #ffffff;
  --text-primary: 22 22 24;
}
 
.dark {
  /* Dark mode variables */
  --background: rgba(11, 10, 18, 1);
  --bg-base: #ffffff; 
  --text-primary: 246 246 247;
}
```

Variables use different formats depending on usage:

- RGB tuples (e.g., 22 22 24) for colors needing alpha channel support via rgb(var(--text-primary) / <alpha-value>) web/tailwind.config.js81-83
- Hex values (e.g., #ffffff) for solid colors web/tailwind.css94
- HSL tuples (e.g., 0 0% 100%) for HSL-based colors used by standard Radix UI components web/tailwind.css7-34
- RGBA values (e.g., rgba(230, 227, 246, 0.15)) for colors with fixed opacity web/tailwind.css38-42

**Sources:** [web/tailwind.css5-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.css#L5-L135) [web/tailwind.css137-216](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.css#L137-L216) [web/tailwind.config.js75-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L75-L114)

### Semantic Color Token Categories

The system organizes colors into semantic categories to ensure consistency across features like `DatasetCard` and `Datasets` lists [web/src/pages/datasets/dataset-card.tsx16-55](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/datasets/dataset-card.tsx#L16-L55)

| Category | Purpose | Example Variables |
| --- | --- | --- |
| **Background** | Surface colors | `--bg-base`, `--bg-card`, `--bg-component`, `--bg-canvas`, `--bg-list` |
| **Text** | Typography colors | `--text-primary`, `--text-secondary`, `--text-disabled`, `--text-input-tip` |
| **Border** | Border and outline colors | `--border-default`, `--border-accent`, `--border-button` |
| **Accent** | Primary brand color | `--accent-primary`, `--bg-accent` |
| **State** | Status indicators | `--state-success`, `--state-warning`, `--state-error` |
| **Team** | Role colors | `--team-group`, `--team-member`, `--team-department` |

**Sources:** [web/tailwind.css91-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.css#L91-L135) [web/tailwind.config.js70-121](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L70-L121)

## Tailwind CSS Configuration

The Tailwind configuration extends the default theme with custom color mappings that reference CSS custom properties.

### Color Mapping Structure

In [web/tailwind.config.js29-184](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L29-L184) colors are defined to support dynamic transparency:

1. Direct variable reference:

```
background: 'var(--background)',
'text-disabled': 'var(--text-disabled)',
```

1. RGB with alpha support:

```
'text-primary': {
  DEFAULT: 'rgb(var(--text-primary) / <alpha-value>)',
},
```

1. Fixed opacity modifiers:

```
'accent-primary': {
  DEFAULT: 'rgb(var(--accent-primary) / <alpha-value>)',
  5: 'rgba(var(--accent-primary) / 0.05)', 
},
```

**Sources:** [web/tailwind.config.js33-184](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L33-L184)

### Extended Theme Configuration

The configuration extends Tailwind's default theme with custom screens to support the application's responsive layout and high-resolution agent canvas [web/src/app.tsx26-34](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L26-L34)

```
Tailwind Config Extensions

extends

extends

extends

screens:
sm: 640px
...
4xl: 1980px

borderWidth:
0.5: '0.5px'

colors:
semantic mapping to
CSS variables

Tailwind Default Theme
```

Custom screen breakpoints include `3xl: 1780px` and `4xl: 1980px` for ultra-wide displays [web/tailwind.config.js26-27](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L26-L27)

**Sources:** [web/tailwind.config.js20-32](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L20-L32) [web/src/app.tsx26-34](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L26-L34)

## Theme Provider Implementation

The `ThemeProvider` is initialized within the `RootProvider` in `web/src/app.tsx`. It defaults to `ThemeEnum.Dark` and persists the user preference in `localStorage` under the key `ragflow-ui-theme` [web/src/app.tsx89-94](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L89-L94)

### Integration with Component Libraries

RAGFlow utilizes multiple UI libraries that must respect the theme state:

- Radix UI: Used for primitives like Accordion, Dialog, and Select. These use the HSL variables defined in tailwind.css web/package.json42-67
- Ant Design: Components like @ant-design/icons and charts from @antv/g2 are used web/package.json32-34
- Sonner: The toast notification system is configured within Root and follows the theme web/src/app.tsx71

**Sources:** [web/src/app.tsx89-94](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L89-L94) [web/package.json31-68](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/package.json#L31-L68)

## Theme Application in Components

Components apply themed styles using Tailwind utility classes. For complex navigation elements like the `Datasets` list, theming ensures that active states and icons remain consistent.

### Component Theming Example

In [web/src/pages/home/datasets.tsx28-59](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/home/datasets.tsx#L28-L59) the `DatasetCard` and headers use semantic classes:

```
<h2 className="leading-8 text-2xl font-semibold mb-2.5">
  <HomeIcon imgClass="me-2.5" name="datasets" width={24} />
  {t('header.dataset')}
</h2>
```

The text color often defaults to `foreground` which maps to `var(--colors-text-neutral-strong)` [web/tailwind.config.js38](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L38-L38) which switches between light and dark values based on the root HTML class [web/tailwind.css53-198](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.css#L53-L198)

### Navigation Sidebar Theming

The sidebar implementation in `web/src/pages/user-setting/sidebar/index.tsx` demonstrates theme integration through utility classes and `ThemeSwitch`.

```
Theme Controls

SideBar Component (web/src/pages/user-setting/sidebar/index.tsx)

aside class='bg-bg-base'

Button class='text-text-primary'

ThemeSwitch Component

ThemeSwitch (web/src/components/theme-switch.tsx)
```

The sidebar uses `bg-bg-base` [web/src/pages/user-setting/sidebar/index.tsx79](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/user-setting/sidebar/index.tsx#L79-L79) and highlights active items with `bg-bg-card text-text-primary` [web/src/pages/user-setting/sidebar/index.tsx108](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/user-setting/sidebar/index.tsx#L108-L108)

**Sources:** [web/src/pages/user-setting/sidebar/index.tsx79-141](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/user-setting/sidebar/index.tsx#L79-L141) [web/tailwind.config.js81-83](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/tailwind.config.js#L81-L83)

## Package Dependencies

The theming system relies on the following key packages [web/package.json31-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/package.json#L31-L139):

- tailwindcss@^3: Core utility-first CSS framework web/package.json188
- next-themes@^0.4.6: Abstraction for theme management web/package.json99
- class-variance-authority@^0.7.1: Tools for creating themeable component variants web/package.json80
- tailwind-merge@^2.6.1: Utility for resolving Tailwind class conflicts web/package.json129
- lucide-react@^1.7.0: Icon set used in themed navigation and buttons web/package.json98

**Sources:** [web/package.json80-188](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/package.json#L80-L188)


---



<!-- ===== 4.4-page-structure-and-state-management ===== -->

# Page Structure and State Management

Relevant source files
- api/apps/restful_apis/document_api.py
- api/apps/restful_apis/memory_api.py
- api/apps/services/memory_api_service.py
- api/db/joint_services/memory_message_service.py
- api/db/services/common_service.py
- api/db/services/langfuse_service.py
- api/db/services/pipeline_operation_log_service.py
- api/db/services/search_service.py
- conf/mapping.json
- internal/dao/api_token.go
- internal/dao/api_token_test.go
- internal/dao/chat_session.go
- internal/dao/chat_session_test.go
- internal/dao/user_canvas.go
- internal/dao/user_canvas_test.go
- internal/entity/api_token.go
- internal/entity/canvas.go
- internal/handler/agent.go
- internal/handler/agent_test.go
- internal/handler/agent_upload_test.go
- internal/service/agent.go
- internal/service/agent_test.go
- internal/service/file_test.go
- memory/services/messages.py
- memory/utils/es_conn.py
- memory/utils/infinity_conn.py
- test/testcases/test_http_api/test_file_management_within_dataset/test_doc_sdk_routes_unit.py
- test/testcases/test_web_api/test_common.py
- test/testcases/test_web_api/test_document_app/test_document_metadata.py
- web/src/hooks/use-document-request.ts
- web/src/routes.tsx
- web/src/services/knowledge-service.ts
- web/src/utils/api.ts
- web/src/utils/authorization-util.ts
- web/src/wrappers/auth.tsx
- web/vite.config.ts

This document describes the frontend application's page structure, routing configuration, and state management architecture. It covers the provider hierarchy, routing system using React Router v7, state management split between React Query for server state and Zustand for client state, and patterns for page component composition.

## Application Entry Point and Provider Hierarchy

The application entry point is defined in `web/src/app.tsx`, where the `AppContainer` component initializes the provider hierarchy and routing system [web/src/app.tsx107-113](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L107-L113)

### Provider Stack

The application uses a nested provider pattern to configure global concerns:

**Diagram: Frontend Provider Hierarchy**

```
AppContainer

RootProvider

TooltipProvider

QueryClientProvider
(React Query)

ThemeProvider
(dark/light mode)

Root Component

Sonner Toaster

Radix Toaster

RouterProviderWrapper

RouterProvider
(React Router v7)
```

**Sources:** [web/src/app.tsx78-98](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L78-L98) [web/src/app.tsx107-113](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L107-L113)

The provider hierarchy is established in `RootProvider` [web/src/app.tsx78-98](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L78-L98) and `Root` [web/src/app.tsx66-76](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L66-L76) with the following structure:

| Provider | Purpose | Configuration |
| --- | --- | --- |
| `TooltipProvider` | Provides tooltip context for Radix UI components | [web/src/app.tsx87](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L87-L87) |
| `QueryClientProvider` | React Query client for server state management | Configured at [web/src/app.tsx57-64](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L57-L64) with `refetchOnWindowFocus: false` and `retry: 2` |
| `ThemeProvider` | Manages dark/light theme state using CSS variables | Storage key: `"ragflow-ui-theme"`, default: `ThemeEnum.Dark` [web/src/app.tsx89-92](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L89-L92) |
| `RouterProvider` | React Router v7 routing engine | Router instance `routers` from [web/src/routes.tsx](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/routes.tsx) |

### Global Initialization and Locale

Language detection and synchronization occurs in the `RootProvider` component [web/src/app.tsx79-84](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L79-L84) It retrieves the language from storage and triggers `changeLanguageAsync` [web/src/app.tsx82](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L82-L82)

- LocalStorage: Persistence via storage.setLanguage web/src/locales/config.ts109
- Document Metadata: Updates document.documentElement.lang and direction web/src/locales/config.ts54-58
- Day.js: Synchronizes the date library locale web/src/locales/config.ts57 The system supports 15+ languages including English, Chinese, Russian, and Arabic web/src/locales/config.ts14-30

**Sources:** [web/src/app.tsx78-98](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L78-L98) [web/src/locales/config.ts54-58](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/config.ts#L54-L58) [web/src/utils/authorization-util.ts1-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/authorization-util.ts#L1-L50)

## Routing System

The application uses React Router v7 for client-side routing. The router configuration is defined in `web/src/routes.tsx` and provided via `RouterProviderWrapper` [web/src/app.tsx100-104](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L100-L104)

### Route Structure

The application defines a comprehensive set of routes including datasets, agents, and chat interfaces using the `Routes` enum [web/src/routes.tsx12-77](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/routes.tsx#L12-L77)

**Diagram: Core Application Routes**

```
Routes.Root (/)

Routes.Datasets (/datasets)

Routes.DatasetBase (/dataset)

Routes.Chats (/chats)

Routes.Chat (/chat)

Routes.Agents (/agents)

Routes.Memories (/memories)

Routes.Files (/files)

Routes.DatasetTesting (/retrieval)
```

**Sources:** [web/src/routes.tsx12-77](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/routes.tsx#L12-L77)

Key routing features include:

- Lazy Loading: Components are loaded on demand via withLazyRoute to optimize initial bundle size web/src/routes.tsx90-106
- Authentication Wrapper: The auth search parameter is handled in the root loader to set authorization tokens via authorizationUtil.setAuthorization web/src/routes.tsx152-161
- Vite Proxy: Routing is supported by a Vite proxy configuration that handles python, go, or hybrid backend targets web/vite.config.ts59-134

## State Management Architecture

RAGFlow uses a hybrid state management approach, separating server-side data from client-side UI state.

**Diagram: State Management Data Flow**

```
Data Fetching Layer

Server State (React Query)

Client State (Hooks & Router)

useGetPaginationWithRouter

useHandleSearchChange

useGetKnowledgeSearchParams

QueryClient

useFetchDocumentList

useUploadDocument

webAPI / restAPIv1 (utils/api)

request (utils/request)

kbService (services/knowledge-service)
```

**Sources:** [web/src/hooks/use-document-request.ts117-215](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/hooks/use-document-request.ts#L117-L215) [web/src/services/knowledge-service.ts217-220](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L217-L220) [web/src/utils/api.ts1-4](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L1-L4) [web/src/app.tsx57-64](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L57-L64)

### Server State with React Query

React Query manages the lifecycle of server data. The `QueryClient` is configured with a default retry limit of 2 and `refetchOnWindowFocus` disabled [web/src/app.tsx57-64](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/app.tsx#L57-L64)

Example usage:

- Document List: useFetchDocumentList manages paginated documents and search filters, with a polling mechanism (refetchInterval) that activates when documents are in a RunningStatus.RUNNING state web/src/hooks/use-document-request.ts128-143
- Document Upload: useUploadDocument uses useMutation to handle multipart file uploads and invalidates the document list query upon success web/src/hooks/use-document-request.ts62-106
- Knowledge Base Details: getKbDetail fetches dataset information and normalizes legacy field names like chunk_num for the UI web/src/services/knowledge-service.ts222-235

### Client State and Persistence

Client-side state is managed through specialized hooks and global providers.

- Pagination Sync: useGetPaginationWithRouter synchronizes pagination state with URL search parameters web/src/hooks/use-document-request.ts120
- Search Debouncing: Search inputs are handled via useHandleSearchChange and debounced using useDebounce to prevent excessive API calls web/src/hooks/use-document-request.ts119-123
- Theme: ThemeProvider persists the user's choice to localStorage under ragflow-ui-theme web/src/app.tsx89-92

## Component and Page Structure

### Service Layer Implementation

The frontend communicates with the backend through a mapping of API endpoints to service functions defined in `web/src/utils/api.ts`.

**Diagram: Natural Language Space to Code Entity Space**

| System Concept | Frontend Code Entity | Backend Route / Service |
| --- | --- | --- |
| **Document Upload** | `uploadDocument` [web/src/services/knowledge-service.ts27](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L27-L27) | `POST /api/v1/datasets/<id>/documents/upload` [api/apps/restful_apis/document_api.py59](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L59-L59) |
| **Document List** | `listDocument` [web/src/services/knowledge-service.ts24](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L24-L24) | `GET /api/v1/datasets/<id>/documents` [web/src/utils/api.ts160](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L160-L160) |
| **Chunk Management** | `chunkService` [web/src/services/knowledge-service.ts121](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L121-L121) | `/api/v1/datasets/<ds_id>/documents/<doc_id>/chunks` [web/src/utils/api.ts153](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L153-L153) |
| **Agent Canvas** | `AgentHandler` [internal/handler/agent.go46](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/handler/agent.go#L46-L46) | `/api/v1/agents` [internal/handler/agent.go73](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/handler/agent.go#L73-L73) |
| **Memory System** | `MEMORY_API_URL` [test/testcases/test_web_api/test_common.py33](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_web_api/test_common.py#L33-L33) | `/api/v1/memories` |

### Data Normalization Layer

The frontend includes a normalization layer in `knowledge-service.ts` to bridge the gap between different API versions and backend implementations (Python vs Go).

- Chunk Mapping: mapChunkToLegacy ensures that chunk objects have consistent fields like chunk_id and doc_id regardless of whether the backend returns id or document_id web/src/services/knowledge-service.ts79-90
- Document Mapping: mapDocumentToLegacy normalizes parser and count fields web/src/services/knowledge-service.ts92-97
- Payload Mapping: mapChunkPayloadToRest converts UI-friendly boolean fields back to the integer or string formats expected by specific API endpoints web/src/services/knowledge-service.ts99-112

### Backend Integration (Go Handler)

For the Go-based backend services, the `AgentHandler` manages agent canvases and versions [internal/handler/agent.go46](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/handler/agent.go#L46-L46)

- Error Mapping: mapAgentError normalizes Go service errors into standard frontend error codes like common.CodeOperatingError (103) to maintain consistency with the Python API internal/handler/agent.go153-165
- Context Handling: GetUser is used across Go handlers to extract authenticated user information from the Gin context internal/handler/agent.go75

**Sources:** [web/src/services/knowledge-service.ts73-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L73-L112) [web/src/hooks/use-document-request.ts1-215](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/hooks/use-document-request.ts#L1-L215) [internal/handler/agent.go153-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/handler/agent.go#L153-L165) [api/apps/restful_apis/document_api.py122-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L122-L173)


---



<!-- ===== 5-llm-integration-system ===== -->

# LLM Integration System

Relevant source files
- api/apps/llm_app.py
- api/db/init_data.py
- api/db/services/llm_service.py
- conf/llm_factories.json
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- web/src/pages/user-setting/setting-model/constant.ts

The LLM Integration System provides a unified abstraction layer for interacting with multiple commercial and open-source AI providers. It manages tenant-specific configurations, model selection, and implements cross-cutting concerns including error handling, retry logic, usage tracking, and model-specific behavior policies. The system supports seven model types: chat, embedding, rerank, vision (image-to-text), text-to-speech (TTS), speech-to-text (ASR), and OCR.

For details on the specific components of this system, see the child pages:

- LLMBundle and Model Types — Document the LLMBundle abstraction, model type system (CHAT, EMBEDDING, RERANK, TTS, etc.), and factory pattern implementation.
- Provider Implementations — Detail the concrete provider implementations (OpenAI, Anthropic, DeepSeek, local models) and how they register with the factory, including Go-side model definitions.
- Error Handling and Retry Logic — Explain the sophisticated error classification system, retry strategies with exponential backoff, and error categorization.
- Tenant Configuration and Model Management — Document how tenants configure their own models, the relationship between tenant_llm table and model selection, and the initialization process.
- Tool Calling and Function Use — Explain the tool binding mechanism, function schema generation, and how LLMs invoke external tools during chat completion.

## Architecture Overview

The LLM integration system follows a layered architecture that bridges high-level tenant requests to low-level provider APIs.

**System Architecture: LLM Integration Layers**

```
Provider_Implementations

Model_Registries

Service_Layer

API_Layer

Configuration_Layer

conf/llm_factories.json

api/db/db_models.py:TenantLLM

POST /set_api_key
api/apps/llm_app.py:77

GET /factories
api/apps/llm_app.py:49

class LLMService
api/db/services/llm_service.py:32

class LLMBundle
api/db/services/llm_service.py:85

class TenantLLMService
api/db/services/tenant_llm_service.py

ChatModel dict
rag/llm/init.py:142

EmbeddingModel dict
rag/llm/init.py:144

RerankModel dict
rag/llm/init.py:145

class Base
rag/llm/chat_model.py:161

class Base
rag/llm/embedding_model.py:138

class Base
rag/llm/rerank_model.py:30
```

**Architecture Description**: Configuration originates from `conf/llm_factories.json` [conf/llm_factories.json1-2](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L1-L2) The **API Layer** in `api/apps/llm_app.py` handles tenant model setup, including verifying API keys through live tests for embedding [api/apps/llm_app.py97-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L97-L109) chat [api/apps/llm_app.py110-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L110-L130) and rerank models [api/apps/llm_app.py131-144](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L131-L144) The **Service Layer** uses the `LLMBundle` class [api/db/services/llm_service.py85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L85-L85) to wrap model instances. **Model Registries** are dynamically populated in `rag/llm/__init__.py` [rag/llm/__init__.py142-149](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L142-L149) by scanning provider modules listed in `MODULE_MAPPING` [rag/llm/__init__.py152-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L152-L161) for classes inheriting from the respective `Base` class [rag/llm/__init__.py171-174](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L171-L174)

**Sources**: [api/apps/llm_app.py77-144](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L77-L144) [rag/llm/__init__.py142-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L142-L161) [rag/llm/chat_model.py161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L161-L161) [rag/llm/embedding_model.py138](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/embedding_model.py#L138-L138) [rag/llm/rerank_model.py30](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L30-L30) [conf/llm_factories.json1-2](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L1-L2) [api/db/services/llm_service.py32-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L32-L85)

## Model Type System

RAGFlow categorizes AI capabilities into specific types, each defined by a base interface and a registry.

| Model Type | Registry | Base Class | Purpose |
| --- | --- | --- | --- |
| **CHAT** | `ChatModel` | `rag/llm/chat_model.py` | Text generation and tool use [rag/llm/chat_model.py161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L161-L161) |
| **EMBEDDING** | `EmbeddingModel` | `rag/llm/embedding_model.py` | Vector encoding for search [rag/llm/embedding_model.py138](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/embedding_model.py#L138-L138) |
| **RERANK** | `RerankModel` | `rag/llm/rerank_model.py` | Scoring document relevance [rag/llm/rerank_model.py30](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L30-L30) |
| **IMAGE2TEXT** | `CvModel` | `rag/llm/cv_model.py` | Vision and image description [rag/llm/cv_model.py59](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/cv_model.py#L59-L59) |
| **TTS** | `TTSModel` | `rag/llm/tts_model.py` | Text-to-speech synthesis [rag/llm/tts_model.py68](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/tts_model.py#L68-L68) |
| **SPEECH2TEXT** | `Seq2txtModel` | `rag/llm/sequence2txt_model.py` | Audio transcription [rag/llm/sequence2txt_model.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/sequence2txt_model.py#L32-L32) |
| **OCR** | `OcrModel` | `rag/llm/__init__.py` | Optical character recognition [rag/llm/__init__.py148](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L148-L148) |

For details on the abstraction and factory pattern, see [LLMBundle and Model Types](/infiniflow/ragflow/5.1-llmbundle-and-model-types).

**Sources**: [rag/llm/__init__.py142-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L142-L159) [rag/llm/chat_model.py161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L161-L161) [rag/llm/embedding_model.py138](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/embedding_model.py#L138-L138) [rag/llm/rerank_model.py30](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L30-L30) [rag/llm/cv_model.py59](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/cv_model.py#L59-L59) [rag/llm/tts_model.py68](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/tts_model.py#L68-L68) [rag/llm/sequence2txt_model.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/sequence2txt_model.py#L32-L32)

## Provider Implementations

The system supports a wide range of providers, from commercial APIs (OpenAI, Anthropic, DeepSeek) to local deployments (Ollama, Xinference).

**Provider Registration Sequence**

```
ChatModel Registry
chat_model.py
rag/llm/__init__.py
ChatModel Registry
chat_model.py
rag/llm/__init__.py
MODULE_MAPPING loop
rag/llm/__init__.py:165
alt
[is valid provider]
loop
[for each class in members]
importlib.import_module()
module members
check if subclass of Base
rag/llm/__init__.py:171
check for _FACTORY_NAME
Register class under factory name
```

Registration is performed dynamically at module load time by inspecting module members for classes that define a `_FACTORY_NAME` [rag/llm/__init__.py165-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L165-L181) Providers define this name (e.g., `"OpenAI"` [rag/llm/sequence2txt_model.py54](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/sequence2txt_model.py#L54-L54) or `"Jina"` [rag/llm/rerank_model.py95](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L95-L95)) which the system uses for routing. Supported LiteLLM providers are enumerated in `SupportedLiteLLMProvider` [rag/llm/__init__.py25-65](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L25-L65)

For details on concrete implementations, see [Provider Implementations](/infiniflow/ragflow/5.2-provider-implementations).

**Sources**: [rag/llm/__init__.py25-65](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L25-L65) [rag/llm/__init__.py165-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L165-L181) [rag/llm/sequence2txt_model.py54](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/sequence2txt_model.py#L54-L54) [rag/llm/rerank_model.py95](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L95-L95)

## Error Handling and Retries

RAGFlow implements a robust error classification and retry system to handle transient API failures.

- Error Classification: The Base._classify_error method rag/llm/chat_model.py178-212 uses keyword mapping to categorize raw exceptions into LLMErrorCode enums such as RATE_LIMIT_EXCEEDED, AUTH_ERROR, or QUOTA_EXCEEDED rag/llm/chat_model.py42-54
- Retry Logic: Transient errors trigger retries with delays calculated via _get_delay() rag/llm/chat_model.py175-176 Retries are capped by max_retries, which defaults to the environment variable LLM_MAX_RETRIES rag/llm/chat_model.py168
- Model Policies: Specific model families (e.g., Qwen3, Kimi-k2.5) have specialized parameter handling via _apply_model_family_policies to manage features like "thinking" or temperature constraints rag/llm/chat_model.py108-158

For details on error categorization and retry strategies, see [Error Handling and Retry Logic](/infiniflow/ragflow/5.3-error-handling-and-retry-logic).

**Sources**: [rag/llm/chat_model.py42-54](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L42-L54) [rag/llm/chat_model.py168](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L168-L168) [rag/llm/chat_model.py175-176](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L175-L176) [rag/llm/chat_model.py178-212](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L178-L212) [rag/llm/chat_model.py108-158](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L108-L158)

## Tenant and Model Management

Tenants configure their own API keys and base URLs, which are stored in the `TenantLLM` table [api/apps/llm_app.py27](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L27-L27)

- Validation: When a tenant adds a model via /set_api_key api/apps/llm_app.py77 the system performs live validation by attempting a small operation (e.g., mdl.encode for embedding api/apps/llm_app.py101-104 or a stream chat for chat models api/apps/llm_app.py114-124).
- Initialization: Default models for superusers are initialized during system setup in init_superuser api/db/init_data.py48-95 ensuring the environment is ready for use immediately after deployment.
- Model Selection: The system resolves model availability for tools and types through LLMService and TenantLLMService api/apps/llm_app.py30-44

For details on how tenants manage their models, see [Tenant Configuration and Model Management](/infiniflow/ragflow/5.4-tenant-configuration-and-model-management).

**Sources**: [api/apps/llm_app.py77-156](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L77-L156) [api/db/init_data.py48-95](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/init_data.py#L48-L95) [api/db/db_models.py27](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L27-L27) [api/apps/llm_app.py30-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L30-L44)

## Tool Calling

Chat models that support tool calling (indicated by `is_tools: true` in `llm_factories.json` [conf/llm_factories.json16](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L16-L16)) can invoke external functions.

- State Management: The Base chat class maintains toolcall_sessions rag/llm/chat_model.py173 to track active function calling contexts and tool definitions rag/llm/chat_model.py172
- Tool Binding: LLMBundle.bind_tools delegates the registration of tools to the underlying model implementation api/db/services/llm_service.py107-111
- Tool Resolution: The system checks if a model supports tools by resolving its is_tools attribute from the database or factory configuration api/apps/llm_app.py30-44

For details on function schema and execution, see [Tool Calling and Function Use](/infiniflow/ragflow/5.5-tool-calling-and-function-use).

**Sources**: [conf/llm_factories.json16](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L16-L16) [rag/llm/chat_model.py172-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L172-L173) [api/db/services/llm_service.py107-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L107-L111) [api/apps/llm_app.py30-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L30-L44)


---



<!-- ===== 5.1-llmbundle-and-model-types ===== -->

# LLMBundle and Model Types

Relevant source files
- api/apps/llm_app.py
- api/db/init_data.py
- api/db/services/llm_service.py
- conf/llm_factories.json
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- web/src/pages/user-setting/setting-model/constant.ts

This page documents the `LLMBundle` wrapper class, which provides a unified interface for interacting with language models in RAGFlow. `LLMBundle` adds cross-cutting concerns like usage tracking and observability to the underlying model implementations, while supporting seven distinct model types for different AI tasks.

For information about specific provider implementations, see [Provider Implementations](/infiniflow/ragflow/5.2-provider-implementations). For tenant configuration management, see [Tenant Configuration and Model Management](/infiniflow/ragflow/5.4-tenant-configuration-and-model-management).

## Overview

`LLMBundle` is a wrapper class defined in [api/db/services/llm_service.py85-456](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L85-L456) that provides a unified interface for all LLM operations in RAGFlow. It inherits from `LLM4Tenant` ([api/db/services/tenant_llm_service.py27-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/tenant_llm_service.py#L27-L82)), which handles model instantiation based on tenant-specific configuration.

**Core Functions:**

1. Usage Tracking: Records token consumption via TenantLLMService.increase_usage_by_id() api/db/services/llm_service.py142-143
2. Observability: Optional Langfuse integration for tracing model calls via self.langfuse.start_observation api/db/services/llm_service.py89-92
3. Unified Interface: Consistent method signatures across providers and model types (e.g., encode, similarity, chat).
4. Tenant Isolation: Per-tenant API keys and model configurations managed through the TenantLLM table api/db/db_models.py27

**LLMBundle Data Flow and Entity Mapping**

Title: "LLMBundle Data Flow and Entity Mapping"

```
Code_Entity_Space_(RAGFlow_Backend)

Natural_Language_Space_(User/System_Interaction)

Database_Tables_[api/db/db_models.py]

Model_Factories_[rag/llm/init.py]

inherits

queries

lookups

lookups

calls

updates used_tokens

init_llm_factory()

User Query / Document Text

Admin Model Configuration

LLMBundle class
[api/db/services/llm_service.py:85]

LLM4Tenant class
[api/db/services/tenant_llm_service.py:27]

TenantLLMService
[api/db/services/tenant_llm_service.py:16]

ChatModel dict

EmbeddingModel dict

RerankModel dict

TenantLLM table

LLM table
```

Sources: [api/db/services/llm_service.py85-456](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L85-L456) [api/db/services/tenant_llm_service.py27-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/tenant_llm_service.py#L27-L82) [rag/llm/__init__.py141-147](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L141-L147) [api/db/db_models.py27](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L27-L27)

## Model Types

RAGFlow supports seven model types, each defined as a Python dictionary in [rag/llm/__init__.py141-147](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L141-L147) and mapped to a `LLMType` enum value in [common/constants.py26](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/constants.py#L26-L26) Each type has its own base class and interface contract.

| Model Type | Global Dict | LLMType Enum | Purpose | Base Class Module |
| --- | --- | --- | --- | --- |
| **Chat** | `ChatModel` | `LLMType.CHAT` | Text generation and conversation | [rag/llm/chat_model.py161-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L161-L173) |
| **Embedding** | `EmbeddingModel` | `LLMType.EMBEDDING` | Text to vector representations | [rag/llm/embedding_model.py138-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/embedding_model.py#L138-L152) |
| **Rerank** | `RerankModel` | `LLMType.RERANK` | Document relevance scoring | [rag/llm/rerank_model.py30-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L30-L35) |
| **Vision (CV)** | `CvModel` | `LLMType.IMAGE2TEXT` | Image description and extraction | [rag/llm/cv_model.py59-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/cv_model.py#L59-L69) |
| **Speech2Text** | `Seq2txtModel` | `LLMType.SPEECH2TEXT` | Audio transcription | [rag/llm/sequence2txt_model.py32-38](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/sequence2txt_model.py#L32-L38) |
| **TTS** | `TTSModel` | `LLMType.TTS` | Text-to-speech synthesis | [rag/llm/tts_model.py68-81](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/tts_model.py#L68-L81) |
| **OCR** | `OcrModel` | `LLMType.OCR` | Visual text recognition | [rag/llm/__init__.py148](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L148-L148) |

**Dynamic Registration Process:**

Providers register themselves by defining a `_FACTORY_NAME` class attribute. The registration loop in [rag/llm/__init__.py165-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L165-L181) automatically discovers all classes inheriting from `Base` that have this attribute and adds them to the appropriate dictionary.

```
# From rag/llm/__init__.py:172-181
elif name == "LiteLLMBase":
    lite_llm_base_class = obj
    assert hasattr(obj, "_FACTORY_NAME"), "LiteLLMbase should have _FACTORY_NAME field."
    if hasattr(obj, "_FACTORY_NAME"):
        if isinstance(obj._FACTORY_NAME, list):
            for factory_name in obj._FACTORY_NAME:
                mapping_dict[factory_name] = obj
        else:
            mapping_dict[obj._FACTORY_NAME] = obj
```

Sources: [rag/llm/__init__.py141-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L141-L181) [rag/llm/chat_model.py161-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L161-L173) [rag/llm/embedding_model.py138-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/embedding_model.py#L138-L152) [rag/llm/rerank_model.py30-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L30-L50) [rag/llm/cv_model.py59-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/cv_model.py#L59-L69) [rag/llm/sequence2txt_model.py32-38](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/sequence2txt_model.py#L32-L38) [rag/llm/tts_model.py68-81](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/tts_model.py#L68-L81)

## LLMBundle Architecture

`LLMBundle` wraps provider implementations stored in `self.mdl` with method delegation, token usage tracking via `TenantLLMService.increase_usage_by_id()`, and optional Langfuse tracing.

**Class Hierarchy and Instantiation Flow**

Title: "LLMBundle Implementation Hierarchy"

```

```

**Initialization Code** ([api/db/services/llm_service.py86-88](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L86-L88)):

```
class LLMBundle(LLM4Tenant):
    def __init__(self, tenant_id: str, model_config: dict, lang="Chinese", **kwargs):
        super().__init__(tenant_id, model_config, lang, **kwargs)
        # self.mdl is instantiated in the parent LLM4Tenant class
```

The parent `LLM4Tenant.__init__()` performs model lookup using the factory dictionaries (e.g., `ChatModel[factory]`) and instantiates the concrete provider class with the tenant's API key and base URL [api/db/services/tenant_llm_service.py27-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/tenant_llm_service.py#L27-L82)

Sources: [api/db/services/llm_service.py85-100](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L85-L100) [api/db/services/tenant_llm_service.py27-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/tenant_llm_service.py#L27-L82) [rag/llm/chat_model.py161-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L161-L173)

## Usage Tracking

`LLMBundle` automatically tracks token consumption for all model interactions. After each API call, the wrapper extracts the token count from the provider's response and persists it to the database using `TenantLLMService.increase_usage_by_id()` [api/db/services/llm_service.py142-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L142-L143)

**Implementation Examples:**

**Embedding Model** ([api/db/services/llm_service.py113-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L113-L153)):

```
def encode(self, texts: list):
    # ... input validation and truncation ...
    embeddings, used_tokens = self.mdl.encode(safe_texts)
    
    if self.model_config["llm_factory"] == "Builtin":
        logging.debug("LLMBundle.encode query: {}, emd len: {}, used_tokens: {}. Builtin model don't need to update token usage".format(texts, len(embeddings), used_tokens))
    else:
        logging.info("LLMBundle.encode used_tokens: {}, llm_name: {}".format(used_tokens, self.model_config["llm_name"]))
        # Usage increment logic...
```

**Rerank Model** ([api/db/services/llm_service.py177-191](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L177-L191)):

```
def similarity(self, query: str, texts: list):
    # ... Langfuse start ...
    sim, used_tokens = self.mdl.similarity(query, texts)
    # Token usage logging
    if self.model_config["llm_factory"] != "Builtin":
        logging.info("LLMBundle.similarity used_tokens: {}, llm_name: {}".format(used_tokens, self.model_config["llm_name"]))
    # ... Langfuse end ...
    return sim, used_tokens
```

Sources: [api/db/services/llm_service.py113-191](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L113-L191) [api/db/services/tenant_llm_service.py27-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/tenant_llm_service.py#L27-L82)

## Factory Configuration and Database Schema

Provider metadata is defined in [conf/llm_factories.json](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json) and loaded into database tables during system initialization. This configuration drives both the provider class lookup and the UI model selection interface.

### llm_factories.json Structure

The JSON file contains a `factory_llm_infos` array where each entry represents one provider factory with its supported models.

**Model Entry Schema** ([conf/llm_factories.json11-16](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L11-L16)):

| Field | Type | Purpose | Example |
| --- | --- | --- | --- |
| `llm_name` | `string` | Model identifier passed to provider API | `"gpt-5.2-pro"` |
| `tags` | `string` | Model-specific capabilities | `"LLM,CHAT,400k,IMAGE2TEXT"` |
| `max_tokens` | `int` | Maximum context window size | `400000` |
| `model_type` | `string` | Task type (chat, embedding, etc.) | `"chat"` |
| `is_tools` | `bool` | Support for function/tool calling | `true` |

### Database Initialization Process

The system initialization in `init_llm_factory` [api/db/init_data.py109-126](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/init_data.py#L109-L126) loads the JSON configuration into MySQL tables:

1. Clear Old Data: The LLMFactoriesService.filter_delete([1 == 1]) clears existing records.
2. Load JSON: Iterates through factory information defined in settings.FACTORY_LLM_INFOS.
3. Insert Factories: Saves provider info to the LLMFactories table.
4. Insert Models: Saves model definitions to the LLM table with a reference to the factory ID (fid).

**API Usage** ([api/apps/llm_app.py47-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L47-L69)): The `/factories` endpoint queries the `LLMService` to return available model types for each factory, which the frontend uses to render the model configuration UI.

Sources: [conf/llm_factories.json1-192](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L1-L192) [api/db/init_data.py109-126](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/init_data.py#L109-L126) [api/apps/llm_app.py47-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L47-L69)

## Model Type Interfaces

Each model type defines specific methods in `LLMBundle` that wrap provider implementations with usage tracking and observability.

### Chat Models

- Methods: async_chat, async_chat_streamly api/db/services/llm_service.py328-461
- Tool Support: bind_tools delegates to the underlying model if self.is_tools is true api/db/services/llm_service.py107-111
- Policies: _apply_model_family_policies handles model-specific tweaks (e.g., disabling thinking for Qwen3 family) rag/llm/chat_model.py108-158

### Embedding Models

- Methods: encode (batch), encode_queries (single) api/db/services/llm_service.py113-175
- Truncation: LLMBundle automatically handles empty inputs and truncates text exceeding self.max_length before calling the provider api/db/services/llm_service.py126-141

### Rerank Models

- Methods: similarity api/db/services/llm_service.py177-191
- Normalization: Base class provides _normalize_rank to scale scores to [0, 1] rag/llm/rerank_model.py67-91

### Vision (CV) Models

- Methods: describe, describe_with_prompt api/db/services/llm_service.py193-218
- Normalization: _normalize_image handles conversion from bytes, base64, or URLs to data URLs rag/llm/cv_model.py105-141

### Speech2Text / TTS

- S2T: transcription handles audio file input and returns text with token counts rag/llm/sequence2txt_model.py40-43
- TTS: tts method includes text normalization to remove markdown artifacts before synthesis rag/llm/tts_model.py79-81

Sources: [api/db/services/llm_service.py85-461](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L85-L461) [rag/llm/chat_model.py108-158](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L108-L158) [rag/llm/rerank_model.py67-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L67-L91) [rag/llm/cv_model.py105-141](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/cv_model.py#L105-L141) [rag/llm/tts_model.py79-81](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/tts_model.py#L79-L81)


---



<!-- ===== 5.2-provider-implementations ===== -->

# Provider Implementations

Relevant source files
- agent/component/browser.py
- api/apps/llm_app.py
- api/apps/restful_apis/provider_api.py
- api/apps/services/models_api_service.py
- api/apps/services/provider_api_service.py
- api/db/init_data.py
- api/db/joint_services/tenant_model_service.py
- api/db/services/llm_service.py
- common/data_source/rdbms_connector.py
- conf/llm_factories.json
- conf/models/cohere.json
- conf/models/fishaudio.json
- conf/models/nvidia.json
- conf/models/openai.json
- conf/models/openrouter.json
- conf/models/replicate.json
- conf/models/volcengine.json
- conf/models/xai.json
- deepdoc/parser/txt_parser.py
- internal/entity/models/aliyun.go
- internal/entity/models/baidu.go
- internal/entity/models/deepseek.go
- internal/entity/models/dummy.go
- internal/entity/models/factory.go
- internal/entity/models/gitee.go
- internal/entity/models/huggingface.go
- internal/entity/models/lmstudio.go
- internal/entity/models/minimax.go
- internal/entity/models/moonshot.go
- internal/entity/models/nvidia.go
- internal/entity/models/nvidia_rerank_test.go
- internal/entity/models/ollama.go
- internal/entity/models/openai.go
- internal/entity/models/openai_test.go
- internal/entity/models/openrouter.go
- internal/entity/models/openrouter_test.go
- internal/entity/models/siliconflow.go
- internal/entity/models/types.go
- internal/entity/models/vllm.go
- internal/entity/models/volcengine.go
- internal/entity/models/volcengine_test.go
- internal/entity/models/xai_test.go
- internal/entity/models/zhipu-ai.go
- internal/handler/providers.go
- internal/service/model_service.go
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/key_utils.py
- rag/llm/model_meta.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- test/testcases/test_web_api/test_llm_app/test_llm_list_unit.py
- test/unit_test/agent/component/test_browser_use_component.py
- test/unit_test/rag/llm/test_localai_model_list.py
- test/unit_test/rag/test_sync_data_source.py
- web/src/pages/user-setting/setting-model/constant.ts

This document describes the dynamic factory pattern used to register and instantiate LLM providers in RAGFlow. It covers the provider discovery mechanism, the `llm_factories.json` registry structure, and the implementation patterns used across 60+ supported AI providers. For information about the unified LLMBundle interface and model types, see [LLMBundle and Model Types](https://github.com/infiniflow/ragflow/blob/d32e05d5/LLMBundle and Model Types) For tenant-specific configuration and API key management, see [Tenant Configuration and Model Management](https://github.com/infiniflow/ragflow/blob/d32e05d5/Tenant Configuration and Model Management)

## Factory Pattern and Dynamic Registration

RAGFlow uses a dynamic factory pattern that automatically discovers and registers provider implementations at module load time. This eliminates the need for manual registration and allows new providers to be added simply by implementing a class with the `_FACTORY_NAME` attribute.

### Module Discovery Process

The following diagram illustrates how the system bridges the "Natural Language Space" of provider names to the "Code Entity Space" of Python classes during initialization.

Title: Provider Registration and Discovery Flow

```
Code Entity Space: Model Dictionaries

Registration Logic

Introspection [rag/llm/init.py:171-185]

Initialization [rag/llm/init.py]

init.py:165-167
Iterate MODULE_MAPPING

MODULE_MAPPING
{chat_model, cv_model,
embedding_model, etc.}

importlib.import_module()

inspect.getmembers()

Find Base class [ABC]

Find LiteLLMBase class

Check _FACTORY_NAME
attribute

Update mapping dicts:
ChatModel, EmbeddingModel, etc.

ChatModel [dict]
'OpenAI' -> OpenAI implementation
'DeepSeek' -> LiteLLMBase

EmbeddingModel [dict]
'OpenAI' -> OpenAIEmbed
'Builtin' -> BuiltinEmbed

RerankModel, CvModel,
TTSModel, Seq2txtModel,
OcrModel
```

Sources: [rag/llm/__init__.py152-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L152-L161) [rag/llm/__init__.py165-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L165-L185)

The registration process occurs in [rag/llm/__init__.py165-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L165-L185) and follows these steps:

1. Module iteration: For each module name in MODULE_MAPPING (chat_model, embedding_model, etc.), the full module is imported using importlib.import_module(). rag/llm/__init__.py165-167
2. Class introspection: inspect.getmembers() retrieves all classes from the module. rag/llm/__init__.py171
3. Base class identification: The system identifies Base and LiteLLMBase classes if present. rag/llm/__init__.py173-176
4. Provider registration: For each class that inherits from Base or is LiteLLMBase, has a _FACTORY_NAME attribute, and is not the base class itself, the class is registered in the corresponding dictionary using the factory name as the key. rag/llm/__init__.py179-185

### Factory Name Attribute

Each provider implementation declares its factory name using the `_FACTORY_NAME` class attribute. If `_FACTORY_NAME` is a list, the class is registered under all specified names. [rag/llm/__init__.py179-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L179-L181)

Sources: [rag/llm/rerank_model.py95](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L95-L95) [rag/llm/rerank_model.py118](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L118-L118) [rag/llm/rerank_model.py150](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L150-L150)

### Model Type Dictionaries

The registration process creates global dictionaries mapping factory names to implementation classes:

| Dictionary | Module | Purpose |
| --- | --- | --- |
| `ChatModel` | `chat_model.py` | Conversational AI models |
| `CvModel` | `cv_model.py` | Vision/image-to-text models |
| `EmbeddingModel` | `embedding_model.py` | Text vectorization models |
| `RerankModel` | `rerank_model.py` | Result reranking models |
| `Seq2txtModel` | `sequence2txt_model.py` | Speech-to-text models |
| `TTSModel` | `tts_model.py` | Text-to-speech models |
| `OcrModel` | `ocr_model.py` | OCR models |

Sources: [rag/llm/__init__.py142-149](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L142-L149) [rag/llm/__init__.py152-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L152-L161)

## LLM Factories Registry

The `llm_factories.json` file serves as the canonical registry of all supported models and their capabilities. This configuration is used by the frontend to render provider options and by the backend for API key validation and tenant model management.

Title: Registry Schema to Database Mapping

```
Initialization: api/db/init_data.py

Code Entity: DB Models

Registry: llm_factories.json

factory_llm_infos[]

llm[]: Model Specs

LLMFactories [api/db/db_models.py]

LLM [api/db/db_models.py]

init_llm_factory()
```

Sources: [conf/llm_factories.json1-10](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L1-L10) [api/db/init_data.py109-126](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/init_data.py#L109-L126) [api/db/db_models.py27](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L27-L27)

### Factory Entry Fields

Each factory in the registry has the following structure:

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `name` | string | Factory identifier matching `_FACTORY_NAME` | `"OpenAI"` |
| `url` | string | Default API base URL | `"https://api.openai.com/v1"` |
| `status` | string | Availability status (1=active) | `"1"` |
| `rank` | string | Display priority (higher = earlier) | `"999"` |
| `llm` | array | Array of model definitions | `[{...}, {...}]` |

Sources: [conf/llm_factories.json4-10](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L4-L10)

### Model Definition Fields

| Field | Type | Description | Example |
| --- | --- | --- | --- |
| `llm_name` | string | Unique model identifier | `"gpt-5.2"` |
| `tags` | string | Model capabilities and context window | `"LLM,CHAT,400k"` |
| `max_tokens` | number | Maximum context length | `400000` |
| `model_type` | string | Model category (chat, embedding, rerank, etc.) | `"chat"` |
| `is_tools` | boolean | Supports function calling | `true` |

Sources: [conf/llm_factories.json11-17](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L11-L17)

## Supported Providers and Base Configurations

RAGFlow supports a wide array of cloud and local providers, many of which share common API patterns.

### Provider Categories and LiteLLM

RAGFlow uses `SupportedLiteLLMProvider` to define standard providers that can be handled via the LiteLLM library.

| Category | Key Providers |
| --- | --- |
| **Global Cloud** | OpenAI, Anthropic, Gemini, xAI, DeepSeek, Mistral, Cohere |
| **China Cloud** | Tongyi-Qianwen, Dashscope, ZHIPU-AI, Moonshot, Baichuan, MiniMax, StepFun |
| **Local/Self-Hosted** | Ollama, Xinference, LocalAI, GPUStack |

Sources: [rag/llm/__init__.py25-65](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/__init__.py#L25-L65)

### Base URL and Prefix Configuration

Default base URLs and LiteLLM prefixes are centralized in `__init__.py`:

- Default URLs: Mapped in FACTORY_DEFAULT_BASE_URL. For example, DeepSeek defaults to https://api.deepseek.com/v1. rag/llm/__init__.py67-97
- LiteLLM Prefixes: Mapped in LITELLM_PROVIDER_PREFIX. For example, Ollama uses the ollama_chat/ prefix. rag/llm/__init__.py100-140

## Implementation Architecture

Providers implement model type classes inheriting from a common `Base` abstract class defined in each model type module.

### Core Implementation Patterns

#### 1. Chat Models (`chat_model.py`)

Chat implementations typically wrap `OpenAI` or `AsyncOpenAI` clients. They handle message formatting and streaming.

- Base Class: Base(ABC) rag/llm/chat_model.py161-174
- Policy Application: _apply_model_family_policies handles model-specific quirks, such as disabling thinking for Qwen3 non-stream requests or managing reasoning parameters for Kimi. rag/llm/chat_model.py108-159
- Parameter Filtering: ALLOWED_GEN_CONF_KEYS and LITELLM_ALLOWED_GEN_CONF_KEYS ensure only supported parameters are passed to underlying APIs. rag/llm/chat_model.py71-105

#### 2. Embedding Models (`embedding_model.py`)

Implementations must provide `encode` and `encode_queries` methods. [rag/llm/embedding_model.py147-151](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/embedding_model.py#L147-L151)

- Batch Processing: _batched_encode provides a shared template for batching, truncation, and token accumulation. rag/llm/embedding_model.py153-175
- DashScope Logic: _dashscope_native_http_api_url resolves native HTTP API roots for Tongyi-Qianwen embeddings, distinguishing between domestic and international endpoints. rag/llm/embedding_model.py76-118

#### 3. Rerank Models (`rerank_model.py`)

Implementations compute similarity scores between a query and multiple texts.

- Normalization: _normalize_rank guarantees scores land in [0, 1] for hybrid search blending, using min-max mapping for out-of-range outputs (e.g., NVIDIA logits). rag/llm/rerank_model.py66-91
- Provider Implementations: Includes JinaRerank, XInferenceRerank, and LocalAIRerank. rag/llm/rerank_model.py94-175

#### 4. CV and Vision Models (`cv_model.py`)

Handles image description and vision-based chat.

- Image Normalization: _normalize_image converts various inputs (bytes, BytesIO, data URLs) into a consistent format. rag/llm/cv_model.py105-141
- Prompt Generation: _image_prompt constructs multimodal message payloads for vision models. rag/llm/cv_model.py142-156

## Provider Verification Flow

Before saving a provider's API key, the system performs a live verification check in `llm_app.py`.

Title: API Key Verification Logic

```
RerankModel[factory]
EmbeddingModel[factory]
ChatModel[factory]
api/apps/llm_app.py
RerankModel[factory]
EmbeddingModel[factory]
ChatModel[factory]
api/apps/llm_app.py
If any pass, save to TenantLLM
Instantiate & check_streamly()
Return valid chunk or Error
Instantiate & encode(["Test"])
Return vector or Error
Instantiate & similarity(q, [t])
Return score or Error
```

Sources: [api/apps/llm_app.py74-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L74-L153)

The verification logic in `set_api_key` includes:

1. Chat Check: Attempts a small streaming request using mdl.async_chat_streamly. api/apps/llm_app.py114-130
2. Embedding Check: Verifies mdl.encode returns a non-empty embedding array. api/apps/llm_app.py97-109
3. Rerank Check: Verifies mdl.similarity returns a valid score and token count. api/apps/llm_app.py131-144

## Go Server Model Drivers

The Go-based server implements a `ModelFactory` to create `ModelDriver` instances based on provider names.

Title: Go Model Factory Architecture

```
Drivers [internal/entity/models/]

Factory [internal/entity/models/factory.go]

anthropic

deepseek

openai

ollama

ModelFactory.CreateModelDriver()

NewAnthropicModel()

NewDeepSeekModel()

NewOpenAIModel()

NewOllamaModel()
```

Sources: [internal/entity/models/factory.go24-33](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/entity/models/factory.go#L24-L33) [internal/entity/models/factory.go35-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/entity/models/factory.go#L35-L159)

The `ModelFactory` uses a large switch statement in `CreateModelDriver` to instantiate the appropriate driver for providers ranging from `anthropic` to `gpustack`. [internal/entity/models/factory.go35-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/entity/models/factory.go#L35-L159) Drivers are typically initialized with a `baseURL` map and a `URLSuffix`. [internal/entity/models/factory.go33](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/entity/models/factory.go#L33-L33)

Sources: [internal/entity/models/factory.go24-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/entity/models/factory.go#L24-L159) [internal/service/model_service.go104-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/model_service.go#L104-L137)


---



<!-- ===== 5.3-error-handling-and-retry-logic ===== -->

# Error Handling and Retry Logic

Relevant source files
- api/apps/llm_app.py
- api/db/init_data.py
- api/db/services/llm_service.py
- conf/llm_factories.json
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- web/src/pages/user-setting/setting-model/constant.ts

This page documents the sophisticated error handling and retry mechanisms implemented in RAGFlow's LLM Integration System. The system provides automatic retry logic with exponential backoff, error classification, and configurable retry policies to handle transient failures when communicating with external LLM providers.

For information about the LLM abstraction layer and model types, see [5.1 LLMBundle and Model Types](https://github.com/infiniflow/ragflow/blob/d32e05d5/5.1 LLMBundle and Model Types) For provider-specific implementations, see [5.2 Provider Implementations](https://github.com/infiniflow/ragflow/blob/d32e05d5/5.2 Provider Implementations)

## Overview

RAGFlow implements a comprehensive error handling system across all LLM model types (chat, embedding, rerank, CV, TTS, etc.). The system classifies errors into retryable and non-retryable categories, automatically retries transient failures with exponential backoff, and provides detailed error messages for debugging. The core logic is housed in the `Base` class within each model category.

**Sources:** [rag/llm/chat_model.py41-53](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L41-L53) [rag/llm/chat_model.py161-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L161-L173) [rag/llm/cv_model.py59-68](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/cv_model.py#L59-L68)

## Error Classification System

### Error Code Enumeration

The system defines a structured set of error codes in the `LLMErrorCode` enumeration. These codes provide a unified way to handle diverse error responses from multiple providers (OpenAI, Anthropic, Dashscope, etc.).

| Error Code | Description |
| --- | --- |
| `ERROR_RATE_LIMIT` | Rate limit exceeded (429, TPM/RPM limit) |
| `ERROR_AUTHENTICATION` | Authentication failure (401, invalid API key) |
| `ERROR_INVALID_REQUEST` | Invalid request format (400, bad parameters) |
| `ERROR_SERVER` | Provider server errors (500, 502, 503, 504) |
| `ERROR_TIMEOUT` | Request timeout from the client or provider |
| `ERROR_CONNECTION` | Network/connection errors (DNS, unreachable) |
| `ERROR_MODEL` | Model not found, does not exist, or max rounds exceeded |
| `ERROR_CONTENT_FILTER` | Content policy violation/safety filters |
| `ERROR_QUOTA` | Quota exceeded, billing issues, or insufficient balance |
| `ERROR_MAX_RETRIES` | System-side retry limit reached |
| `ERROR_GENERIC` | Uncategorized or unknown errors |

**Sources:** [rag/llm/chat_model.py42-54](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L42-L54)

### Error Classification Logic

The `Base` class implements `_classify_error`, which uses keyword matching to categorize exceptions. It converts the error string to lowercase and searches for specific patterns mapped to `LLMErrorCode`.

Title: "Error Classification Decision Tree"

```
Yes

No

Yes

No

Yes

No

Yes

No

Yes

No

Yes

No

Yes

No

Yes

No

Yes

No

Error Occurs

Base._classify_error(error)

Match Keywords in Error String

'quota, capacity, credit, billing, balance, 欠费'

'rate limit, 429, tpm limit, too many requests, requests per minute'

'auth, key, apikey, 401, forbidden, permission'

'invalid, bad request, 400, format, malformed'

'server, 503, 502, 504, 500, unavailable'

'timeout, timed out'

'connect, network, unreachable, dns'

'filter, content, policy, blocked, safety'

'model, not found, does not exist'

Return LLMErrorCode.ERROR_QUOTA

Return LLMErrorCode.ERROR_RATE_LIMIT

Return LLMErrorCode.ERROR_AUTHENTICATION

Return LLMErrorCode.ERROR_INVALID_REQUEST

Return LLMErrorCode.ERROR_SERVER

Return LLMErrorCode.ERROR_TIMEOUT

Return LLMErrorCode.ERROR_CONNECTION

Return LLMErrorCode.ERROR_CONTENT_FILTER

Return LLMErrorCode.ERROR_MODEL

Return LLMErrorCode.ERROR_GENERIC
```

**Sources:** [rag/llm/chat_model.py178-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L178-L210)

## Retry Configuration

### Configuration Parameters

Retry behavior is configured through constructor parameters or environment variables, allowing for global defaults with per-instance overrides.

| Parameter | Environment Variable | Default | Description |
| --- | --- | --- | --- |
| `max_retries` | `LLM_MAX_RETRIES` | 5 | Maximum number of retry attempts |
| `retry_interval` | `LLM_BASE_DELAY` | 2.0 | Base delay in seconds for backoff calculation |
| `max_rounds` | N/A | 5 | Maximum rounds for tool-calling/ReAct loops |
| `timeout` | `LLM_TIMEOUT_SECONDS` | 600 | Global request timeout for OpenAI clients |

**Sources:** [rag/llm/chat_model.py163-170](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L163-L170) [rag/llm/cv_model.py61-64](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/cv_model.py#L61-L64)

## Retry Strategy and Backoff

### Exponential Backoff with Jitter

The system uses a randomized backoff strategy to avoid "thundering herd" problems where many clients retry simultaneously.

Title: "Delay Calculation Flow"

```
Base._get_delay()

self.base_delay

random.uniform(10, 150)

delay = base_delay * random_factor

time.sleep(delay) or asyncio.sleep(delay)
```

The delay ranges from `base_delay * 10` to `base_delay * 150` seconds (typically 20 to 300 seconds). This large range provides significant jitter to distribute retry attempts across time.

**Sources:** [rag/llm/chat_model.py175-176](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L175-L176)

### Retryable Error Types

Only specific error types trigger automatic retries. Generally, `ERROR_RATE_LIMIT` and `ERROR_SERVER` are considered retryable, while `ERROR_AUTHENTICATION` or `ERROR_INVALID_REQUEST` are terminal. In `EmbeddingModel`, HTTP status codes are used to determine retryability.

**Sources:** [rag/llm/embedding_model.py63-68](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/embedding_model.py#L63-L68) [rag/llm/chat_model.py178-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L178-L210)

## Integration and Implementation

### API Layer Validation

The API layer in `api/apps/llm_app.py` adds an additional layer of timeout protection using `asyncio.wait_for`. This ensures that UI-driven operations (like testing an API key) do not hang indefinitely. During API key verification, a 10-second timeout is enforced.

Title: "API Timeout and Validation Flow"

```
rag/llm/chat_model.py

Validation Logic

api/apps/llm_app.py

set_api_key()

timeout_seconds = 10

asyncio.wait_for(mdl.encode/chat, timeout)

ChatModel.async_chat_streamly
```

**Sources:** [api/apps/llm_app.py87](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L87-L87) [api/apps/llm_app.py101-104](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L101-L104) [api/apps/llm_app.py124](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L124-L124)

### Model-Specific Overrides

Some models implement custom retry logic or specialized error handling:

- Embedding Status Checks: Uses _raise_model_exception_if_failed to distinguish between 4xx (non-retryable) and 5xx/408/429 (retryable) HTTP errors. rag/llm/embedding_model.py63-68
- Rerank Normalization: The Base.similarity method in rerank_model.py handles empty inputs by returning zeros and ensures scores are min-max normalized to [0, 1] to prevent un-normalized provider output from corrupting hybrid search results. rag/llm/rerank_model.py34-60
- Xinference ASR: Uses standard requests.post with manual error checking and returns **ERROR** if transcription fails via RequestException. rag/llm/sequence2txt_model.py197-210
- CV Model: Implements basic error catching in async_chat and async_chat_streamly, returning the **ERROR** prefix on failure. rag/llm/cv_model.py158-167 rag/llm/cv_model.py169-190
- Fish Audio TTS: Raises a RuntimeError with an **ERROR** prefix when httpx status errors occur during the stream. rag/llm/tts_model.py183-184

### Error Message Protocol

Terminal errors are returned as strings prefixed with `**ERROR**`. This allows downstream components to distinguish between successful content and error reports. Additionally, truncated responses due to context limits are flagged with specific notification strings like `LENGTH_NOTIFICATION_CN`.

**Sources:** [rag/llm/chat_model.py62-64](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L62-L64) [rag/llm/cv_model.py167](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/cv_model.py#L167-L167) [rag/llm/sequence2txt_model.py123](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/sequence2txt_model.py#L123-L123)


---



<!-- ===== 5.4-tenant-configuration-and-model-management ===== -->

# Tenant Configuration and Model Management

Relevant source files
- api/apps/llm_app.py
- api/apps/restful_apis/agent_api.py
- api/db/init_data.py
- api/db/services/canvas_service.py
- api/db/services/conversation_service.py
- api/db/services/llm_service.py
- api/db/services/tenant_llm_service.py
- conf/llm_factories.json
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- test/testcases/test_http_api/test_session_management/test_agent_completions.py
- test/testcases/test_http_api/test_session_management/test_agent_sessions.py
- test/testcases/test_http_api/test_session_management/test_chat_completions.py
- test/testcases/test_web_api/test_agent_app/test_agents_webhook_unit.py
- test/unit_test/api/apps/restful_apis/test_get_agent_session.py
- test/unit_test/api/db/services/test_dialog_service_final_answer.py
- test/unit_test/api/db/services/test_dialog_service_use_sql_source_columns.py
- web/src/pages/user-setting/setting-model/constant.ts

## Purpose and Scope

This document describes RAGFlow's multi-tenant model management system, which enables each tenant (user) to configure and manage their own LLM models independently. The system provides:

- Per-tenant model configurations stored in the tenant_llm database table [api/db/db_models.py:27-27].
- Model selection and initialization from system-wide defaults or custom configurations [api/db/services/llm_service.py:36-82].
- API key management with validation before persistence [api/apps/llm_app.py:76-156].
- CRUD operations via HTTP API endpoints for model lifecycle management [api/apps/llm_app.py:47-471].
- Usage tracking with per-model token counters [api/db/services/llm_service.py:142-142].

The system allows tenants to choose different models for different purposes (chat, embedding, reranking, etc.) and maintains independent API keys and configurations for each tenant-model combination.

## Architecture Overview

### Model Configuration and Instantiation Flow

```
Model_Initialization

Configuration_Sources

Model_Factory_Registry

Service_Layer

Tenant_Management_Layer

configures

has defaults in

queried by

default model IDs

instantiates

wraps with usage tracking

selects provider class from

selects provider class from

selects provider class from

loaded into

loaded into

used by

populates on user creation

available models

factory info

config resolution

User/Tenant
(tenant_id)

TenantLLM table
Per-tenant model configs

Tenant table
Default model selections

TenantLLMService
CRUD & model selection

LLM4Tenant
Config resolution

LLMBundle
Model instantiation wrapper

LLM table
Available models from
llm_factories.json

LLMFactories table
Provider metadata

ChatModel registry

EmbeddingModel registry

RerankModel registry

service_conf.yaml
Default LLM settings

conf/llm_factories.json
Model definitions

get_init_tenant_llm()
Create default configs

Model Selection Logic:
1. tenant_llm row
2. service_conf defaults
3. Factory defaults
```

The system manages models through a multi-layered configuration approach: (1) the `tenant_llm` table stores per-tenant API keys and configurations [[api/db/db_models.py:27-27]](), (2) the `tenant` table stores default model selections, (3) `TenantLLMService` handles CRUD operations and model selection [[api/db/services/tenant_llm_service.py:23-23]](), (4) `LLM4Tenant` resolves configurations from multiple sources, and (5) `LLMBundle` instantiates the actual model classes from the factory registries while adding usage tracking [[api/db/services/llm_service.py:85-118]]().

Sources: [[api/db/services/llm_service.py:85-91]](), [[api/db/services/tenant_llm_service.py:23-23]](), [[api/apps/llm_app.py:79-156]]()

## Database Schema

### tenant_llm Table

The `tenant_llm` table stores per-tenant model configurations. Each row represents one tenant's configuration for one specific model instance.

| Field | Type | Description |
| --- | --- | --- |
| `id` | String | Unique identifier for this configuration |
| `tenant_id` | String | References `tenant.id` - identifies which tenant owns this config |
| `llm_factory` | String | Provider name (e.g., "OpenAI", "Anthropic") |
| `llm_name` | String | Specific model name (e.g., "gpt-4o", "claude-3") |
| `model_type` | String | Type: `chat`, `embedding`, `rerank`, etc. [[common/constants.py:26-26]]() |
| `api_key` | String | API key or JSON authentication object |
| `api_base` | String | Custom base URL |
| `max_tokens` | Integer | Maximum context window size |
| `used_tokens` | BigInteger | Cumulative token usage counter |

**Table: tenant_llm Schema**

The `api_key` field stores either a simple string or a JSON-encoded object for complex authentication requirements. Token usage is updated via `TenantLLMService.increase_usage_by_id()` [[api/db/services/tenant_llm_service.py:27-27]]().

Sources: [[api/db/db_models.py:27-27]](), [[api/db/services/tenant_llm_service.py:23-23]]()

## API Key Validation Process

The API key validation process tests credentials against actual provider endpoints before persistence in the `set_api_key` route [[api/apps/llm_app.py:76-79]]().

### Validation Logic

For each `llm_factory`, the system tests model types until one succeeds:

1. Embedding Model Test: Calls mdl.encode(["Test if the api key is available"]) and verifies results [api/apps/llm_app.py:101-104].
2. Chat Model Test: Calls mdl.async_chat_streamly() with a simple greeting and verifies the stream [api/apps/llm_app.py:114-124].
3. Rerank Model Test: Calls mdl.similarity() with test query and documents [api/apps/llm_app.py:135-138].

```
Yes

Yes

No

No

Yes

Yes

No

No

Yes

Yes

No

No

set_api_key() called

Query LLM table for factory

Embedding
model exists?

mdl.encode(['Test...'])

Success?

TenantLLMService.filter_update()

Chat
model exists?

mdl.async_chat_streamly(...)

Success?

Rerank
model exists?

mdl.similarity(...)

Success?

Return error message

Return success
```

Sources: [[api/apps/llm_app.py:79-156]]()

### Special Authentication Formats

Some providers require JSON-encoded parameters in the `api_key` field which are parsed during model initialization:

| Provider | JSON Fields | Location |
| --- | --- | --- |
| **Azure-OpenAI** | `api_key`, `api_version`, `base_url` | [[rag/llm/sequence2txt_model.py:171-171]]() |
| **Fish Audio** | `fish_audio_ak`, `fish_audio_refid` | [[rag/llm/tts_model.py:152-157]]() |
| **Tencent Cloud** | `tencent_cloud_sid`, `tencent_cloud_sk` | [[rag/llm/tts_model.py:212-214]]() |

Sources: [[rag/llm/sequence2txt_model.py:171-171]](), [[rag/llm/tts_model.py:152-157]]()

## Model Selection and Initialization

### LLMBundle Class Structure

```
wraps

increase_usage_by_id()

LLM4Tenant

+tenant_id: str

+model_config: dict

+mdl: Base model instance

+init(tenant_id, model_config, lang)

LLMBundle

+bind_tools(session, tools)

+encode(texts)

+encode_queries(query)

+similarity(query, texts)

+describe(image)

+async_chat(system, history, gen_conf)

+async_chat_streamly(system, history, gen_conf)

«interface»

ChatModel_Base

+async_chat()

+async_chat_streamly()

TenantLLMService
```

`LLMBundle` extends `LLM4Tenant` [[api/db/services/llm_service.py:85-87]]() to add usage tracking wrappers. Each wrapper method calls the underlying provider and then updates usage via `TenantLLMService.increase_usage_by_id()` [[api/db/services/tenant_llm_service.py:27-27]]().

Sources: [[api/db/services/llm_service.py:85-364]](), [[api/db/services/tenant_llm_service.py:27-27]]()

## Token Usage Tracking

Token usage tracking is implemented via `increase_usage_by_id()`, which increments token counters in the `TenantLLM` table after each LLM call [[api/db/services/tenant_llm_service.py:27-27]]().

### Token Counting Implementation

| Model Type | Counting Method | Location |
| --- | --- | --- |
| **Chat** | `total_token_count_from_response(response)` | [[rag/llm/chat_model.py:35-35]]() |
| **Embedding** | `total_token_count_from_response(res)` | [[rag/llm/embedding_model.py:186-186]]() |
| **Rerank** | `total_token_count_from_response(res)` | [[rag/llm/rerank_model.py:74-74]]() |
| **Vision** | `total_token_count_from_response(response)` | [[rag/llm/cv_model.py:34-34]]() |

Sources: [[rag/llm/chat_model.py:35-35]](), [[rag/llm/embedding_model.py:186-186]](), [[rag/llm/rerank_model.py:74-74]](), [[rag/llm/cv_model.py:34-34]]()

## API Endpoints for Configuration

The `llm_app.py` manager provides the interface for managing tenant configurations [[api/apps/llm_app.py:47-471]]().

- GET /factories: Returns available LLM factories [api/apps/llm_app.py:47-49].
- POST /set_api_key: Validates and sets API keys for a factory [api/apps/llm_app.py:76-79].
- POST /add_llm: Adds a single custom LLM configuration [api/apps/llm_app.py:163-166].
- GET /my_llms: Lists current tenant's configured LLMs [api/apps/llm_app.py:388-390].

Sources: [[api/apps/llm_app.py:47-471]]()

## Configuration File Format

The `conf/llm_factories.json` file defines available models and their capabilities [[conf/llm_factories.json:1-10]]():

```
{
  "factory_llm_infos": [
    {
      "name": "OpenAI",
      "llm": [
        {
          "llm_name": "gpt-5.2-pro",
          "tags": "LLM,CHAT,400k,IMAGE2TEXT",
          "max_tokens": 400000,
          "model_type": "chat",
          "is_tools": true
        }
      ]
    }
  ]
}
```

Sources: [[conf/llm_factories.json:1-110]]()


---



<!-- ===== 5.5-tool-calling-and-function-use ===== -->

# Tool Calling and Function Use

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/retrieval.py
- api/apps/llm_app.py
- api/db/init_data.py
- api/db/services/llm_service.py
- common/mcp_tool_call_conn.py
- conf/llm_factories.json
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- rag/prompts/generator.py
- web/src/pages/user-setting/setting-model/constant.ts

This document describes RAGFlow's tool calling and function invocation system, which enables Large Language Models (LLMs) to execute external functions and integrate their results into conversation responses. Tool calling allows models to access external APIs, databases, or custom functions during generation, making them capable of performing actions beyond text generation.

For information about model provider configuration and usage tracking, see [5.4 Tenant Configuration and Model Management](https://github.com/infiniflow/ragflow/blob/d32e05d5/5.4 Tenant Configuration and Model Management) For details on the broader LLM abstraction layer, see [5.1 LLMBundle and Model Types](https://github.com/infiniflow/ragflow/blob/d32e05d5/5.1 LLMBundle and Model Types)

## Purpose and Scope

The tool calling system provides:

- Tool Binding: Mechanism to attach function definitions to LLM instances via LLMBundle api/db/services/llm_service.py107-111
- Multi-Round Execution: Automatic handling of iterative tool calls with result feedback in async_chat_with_tools rag/llm/chat_model.py279-331
- Streaming Support: Real-time tool execution during streaming responses in async_chat_streamly_with_tools rag/llm/chat_model.py333-442
- Error Resilience: JSON repair via json_repair, exception handling, and retry logic for tool failures rag/llm/chat_model.py311-318
- Provider Compatibility: Unified interface across OpenAI-compatible tool calling APIs rag/llm/chat_model.py116-128
- Agent Integration: Advanced tool orchestration within the Agent component, supporting Model Context Protocol (MCP) servers agent/component/agent_with_tools.py100-110

## Model Support for Tool Calling

Capability for tool calling is tracked through the `is_tools` flag in model definitions. This flag is initialized in the `Base` chat model class [rag/llm/chat_model.py171](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L171-L171)

### Supported Models Configuration

The configuration file `conf/llm_factories.json` defines which models support tool calling. The `is_tools` attribute determines if the model is eligible for function binding:

| Provider | Model Name | Tool Support (`is_tools`) |
| --- | --- | --- |
| OpenAI | `gpt-5.5` | `true` [conf/llm_factories.json16](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L16-L16) |
| OpenAI | `gpt-4o` | `true` [conf/llm_factories.json156](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L156-L156) |
| OpenAI | `gpt-3.5-turbo` | `false` [conf/llm_factories.json163](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L163-L163) |
| DeepSeek | `deepseek-chat` | `true` [conf/llm_factories.json3447](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L3447-L3447) |

The `_resolve_my_llm_is_tools` function in the API layer dynamically checks this capability for tenant-specific models [api/apps/llm_app.py30-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L30-L44)

**Sources**: [conf/llm_factories.json1-163](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/llm_factories.json#L1-L163) [rag/llm/chat_model.py170-171](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L170-L171) [api/apps/llm_app.py30-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/llm_app.py#L30-L44)

## Core Architecture

### Tool Calling Components

The following diagram bridges the Natural Language space (User Query) to the Code Entity space (Classes/Methods).

Title: Tool Calling Component Mapping

```
Tool_Execution_Layer

Code_Entity_Space:_rag/llm/chat_model.py

Code_Entity_Space:_api/db/services/llm_service.py

Natural_Language_Space

User Query: 'Check weather in London'

LLMBundle

LLMBundle.bind_tools()

Base (Chat Model Class)

Base.async_chat_with_tools()

Base.async_chat_streamly_with_tools()

json_repair.loads()

toolcall_sessions

tool_call(name, args)
```

**Sources**: [api/db/services/llm_service.py85-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L85-L111) [rag/llm/chat_model.py272-277](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L272-L277) [rag/llm/chat_model.py170-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L170-L173)

### Key Data Structures

The tool calling system uses structures compatible with the OpenAI API format:

| Structure | Purpose | Key Fields |
| --- | --- | --- |
| `tools` list | Function definitions sent to LLM | `type`, `function` (name, parameters) [rag/llm/chat_model.py276](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L276-L276) |
| `tool_call` | LLM's request to execute a function | `id`, `function.name`, `function.arguments` [rag/llm/chat_model.py299](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L299-L299) |
| `toolcall_sessions` | Execution context | Dictionary storing session-specific tool handlers [rag/llm/chat_model.py173](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L173-L173) |

## Tool Binding Mechanism

Tool binding attaches function definitions and an execution session to an LLM instance. In the `Agent` component, this includes loading tools from both internal components and external MCP servers.

Title: Tool Binding Logic Flow

```
"ChatModel (rag/llm/chat_model.py)"
"LLMBundle (api/db/services/llm_service.py)"
"Agent/Canvas (agent/component/agent_with_tools.py)"
"ChatModel (rag/llm/chat_model.py)"
"LLMBundle (api/db/services/llm_service.py)"
"Agent/Canvas (agent/component/agent_with_tools.py)"
Checks self.is_tools flag [108-110]
bind_tools(session, tools)
mdl.bind_tools(session, tools)
Sets self.toolcall_sessions [272-277]
Sets self.tools [272-277]
```

**Implementation Details**:

1. LLMBundle Layer: Checks self.is_tools flag from model config. If false, logs a warning but proceeds api/db/services/llm_service.py108-111
2. Base Model Layer: Stores the toolcall_session and tools list. It asserts that both are provided rag/llm/chat_model.py272-277
3. Agent Layer: Dynamically initializes tool objects using component_class and binds them to the LLMBundle instance agent/component/agent_with_tools.py79-92

**Sources**: [api/db/services/llm_service.py107-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L107-L111) [rag/llm/chat_model.py272-277](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L272-L277) [agent/component/agent_with_tools.py76-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L76-L110)

## Multi-Round Execution Flow

The `async_chat_with_tools` method implements a loop that allows the LLM to call tools, receive results, and call more tools if needed.

1. Loop Initialization: Runs up to self.max_rounds (default 5) rag/llm/chat_model.py170-290
2. Tool Detection: Checks response.choices[0].message.tool_calls rag/llm/chat_model.py297
3. JSON Repair: Arguments are parsed using json_repair.loads() to handle malformed LLM output rag/llm/chat_model.py311
4. Execution: Invokes self.toolcall_session.tool_call(name, args) via thread_pool_exec rag/llm/chat_model.py313
5. History Update: Appends the tool result to the conversation history and continues the loop rag/llm/chat_model.py315-318

**Sources**: [rag/llm/chat_model.py279-331](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L279-L331)

## Streaming Tool Execution

Streaming tool calls require accumulating fragments (deltas) before execution.

Title: Streaming Tool Execution State Machine

```
rag/llm/chat_model.py:_async_chat_streamly_with_tools

Yes

No

Yes

Yes

Receive Stream Chunk

Is Content?

Yield Content to User

Is Tool Call?

Accumulate arguments in final_tool_calls

Stream End?

Execute accumulated tool_calls
```

**Implementation Details**:

- Delta Accumulation: Tool arguments arrive in fragments; the system accumulates them into a final_tool_calls dictionary rag/llm/chat_model.py360-369
- Verbose Output: Before execution, it yields a verbose message Begin to call... to the stream rag/llm/chat_model.py401
- Recursive Rounding: After tool execution, it recursively calls itself to handle subsequent tool calls from the LLM rag/llm/chat_model.py423-432

**Sources**: [rag/llm/chat_model.py333-442](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L333-L442)

## Error Handling and Resilience

- Classification: Errors are classified into types like ERROR_INVALID_REQUEST or ERROR_RATE_LIMIT via keyword mapping in _classify_error rag/llm/chat_model.py178-202
- Retry Logic: Uses exponential backoff with jitter (random.uniform(10, 150)) for API failures rag/llm/chat_model.py175-176
- Max Rounds: If max_rounds is exceeded, the system appends a notification and forces a final generation without tools rag/llm/chat_model.py320-325
- Model Family Policies: Special logic in _apply_model_family_policies handles provider-specific quirks, such as disabling thinking for Qwen3 models or adjusting temperature for Kimi-k2.5 rag/llm/chat_model.py108-158

**Sources**: [rag/llm/chat_model.py108-158](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L108-L158) [rag/llm/chat_model.py178-202](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L178-L202) [rag/llm/chat_model.py320-325](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L320-L325)


---



<!-- ===== 6-document-processing-pipeline ===== -->

# Document Processing Pipeline

Relevant source files
- api/db/__init__.py
- api/db/db_models.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- deepdoc/parser/excel_parser.py
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- rag/app/book.py
- rag/app/laws.py
- rag/app/manual.py
- rag/app/naive.py
- rag/app/one.py
- rag/app/paper.py
- rag/app/presentation.py
- rag/app/qa.py
- rag/app/table.py
- rag/nlp/__init__.py
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/task_executor.py
- test/unit_test/deepdoc/parser/test_excel_parser.py

## Purpose and Scope

The Document Processing Pipeline is the core system that transforms raw uploaded documents into searchable, semantically-indexed chunks within RAGFlow. This page documents the complete workflow from document upload through parsing, chunking, enhancement, embedding, and final indexing in the document store.

For details on parsing strategies and format support, see [Document Parsing Strategies](/infiniflow/ragflow/6.1-document-parsing-strategies). For details on chunking logic, see [Chunking Methods](/infiniflow/ragflow/6.2-chunking-methods). For details on vision and OCR, see [Vision Processing: OCR and Layout Recognition](/infiniflow/ragflow/6.4-vision-processing:-ocr-and-layout-recognition). For advanced features, see [Advanced Features: GraphRAG and RAPTOR](/infiniflow/ragflow/6.5-advanced-features:-graphrag-and-raptor).

## Pipeline Architecture Overview

### High-Level Pipeline Flow

The document processing pipeline follows a multi-stage asynchronous workflow. Documents are uploaded via API, tasks are queued in Redis, and background workers (task executors) process them through parsing, chunking, enhancement, embedding, and final indexing stages.

**Pipeline Flow Diagram**

```
Storage

Embedding & Indexing

Enhancement

Parser Selection

Task Executor

Upload & Task Creation

Document Upload
POST /upload

TaskService.get_task

Redis Streams
SVR_CONSUMER_GROUP_NAME

TaskManager.collect

build_chunks()

FACTORY Dict
ParserType → Module

Parser Execution
chunk()

keyword_extraction()

question_proposal()

gen_metadata()

content_tagging()

LLMBundle.encode

search.index_name

DocStoreConnection
Dealer.search

Object Storage
MinIO

Metadata DB
MySQL/PostgreSQL
```

**Sources**: [rag/svr/task_executor.py114-131](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L114-L131) [rag/svr/task_executor.py133-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L133-L139) [api/db/services/task_service.py76-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L76-L143) [api/db/services/document_service.py41-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L41-L72) [rag/nlp/search.py38-51](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L38-L51)

## Task Executor and Queue System

### Task Collection and Distribution

The task executor service (`task_executor.py`) runs as a background worker process that continuously polls Redis Streams for new document processing tasks. Multiple executor instances can run concurrently, each identified by a consumer number.

**Task Distribution Architecture**

```
Concurrency Control

Task Executors

Consumer Group

Redis Streams

REDIS_CONN
SVR_CONSUMER_GROUP_NAME

TaskManager
SVR_CONSUMER_GROUP_NAME

task_executor_0
TE_IDX

task_executor_1
TE_IDX

task_limiter
Semaphore

chunk_limiter
Semaphore

embed_limiter
Semaphore
```

**Sources**: [rag/svr/task_executor.py95-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L95-L101) [rag/svr/task_executor.py143-144](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L143-L144) [rag/svr/task_executor.py155-156](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L155-L156) [rag/utils/redis_conn.py1-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/redis_conn.py#L1-L50)

### Task Types and Routing

The task executor handles multiple task types, each corresponding to different processing modes. The mapping is defined in `TASK_TYPE_TO_PIPELINE_TASK_TYPE`.

| Task Type | Pipeline Task Type | Handler / Logic | Description |
| --- | --- | --- | --- |
| `dataflow` | `PARSE` | `run_dataflow()` | Standard document parsing through custom pipelines |
| `raptor` | `RAPTOR` | `RAPTOR_TREE_BUILDER` | Hierarchical clustering for improved retrieval |
| `graphrag` | `GRAPH_RAG` | `run_graphrag_for_kb()` | Knowledge graph extraction |
| `mindmap` | `MINDMAP` | `MindMapExtractor` | Mind map generation from documents |
| `memory` | `MEMORY` | `handle_save_to_memory_task()` | Agent memory persistence |

**Sources**: [rag/svr/task_executor.py43-55](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L43-L55) [rag/svr/task_executor.py133-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L133-L139) [rag/raptor.py87-89](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L87-L89)

## Parser Selection and Factory Pattern

### FACTORY Dictionary Mapping

The task executor uses a factory pattern to select the appropriate parser based on the document's `parser_id`. The `FACTORY` dictionary maps parser types to their corresponding parser modules.

**Parser Factory Mapping**

```
Parser Modules

Parser Factory

FACTORY = {
ParserType → Module
}

rag.app.naive

rag.app.paper

rag.app.book

rag.app.presentation

rag.app.manual

rag.app.laws

rag.app.qa

rag.app.table

rag.app.one

rag.app.tag
```

**Sources**: [rag/svr/task_executor.py114-131](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L114-L131) [rag/app/naive.py1-63](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L1-L63) [rag/app/manual.py1-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/manual.py#L1-L31) [rag/app/table.py1-37](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L1-L37)

### Parser Selection Logic

Each parser module provides a method (typically `chunk()`) that takes the document binary and configuration parameters. The parser is selected at runtime using the `parser_id` from the task metadata.

The `chunk()` function is typically invoked with:

- filename: Document name
- binary: Document binary content
- from_page/to_page: Page range for processing
- lang: Document language
- callback: Progress callback function (set_progress)
- parser_config: Parser-specific configuration (e.g., layout_recognize, chunk_token_num)

For details on format-specific parsing, see [Document Parsing Strategies](/infiniflow/ragflow/6.1-document-parsing-strategies).

**Sources**: [rag/svr/task_executor.py165-196](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L196) [rag/app/naive.py115-128](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L115-L128) [rag/app/manual.py137-174](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/manual.py#L137-L174)

## Chunking Process

### Chunk Building Workflow

The `build_chunks()` function orchestrates the entire chunking process with timeout protection and error handling. It fetches the document binary from storage and executes the selected chunker.

**Chunk Building Pipeline**

```
build_chunks()

Select Chunker
FACTORY[parser_id]

Execute chunker.chunk()
with chunk_limiter

Process Images
image2id() → MinIO

Content Enhancement
LLM Prompts

Return chunks[]
```

For details on the specific strategies used for different document types, see [Chunking Methods](/infiniflow/ragflow/6.2-chunking-methods).

**Sources**: [rag/svr/task_executor.py245-519](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L245-L519) [api/db/services/task_service.py58-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L58-L72)

### Chunk Data Structure

Each chunk returned by the parser is transformed into a document structure with the following key fields:

| Field | Type | Description |
| --- | --- | --- |
| `id` | string | Hash of content + doc_id |
| `doc_id` | string | Parent document ID |
| `kb_id` | string | Knowledge base ID |
| `content_with_weight` | string | Main text content with weighted title |
| `content_ltks` | list | Tokenized content (loose tokens) |
| `img_id` | string | MinIO object ID for associated images |
| `page_num_int` | int | Source page number |

**Sources**: [rag/svr/task_executor.py294-342](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L294-L342) [rag/nlp/search.py149-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L149-L153)

## Content Enhancement and Embedding

### LLM-Based Enhancement

If enabled in parser configuration, the system generates metadata and semantic improvements for each chunk using LLMs. This includes:

- Keyword Extraction: keyword_extraction() extracts significant terms.
- Question Generation: question_proposal() generates hypothetical questions the chunk can answer.
- Metadata Extraction: gen_metadata() extracts structured fields based on a JSON schema.
- TOC Generation: run_toc_from_text() generates table of contents entries.

For details on these features, see [Content Enhancement and Embedding](/infiniflow/ragflow/6.3-content-enhancement-and-embedding).

**Sources**: [rag/svr/task_executor.py343-452](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L343-L452) [rag/prompts/generator.py60](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L60-L60)

### Embedding Process

After chunks are built and enhanced, they are embedded into vector representations. The `LLMBundle.encode` method (or `encode_queries` in Go-side Search) handles communication with the embedding model provider.

The system ensures context preservation by prepending document titles or specific metadata to the chunk content before embedding.

**Sources**: [rag/svr/task_executor.py575-626](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L575-L626) [rag/nlp/search.py53-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L53-L61) [api/db/services/llm_service.py79](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L79-L79)

## Dataflow Pipeline Execution

### Custom Pipeline Processing

The `run_dataflow()` function handles execution of custom document processing pipelines defined via the Agent/Canvas system. This allows users to define multi-stage processing workflows that go beyond standard parsing.

**Dataflow Execution Diagram**

```
No

Yes

Load Pipeline DSL
UserCanvas

Execute Pipeline
await pipeline.run()

Process Output
chunks/json/markdown

Has Vectors?

Embed Chunks
LLMBundle.encode

Insert to DocStore
Dealer.search
```

**Sources**: [rag/svr/task_executor.py629-768](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L629-L768) [api/db/db_models.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L25-L25)

## Vision and Advanced Features

### Vision Processing

RAGFlow supports sophisticated vision processing including OCR, layout recognition, and table structure recognition. These features are essential for parsing complex PDFs and images. For details, see [Vision Processing: OCR and Layout Recognition](/infiniflow/ragflow/6.4-vision-processing:-ocr-and-layout-recognition).

**Sources**: [rag/app/naive.py35-47](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L35-L47) [rag/app/manual.py33-67](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/manual.py#L33-L67)

### GraphRAG and RAPTOR

For complex knowledge structures, RAGFlow provides:

- RAPTOR: RAPTOR_TREE_BUILDER for hierarchical clustering of chunks.
- GraphRAG: run_graphrag_for_kb() for entity and relationship extraction.

For details, see [Advanced Features: GraphRAG and RAPTOR](/infiniflow/ragflow/6.5-advanced-features:-graphrag-and-raptor).

**Sources**: [rag/svr/task_executor.py136-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L136-L137) [rag/raptor.py87-89](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L87-L89)

## Data Source Connectors

RAGFlow integrates with external data sources through a connector system. These connectors sync external data into the document processing pipeline. For details, see [Data Source Connectors](/infiniflow/ragflow/6.6-data-source-connectors).

**Sources**: [api/db/services/knowledgebase_service.py32-49](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L32-L49)

## Progress Tracking and Error Handling

### Progress Updates

Throughout the pipeline, progress is reported to the database via `set_progress()`. This allows the UI to display real-time processing status.

Progress values typically follow:

- 0.0 - 0.6: OCR and layout analysis
- 0.6 - 0.7: Chunking
- 0.7 - 0.9: Enhancement (keywords, questions)
- 0.9 - 1.0: Embedding and indexing
- -1: Error occurred or Task Canceled

**Sources**: [rag/svr/task_executor.py165-196](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L196) [api/db/services/task_service.py128-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L128-L143)

### Error Handling and Cancellation

The task executor implements multiple error handling mechanisms:

1. Task Cancellation: Checks has_canceled(task_id) before expensive operations.
2. Retry Logic: Tasks are retried up to 3 times before being marked as abandoned.
3. Task Status: Documents and tasks track status using TaskStatus (UNSTART, RUNNING, FAIL, DONE).

**Sources**: [rag/svr/task_executor.py169](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L169-L169) [api/db/services/task_service.py130-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L130-L143) [api/db/services/document_service.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L32-L32)


---



<!-- ===== 6.1-document-parsing-strategies ===== -->

# Document Parsing Strategies

Relevant source files
- deepdoc/parser/__init__.py
- deepdoc/parser/excel_parser.py
- deepdoc/parser/html_parser.py
- deepdoc/parser/json_parser.py
- deepdoc/parser/markdown_parser.py
- rag/app/book.py
- rag/app/laws.py
- rag/app/manual.py
- rag/app/naive.py
- rag/app/one.py
- rag/app/paper.py
- rag/app/picture.py
- rag/app/presentation.py
- rag/app/qa.py
- rag/app/table.py
- rag/flow/chunker/__init__.py
- rag/flow/chunker/schema.py
- rag/flow/parser/parser.py
- rag/flow/parser/pdf_chunk_metadata.py
- rag/flow/parser/utils.py
- rag/flow/tests/dsl_examples/general_pdf_all.json
- rag/flow/tokenizer/schema.py
- rag/flow/tokenizer/tokenizer.py
- rag/nlp/__init__.py
- test/unit_test/deepdoc/parser/test_excel_parser.py
- test/unit_test/deepdoc/parser/test_markdown_parser.py

This document describes the parser selection system in RAGFlow, which uses a FACTORY pattern to route documents to appropriate parsing strategies based on file type and configuration. The system supports multiple file formats (PDF, DOCX, Excel, Markdown, etc.) and provides specialized parsing modes for different document types (books, papers, presentations, Q&A, etc.).

## Parser Selection Architecture

RAGFlow employs a two-level factory pattern for parser selection:

1. Chunking Strategy Selection: Based on the parser_id configuration, different chunking modules are loaded from the rag/app/ directory.
2. PDF Parser Selection: Within each chunking strategy, PDF documents can be parsed using different layout recognition engines or third-party integrations.

### Chunking Strategy Factory

The system provides specialized chunking strategies, each implemented as a module in `rag/app/`:

| Module | Strategy | Use Case |
| --- | --- | --- |
| `naive.py` | General/Naive | Default general-purpose parsing [rag/app/naive.py782-1330](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L782-L1330) |
| `book.py` | Book | Long-form content with hierarchical structure [rag/app/book.py63-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/book.py#L63-L175) |
| `paper.py` | Paper | Academic papers with title/abstract/sections [rag/app/paper.py149-230](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/paper.py#L149-L230) |
| `presentation.py` | Presentation | PPT/PDF slides (one page per chunk) [rag/app/presentation.py128-243](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/presentation.py#L128-L243) |
| `manual.py` | Manual | Technical documentation with section hierarchies [rag/app/manual.py137-234](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/manual.py#L137-L234) |
| `qa.py` | Q&A | Question-answer pairs from Excel/PDF/Docx [rag/app/qa.py36-75](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/qa.py#L36-L75) |
| `table.py` | Table | Spreadsheet data (Excel/CSV) [rag/app/table.py41-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L41-L130) |
| `laws.py` | Laws | Legal documents with numbered sections [rag/app/laws.py120-192](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/laws.py#L120-L192) |
| `one.py` | One | Entire document as single chunk [rag/app/one.py59-168](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/one.py#L59-L168) |

**Diagram: Chunking Strategy Selection Flow**

Title: "Chunking Strategy Selection Flow"

```

```

Sources: [rag/app/naive.py254-261](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L254-L261) [rag/app/naive.py782-1330](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L782-L1330) [rag/app/table.py41-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L41-L130) [rag/app/book.py63-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/book.py#L63-L175) [rag/app/one.py59-168](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/one.py#L59-L168)

### PDF Parser Factory Pattern

For PDF documents, the system uses a `PARSERS` dictionary that maps layout recognizer names to parser functions.

**Diagram: PDF Parser Factory**

Title: "PDF Parser Factory"

```

```

Sources: [rag/app/naive.py115-271](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L115-L271) [deepdoc/parser/pdf_parser.py57](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/pdf_parser.py#L57-L57) [deepdoc/parser/docling_parser.py35](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/docling_parser.py#L35-L35) [deepdoc/parser/tcadp_parser.py37](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/tcadp_parser.py#L37-L37)

## Supported File Formats

| Format | Extensions | Primary Parser | Key Features |
| --- | --- | --- | --- |
| **PDF** | `.pdf` | Multiple (DeepDOC, MinerU, Docling) | Layout analysis, OCR, table extraction |
| **Word** | `.docx`, `.doc` | `Docx` class [rag/app/naive.py286-512](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L286-L512) | Paragraph extraction, table parsing, image extraction via `DocxParser` |
| **Excel** | `.xlsx`, `.xls` | `Excel` class [rag/app/table.py41-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L41-L130) | Multi-sheet support, header detection, merged cells, image extraction [deepdoc/parser/excel_parser.py110-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/excel_parser.py#L110-L153) |
| **CSV** | `.csv` | `RAGFlowExcelParser` | Conversion to Workbook via pandas [deepdoc/parser/excel_parser.py31-49](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/excel_parser.py#L31-L49) |
| **Markdown** | `.md`, `.markdown` | `Markdown` class [rag/app/naive.py597-732](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L597-L732) | Table extraction, image URL parsing |
| **PowerPoint** | `.ppt`, `.pptx` | `RAGFlowPptParser` [rag/app/presentation.py142](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/presentation.py#L142-L142) | Slide-by-slide extraction, Tika fallback [rag/app/presentation.py153-171](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/presentation.py#L153-L171) |
| **EPUB** | `.epub` | `EpubParser` | Chapter-based parsing [deepdoc/parser/__init__.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/__init__.py) |

Sources: [rag/app/naive.py782-1330](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L782-L1330) [rag/app/table.py41-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L41-L130) [deepdoc/parser/excel_parser.py110-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/excel_parser.py#L110-L153) [rag/app/presentation.py140-171](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/presentation.py#L140-L171)

## PDF Parser Strategies

### 1. DeepDOC Parser (`by_deepdoc`)

The default parser using RAGFlow's proprietary layout analysis pipeline.

- Implementation: rag/app/naive.py115-127
- Class: Pdf in rag/app/naive.py (inheriting RAGFlowPdfParser) rag/app/naive.py254
- Pipeline:
__images__(): OCR processing viaOCRclass._layouts_rec(): Layout recognition viaLayoutRecognizerdeepdoc/parser/pdf_parser.py85-90_table_transformer_job(): Table structure detection viaTableStructureRecognizerdeepdoc/parser/pdf_parser.py91_text_merge(): Merge text boxes using XGBoost modelsdeepdoc/parser/pdf_parser.py93-102

### 2. MinerU Parser (`by_mineru`)

LLM-based OCR parser using MinerU model via the LLM integration system.

- Implementation: rag/app/naive.py130-183
- Model Access: Through LLMBundle with LLMType.OCR rag/app/naive.py153-155
- VLM Enhancement: Automatically triggers VLM descriptions for image chunks if IMAGE2TEXT model is configured rag/app/naive.py164-167

### 3. Docling Parser (`by_docling`)

Integration with IBM's Docling library for document understanding.

- Implementation: rag/app/naive.py186-204
- Class: DoclingParser deepdoc/parser/docling_parser.py35

## Parser Configuration System

Parser behavior is controlled through the `parser_config` dictionary and the standardized flow-based parameters.

### Pipeline/Flow-based Parser

The `ParserParam` class standardized setups for different media types in the workflow system.

**Standardized Setups**:

- PDF: Default parse_method is "deepdoc", supports json and markdown output rag/flow/parser/parser.py121-131
- Spreadsheet: Default output_format is "html", supports .xls, .xlsx, and .csv rag/flow/parser/parser.py132-141
- Word: Supports .doc and .docx with json or markdown output rag/flow/parser/parser.py142-158
- Image: Uses parse_method: "ocr", supports language selection and custom system prompts rag/flow/parser/parser.py198-205
- Audio: Supports formats like wav, mp3, aac, etc. rag/flow/parser/parser.py214-230

**Diagram: Parser Configuration Flow**

Title: "Parser Configuration Flow"

```

```

Sources: [rag/flow/parser/parser.py67-230](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/flow/parser/parser.py#L67-L230) [rag/app/naive.py762-774](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L762-L774) [common/parser_config_utils.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/parser_config_utils.py)

## Vision Enhancement and Context

RAGFlow supports visual enhancement for figures and tables using Vision-Language Models (VLM).

- Figure Description: VLMs generate descriptions for images found in PDF, Word, or Excel files deepdoc/parser/figure_parser.py44
- Context Attachment: The append_context2table_image4pdf function extracts surrounding text to provide context for tables and images during the description process rag/app/naive.py61
- Multi-modal Merging: Functions like naive_merge_with_images combine text and image data into unified chunks rag/app/naive.py55
- Excel Vision: Images embedded in Excel cells are extracted via _extract_images_from_worksheet and processed via vision_figure_parser_figure_xlsx_wrapper, with descriptions optionally replacing cell text rag/app/table.py58-68

Sources: [deepdoc/parser/figure_parser.py21-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/figure_parser.py#L21-L44) [rag/app/naive.py51-63](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L51-L63) [rag/app/table.py55-68](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L55-L68) [deepdoc/parser/excel_parser.py110-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/excel_parser.py#L110-L153)


---



<!-- ===== 6.2-chunking-methods ===== -->

# Chunking Methods

Relevant source files
- deepdoc/parser/excel_parser.py
- rag/app/book.py
- rag/app/laws.py
- rag/app/manual.py
- rag/app/naive.py
- rag/app/one.py
- rag/app/paper.py
- rag/app/presentation.py
- rag/app/qa.py
- rag/app/table.py
- rag/nlp/__init__.py
- test/unit_test/deepdoc/parser/test_excel_parser.py

## Purpose and Scope

This document explains the technical implementation and use cases for the various chunking strategies in RAGFlow. Chunking is the process of splitting parsed document content into discrete, semantically meaningful segments for indexing and retrieval. RAGFlow provides a variety of specialized chunking modes, each optimized for specific document structures like legal articles, academic papers, or spreadsheet rows.

For information about document parsing before chunking, see [6.1 Document Parsing Strategies](https://github.com/infiniflow/ragflow/blob/d32e05d5/6.1 Document Parsing Strategies) For information about embedding generation after chunking, see [6.3 Content Enhancement and Embedding](https://github.com/infiniflow/ragflow/blob/d32e05d5/6.3 Content Enhancement and Embedding)

## Chunking Modes Overview

RAGFlow implements chunking strategies as specialized modules within the `rag/app/` directory. Each module contains a `chunk()` function that orchestrates the parsing and splitting logic.

| Mode | Module | Primary Use Case | Key Features |
| --- | --- | --- | --- |
| **Naive** | `naive.py` | General documents | Token-aware splitting, delimiter-based, supports most formats [rag/app/naive.py763-1028](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L763-L1028) |
| **Manual** | `manual.py` | Structured documents | Preserved hierarchical structure via section levels [rag/app/manual.py137-236](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/manual.py#L137-L236) |
| **Q&A** | `qa.py` | Question-answer pairs | Extracts Q&A pairs using regex patterns [rag/app/qa.py319-466](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/qa.py#L319-L466) |
| **Table** | `table.py` | Structured data | Row-based chunking for Excel/CSV with image support [rag/app/table.py42-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L42-L130) |
| **Book** | `book.py` | Long-form content | Hierarchical merging, TOC removal [rag/app/book.py63-182](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/book.py#L63-L182) |
| **Laws** | `laws.py` | Legal documents | Tree-based structure preserving articles/sections [rag/app/laws.py120-217](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/laws.py#L120-L217) |
| **Paper** | `paper.py` | Academic papers | Metadata extraction (abstract, authors) [rag/app/paper.py149-260](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/paper.py#L149-L260) |
| **Presentation** | `presentation.py` | Slides/presentations | One page per chunk with visual context [rag/app/presentation.py128-243](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/presentation.py#L128-L243) |
| **One** | `one.py` | Complete document | Entire file as a single semantic unit [rag/app/one.py59-167](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/one.py#L59-L167) |
| **Knowledge Graph** | `knowledge_graph.py` | Relational data | Extracts entities and relations for GraphRAG [rag/app/knowledge_graph.py28-115](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/knowledge_graph.py#L28-L115) |

**Sources:** [rag/app/naive.py763-1028](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L763-L1028) [rag/app/manual.py137-236](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/manual.py#L137-L236) [rag/app/qa.py319-466](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/qa.py#L319-L466) [rag/app/table.py42-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L42-L130) [rag/app/book.py63-182](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/book.py#L63-L182) [rag/app/laws.py120-217](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/laws.py#L120-L217) [rag/app/paper.py149-260](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/paper.py#L149-L260) [rag/app/presentation.py128-243](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/presentation.py#L128-L243) [rag/app/one.py59-167](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/one.py#L59-L167)

## Chunking Architecture

The chunking process is orchestrated by the background task executor, which selects the application module based on the `parser_id` defined in the dataset configuration.

### Chunking Logic Flow

```
Common Processing (rag/nlp/)

Chunking Modules (rag/app/)

Chunking Router

Entry Point

naive

manual

qa

table

book

laws

paper

presentation

one

TaskExecutor
rag/svr/task_executor.py

parser_id
selection

naive.chunk()

manual.chunk()

qa.chunk()

table.chunk()

book.chunk()

laws.chunk()

paper.chunk()

presentation.chunk()

one.chunk()

Document Parsers
by_deepdoc, by_mineru, by_docling

rag_tokenizer
tokenize(), fine_grained_tokenize()

num_tokens_from_string()

attach_media_context()
```

**Sources:** [rag/app/naive.py115-184](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L115-L184) [rag/app/manual.py137-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/manual.py#L137-L173) [rag/nlp/__init__.py31](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L31-L31) [common/token_utils.py21](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/token_utils.py#L21-L21)

## Naive Chunking

Naive chunking provides general-purpose document splitting. It supports PDF, DOCX, TXT, Markdown, and HTML.

### Configuration Parameters

The `parser_config` controls the splitting behavior:

| Parameter | Default | Description |
| --- | --- | --- |
| `chunk_token_num` | 512 | Maximum tokens per chunk [rag/app/naive.py774](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L774-L774) |
| `delimiter` | `"\n!?。；！？"` | Delimiters for splitting [rag/app/naive.py774](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L774-L774) |
| `layout_recognize` | `"DeepDOC"` | Layout analysis engine [rag/app/naive.py149](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L149-L149) |

**Sources:** [rag/app/naive.py774-786](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L774-L786)

### Token-Aware Merging

The `naive_merge()` function implements splitting that respects token limits while preserving semantic boundaries:

1. Splitting: Uses configured delimiters to break text into segments.
2. Counting: Calculates tokens via num_tokens_from_string() common/token_utils.py21
3. Merging: Concatenates segments until reaching chunk_token_num.
4. Positioning: Tracks document coordinates via @@page\tleft\tright\ttop\tbottom## tags rag/app/naive.py821-825

**Sources:** [rag/nlp/__init__.py1145-1300](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L1145-L1300) [common/token_utils.py21](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/token_utils.py#L21-L21)

## Manual Chunking

Manual chunking preserves document hierarchy by detecting titles and heading levels.

### Structure Detection

Manual mode utilizes `bullets_category()` to detect heading patterns (e.g., `Chapter I`, `1.1`) [rag/nlp/__init__.py169-233](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L169-L233)

- PDF: Uses PdfParser (as Pdf class) to extract layout blocks and merges them based on vertical position rag/app/manual.py33-67
- DOCX: Uses docx_question_level() to build a hierarchical stack of questions and answers based on paragraph styles rag/app/manual.py70-134

**Sources:** [rag/app/manual.py33-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/manual.py#L33-L134) [rag/nlp/__init__.py169-233](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L169-L233)

## Q&A Chunking

Q&A chunking extracts question-answer pairs. For PDF files, it uses `QUESTION_PATTERN` to detect question bullets:

```
QUESTION_PATTERN = [
    r"第([零一二三四五六七八九十百0-9]+)问",
    r"第([零一二三四五六七八九十百0-9]+)条",
    r"<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/\\(（" undefined  file-path="\\(（">Hii</FileRef>[\)）]",
    # ... more patterns in rag/nlp/__init__.py:75-87
]
```

It identifies questions using `has_qbullet()` which checks for matches against these patterns and ensures sequence continuity [rag/nlp/__init__.py90-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L90-L130)

**Sources:** [rag/nlp/__init__.py75-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L75-L130) [rag/app/qa.py111-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/qa.py#L111-L112)

## Table Chunking

Table chunking treats rows in structured datasets (Excel, CSV) as chunks.

### Excel and Image Integration

The `Excel` class in `rag/app/table.py` handles spreadsheet parsing:

- Image Extraction: _extract_images_from_worksheet() pulls images from cells deepdoc/parser/excel_parser.py110-153
- Vision Parsing: Sends images to vision_figure_parser_figure_xlsx_wrapper() for LLM-based description rag/app/table.py59-68
- Row-to-Chunk: Converts each row into a text representation, optionally mapping cell images to their descriptions rag/app/table.py97-114

**Sources:** [rag/app/table.py42-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/table.py#L42-L130) [deepdoc/parser/excel_parser.py110-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/excel_parser.py#L110-L153)

## Presentation Chunking

Presentation mode treats each slide as a single chunk with a thumbnail.

### Slide Assembly

For PDF presentations, the `Pdf` class in `rag/app/presentation.py`:

1. Analysis: Runs layout and table analysis rag/app/presentation.py40-56
2. Grouping: Groups all text, tables, and figures by page_number rag/app/presentation.py59-99
3. Sorting: Sorts items on a page by (top, x0) to maintain visual order rag/app/presentation.py106-107
4. Rendering: Returns the full page text paired with the page image rag/app/presentation.py111-115

**Sources:** [rag/app/presentation.py35-115](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/presentation.py#L35-L115)

## Laws Chunking

The Laws method is designed for legal documents with rigid structures (Chapters, Sections, Articles).

### Tree Construction

1. Level Detection: docx_question_level() identifies the depth of paragraphs rag/app/laws.py65
2. Node Tree: Uses a Node class in rag/nlp/__init__.py to build a tree based on detected levels rag/app/laws.py82-83
3. Flattening: Flattens the tree into chunks while preserving hierarchical context rag/app/laws.py85

**Sources:** [rag/app/laws.py56-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/laws.py#L56-L85) [rag/nlp/__init__.py24-25](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L24-L25)

## Common Processing Utilities

### Context Attachment

`attach_media_context()` enriches table and image chunks with surrounding text within the configured token budget (`table_context_size` or `image_context_size`).

**Sources:** [rag/nlp/__init__.py409-704](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L409-L704)

### Vision Figure Parser

`VisionFigureParser` and its wrappers (e.g., `vision_figure_parser_pdf_wrapper`) utilize LLMs to generate descriptions for images and tables. In `by_mineru`, if a tenant has an `IMAGE2TEXT` model, it is used to enrich image chunks with semantic descriptions [rag/app/naive.py151-167](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L151-L167)

```
vision_figure_parser_pdf_wrapper

VisionFigureParser

LLMBundle (OCR/IMAGE2TEXT)

LLM API Call

Image Description

Enriched Chunk
```

**Sources:** [rag/app/naive.py114-120](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L114-L120) [rag/app/naive.py151-167](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/app/naive.py#L151-L167) [deepdoc/parser/figure_parser.py93-132](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/figure_parser.py#L93-L132)

### Tokenization

All methods use `rag_tokenizer` for unified segmentation and token counting.

```
common/token_utils.py

rag/nlp/init.py

rag_tokenizer

tokenize_chunks()

tokenize_table()

num_tokens_from_string()
```

**Sources:** [rag/nlp/__init__.py31](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L31-L31) [rag/nlp/__init__.py302-327](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/__init__.py#L302-L327) [common/token_utils.py21](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/token_utils.py#L21-L21)


---



<!-- ===== 6.3-content-enhancement-and-embedding ===== -->

# Content Enhancement and Embedding

Relevant source files
- api/apps/llm_app.py
- api/db/__init__.py
- api/db/db_models.py
- api/db/init_data.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/llm_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- conf/llm_factories.json
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/task_executor.py
- web/src/pages/user-setting/setting-model/constant.ts

## Purpose and Scope

This document describes the content enhancement and embedding phase of RAGFlow's document processing pipeline. After documents are parsed and chunked (see [6.1](https://github.com/infiniflow/ragflow/blob/d32e05d5/6.1) and [6.2](https://github.com/infiniflow/ragflow/blob/d32e05d5/6.2)), each chunk undergoes optional LLM-based enhancement to generate additional metadata, followed by mandatory vectorization via embedding models. These processes prepare chunks for storage and retrieval in the document store.

For information about advanced enhancement features like GraphRAG and RAPTOR, see [6.5](https://github.com/infiniflow/ragflow/blob/d32e05d5/6.5) For the retrieval process that uses these embeddings, see [9](https://github.com/infiniflow/ragflow/blob/d32e05d5/9)

## Architecture Overview

The enhancement and embedding pipeline operates on chunks produced by parsers, enriching them with LLM-generated features before vectorization. All operations are asynchronous and use semaphores for concurrency control.

### Content Enhancement and Embedding Pipeline

```
Chunk Entity Space

Embedding Process
(Mandatory)

LLM-Based Enhancement
(Optional)

Input Space

Parsed Chunks
(content_with_weight)

Keyword Extraction
rag.prompts.generator.keyword_extraction

Question Generation
rag.prompts.generator.question_proposal

Metadata Extraction
rag.prompts.generator.gen_metadata

Content Tagging
rag.prompts.generator.content_tagging

Text Preparation
rag.svr.task_executor.build_chunks

Title Embedding
emb_mdl.encode

Content Embedding
emb_mdl.encode

Weighted Combination
filename_embd_weight

important_kwd[]
important_tks

question_kwd[]
question_tks

metadata_obj{}

tag_fld{}

q_N_vec[]
(N = dimension)

Document Store
(Elasticsearch/Infinity/OceanBase)
```

**Sources:** [rag/svr/task_executor.py114-131](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L114-L131) [rag/svr/task_executor.py343-626](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L343-L626)

## LLM-Based Enhancement Features

Enhancement features are controlled by parser configuration settings in the `Knowledgebase` and `Document` models. They use chat models to extract structured information from chunk content.

### Keyword Extraction

The `auto_keywords` feature extracts salient keywords from each chunk using an LLM. Keywords are stored in raw list format (`important_kwd`) and tokenized form (`important_tks`) for full-text search.

**Implementation:**

```
"Chunk Object"
"rag/prompts/generator.py:keyword_extraction()"
"rag/graphrag/utils.py:get_llm_cache()"
"rag/svr/task_executor.py:build_chunks()"
"Chunk Object"
"rag/prompts/generator.py:keyword_extraction()"
"rag/graphrag/utils.py:get_llm_cache()"
"rag/svr/task_executor.py:build_chunks()"
alt
[Cache hit]
[Cache miss]
Check auto_keywords > 0
get_llm_cache(llm_name, content, "keywords", topn)
Cached keywords
keyword_extraction(mdl, content, topn)
"keyword1,keyword2,keyword3"
set_llm_cache(...)
important_kwd = split(",")
important_tks = tokenize(keywords)
```

- Concurrency: Uses chat_limiter semaphore to control parallel LLM requests rag/svr/task_executor.py92
- Caching: Keyed by (llm_name, content, "keywords", {"topn": N}) rag/svr/task_executor.py350-357
- Fields populated:
important_kwd: List of keyword strings.important_tks: Tokenized keywords for BM25 search viarag_tokenizer.tokenize()rag/svr/task_executor.py372-373

**Sources:** [rag/svr/task_executor.py343-375](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L343-L375) [rag/prompts/generator.py59-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L59-L60) [rag/graphrag/utils.py59](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/graphrag/utils.py#L59-L59)

### Question Generation

The `auto_questions` feature generates questions that each chunk could answer. This enables retrieval where user queries are matched against generated questions.

**Configuration:** Controlled by `parser_config.auto_questions` in the `Document` model.

| Field | Type | Purpose |
| --- | --- | --- |
| `question_kwd` | `list[str]` | Generated questions (one per line) |
| `question_tks` | Tokenized | Questions tokenized for full-text search |

The generation process:

1. Check LLM cache with key (llm_name, content, "question", {"topn": N}) rag/svr/task_executor.py384-391
2. If miss, call question_proposal(chat_mdl, content, topn) rag/svr/task_executor.py393-394
3. Split response by newlines to extract individual questions rag/svr/task_executor.py401-403
4. Tokenize questions using rag_tokenizer.tokenize() for search indexing rag/svr/task_executor.py405-406

**Sources:** [rag/svr/task_executor.py377-408](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L377-L408) [rag/prompts/generator.py59-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L59-L60) [api/db/db_models.py152-156](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L152-L156)

### Metadata Extraction

The `enable_metadata` feature extracts structured metadata fields defined in the dataset configuration via `gen_metadata` prompt [rag/prompts/generator.py60](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L60-L60)

**Metadata Aggregation:**

After extraction, metadata is aggregated across all chunks in the document:

```
common/metadata_utils.py

rag/svr/task_executor.py

Chunk 1 metadata_obj

Chunk 2 metadata_obj

Chunk N metadata_obj

update_metadata_to()
Merge all metadata_obj

DocMetadataService.get_metadata_for_documents()

Final Merged Metadata
```

1. Collect metadata_obj from all chunks rag/svr/task_executor.py438-440
2. Merge using update_metadata_to() which combines values intelligently common/metadata_utils.py46-47
3. Fetch any existing document metadata from DocMetadataService rag/svr/task_executor.py441-443
4. Store final metadata at document level via DocMetadataService.update_document_metadata() rag/svr/task_executor.py450-452

**Sources:** [rag/svr/task_executor.py410-452](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L410-L452) [common/metadata_utils.py46-47](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/metadata_utils.py#L46-L47) [api/db/services/doc_metadata_service.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/doc_metadata_service.py#L29-L29)

### Content Tagging

The `tag_kb_ids` feature tags chunks using a vocabulary extracted from specified knowledge bases.

**Tagging Strategy:**

1. Vector-based matching: Use nlp_search.Dealer.search() or similar retrieval mechanisms to find similar tagged chunks. If similarity exceeds threshold, reuse those tags rag/svr/task_executor.py483-490
2. LLM-based tagging: For chunks without matches, use LLM with few-shot examples via content_tagging() to generate tags rag/svr/task_executor.py504-517

**Sources:** [rag/svr/task_executor.py454-517](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L454-L517) [rag/prompts/generator.py59-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L59-L60)

## Embedding Process

All chunks must be embedded. The process combines title and content vectors with configurable weighting.

### Text Preparation

The system determines what text to embed for each chunk:

```
# Priority: questions > content
c = "\n".join(d.get("question_kwd", []))
if not c:
    c = d["content_with_weight"]
c = re.sub(r"</?(table|td|caption|tr|th)( [^<>]{0,12})?>", " ", c)
```

1. If questions were generated (question_kwd), embed those rag/svr/task_executor.py581-582
2. Otherwise, embed the chunk's content_with_weight rag/svr/task_executor.py583-584
3. Remove HTML table tags to clean the text rag/svr/task_executor.py584-585

**Sources:** [rag/svr/task_executor.py579-587](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L579-L587)

### Dual Embedding: Title and Content

RAGFlow embeds both document titles and chunk content, then combines them:

```
Vector Fusion

rag/svr/task_executor.py

rag/svr/task_executor.py

docnm_kwd (document name)

emb_mdl.encode([title])

Title Vector

Prepared Text

emb_mdl.encode(batch)

Content Vectors[]

filename_embd_weight (default: 0.1)

vect = title_w * title_vec + (1 - title_w) * content_vec

q_N_vec
```

**Weighting Formula:** The final embedding vector is: `vect = (title_w * title_vector) + ((1 - title_w) * content_vector)` The `filename_embd_weight` parameter (default 0.1) controls the title contribution [rag/svr/task_executor.py618-620](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L618-L620)

**Sources:** [rag/svr/task_executor.py589-625](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L589-L625) [rag/llm/embedding_model.py147-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/embedding_model.py#L147-L152)

### Batch Processing and Concurrency

Embedding uses batching for efficiency:

- Outer Batch: BATCH_SIZE (default 64) rag/svr/task_executor.py111
- Concurrency: embed_limiter semaphore limits simultaneous embedding operations rag/svr/task_executor.py98

**Sources:** [rag/svr/task_executor.py111](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L111-L111) [rag/svr/task_executor.py98](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L98-L98) [rag/svr/task_executor.py595-609](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L595-L609)

## Vector Field Naming Convention

Embedded vectors are stored with dynamic field names based on dimension: `q_<dimension>_vec`

When searching, the `Dealer` class constructs the vector column name based on the embedding data length [rag/nlp/search.py60-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L60-L61)

**Sources:** [rag/svr/task_executor.py622-625](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L622-L625) [rag/nlp/search.py59-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L59-L61)

## Caching Strategy

To minimize LLM API costs, enhancement operations use a caching strategy managed via Redis:

| Function | Code Entity | Purpose |
| --- | --- | --- |
| `get_llm_cache` | `rag/graphrag/utils.py` | Retrieves cached LLM results [rag/graphrag/utils.py59](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/graphrag/utils.py#L59-L59) |
| `set_llm_cache` | `rag/graphrag/utils.py` | Stores new LLM results [rag/graphrag/utils.py59](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/graphrag/utils.py#L59-L59) |

Cache keys are constructed using the LLM model name, the content text, the operation type (e.g., "keywords"), and parameters like `topn` [rag/svr/task_executor.py350-357](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L350-L357)

**Sources:** [rag/svr/task_executor.py350-357](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L350-L357) [rag/svr/task_executor.py384-391](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L384-L391) [rag/graphrag/utils.py59](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/graphrag/utils.py#L59-L59)

## Integration with Retrieval Pipeline

The enhanced fields are utilized during hybrid search in the `Dealer` class:

| Field | Search Usage | Code Entity |
| --- | --- | --- |
| `q_N_vec` | Vector similarity (dense) | `MatchDenseExpr` [rag/nlp/search.py61](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L61-L61) |
| `content_ltks` | Sparse text match | `FulltextQueryer` [rag/nlp/search.py168](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L168-L168) |
| `question_tks` | Sparse text match | `FulltextQueryer` [rag/nlp/search.py152](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L152-L152) |
| `important_tks` | Sparse text match | `FulltextQueryer` [rag/nlp/search.py150](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L150-L150) |

The retrieval system uses `FusionExpr` to combine dense and sparse scores [rag/nlp/search.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L25-L25)

**Sources:** [rag/nlp/search.py53-168](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L53-L168) [api/db/services/dialog_service.py49-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L50)

## Performance and Monitoring

### Concurrency Limits

Semaphores in `task_executor.py` ensure the system does not overwhelm LLM providers or local resources:

- task_limiter: Controls total concurrent tasks rag/svr/task_executor.py96
- chunk_limiter: Controls parallel chunk building rag/svr/task_executor.py97
- embed_limiter: Controls parallel embedding rag/svr/task_executor.py98

### Progress Reporting

The `set_progress` function updates the `Task` status in the database, providing real-time feedback to the user [rag/svr/task_executor.py165-190](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L190)

**Sources:** [rag/svr/task_executor.py95-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L95-L101) [rag/svr/task_executor.py165-197](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L197)


---



<!-- ===== 6.4-vision-processing:-ocr-and-layout-recognition ===== -->

# Vision Processing: OCR and Layout Recognition

Relevant source files
- agent/templates/stock_market_research_assistant.json
- api/apps/restful_apis/mcp_api.py
- common/parser_config_utils.py
- deepdoc/parser/figure_parser.py
- deepdoc/parser/mineru_parser.py
- deepdoc/parser/paddleocr_parser.py
- deepdoc/parser/pdf_parser.py
- deepdoc/parser/tcadp_parser.py
- deepdoc/vision/layout_recognizer.py
- deepdoc/vision/ocr.py
- deepdoc/vision/recognizer.py
- deepdoc/vision/t_ocr.py
- deepdoc/vision/t_recognizer.py
- deepdoc/vision/table_structure_recognizer.py
- rag/llm/ocr_model.py
- rag/settings.py
- test/fixtures/mineru/bmw_page_chrome_content_list.json
- test/testcases/restful_api/test_mcp_routes_unit.py
- test/testcases/test_web_api/test_mcp_server_app/test_mcp_server_app_unit.py
- test/unit_test/deepdoc/parser/test_mineru_parser.py
- web/src/components/layout-recognize-form-field.tsx
- web/src/components/paddleocr-options-form-field.tsx
- web/src/hooks/use-mcp-request.ts
- web/src/interfaces/database/mcp.ts
- web/src/interfaces/request/mcp.ts
- web/src/pages/user-setting/setting-model/modal/provider-modal/field-config/local-llm-configs.ts
- web/src/pages/user-setting/setting-model/modal/provider-modal/field-config/provider-config-map.ts
- web/src/pages/user-setting/setting-model/modal/provider-modal/field-config/utils.ts
- web/src/pages/user-setting/setting-model/modal/provider-modal/hooks/use-provider-modal-actions.ts
- web/src/pages/user-setting/setting-model/modal/provider-modal/types.ts
- web/src/pages/user-setting/setting-model/payload-utils.ts
- web/src/services/mcp-server-service.ts

This page documents RAGFlow's vision processing system for OCR (Optical Character Recognition) and layout analysis. The system identifies text regions, recognizes text content, classifies document layouts (titles, tables, figures, etc.), and detects table structures. For information about how parsed document content is chunked and embedded, see [6.3 Content Enhancement and Embedding](https://github.com/infiniflow/ragflow/blob/d32e05d5/6.3 Content Enhancement and Embedding) For details on different document parsing strategies, see [6.1 Document Parsing Strategies](https://github.com/infiniflow/ragflow/blob/d32e05d5/6.1 Document Parsing Strategies)

## OCR System Architecture

The OCR system consists of primary classes that detect and recognize text in images. It utilizes ONNX models for both text detection (`det.onnx`) and recognition (`rec.onnx`). The `OCR` class manages these models and provides high-level methods for full-page processing.

### System to Code Entity Space Mapping

The following diagram bridges the high-level OCR system components to their specific class implementations and model loading logic in the codebase.

Title: OCR System Entity Mapping

```
Model_Loading_Logic

TextRecognizer_Entity

TextDetector_Entity

OCR_Class_Space

OCR
deepdoc/vision/ocr.py

text_detector: List[TextDetector]
Per-GPU instances

text_recognizer: List[TextRecognizer]
Per-GPU instances

TextDetector
deepdoc/vision/ocr.py

predictor: ort.InferenceSession
ONNX model (det.onnx)

preprocess_op
DetResizeForTest, NormalizeImage

postprocess_op
DBPostProcess

TextRecognizer
deepdoc/vision/ocr.py

predictor: ort.InferenceSession
ONNX model (rec.onnx)

postprocess_op
CTCLabelDecode

rec_batch_num: 16
Batch processing

load_model()
deepdoc/vision/ocr.py:71-136

ort.InferenceSession
CPU or CUDA provider

loaded_models: dict
Global cache
```

**Sources:** [deepdoc/vision/ocr.py71-136](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L71-L136) [deepdoc/vision/ocr.py139-151](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L139-L151) [deepdoc/vision/ocr.py542-585](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L542-L585)

### OCR Class Initialization

The `OCR` class initializes multiple detector and recognizer instances based on `settings.PARALLEL_DEVICES`. Each instance can target a specific GPU via the `device_id` parameter, enabling parallel OCR processing across multiple documents or pages [deepdoc/vision/ocr.py71-136](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L71-L136)

## Text Detection and Recognition Pipeline

OCR operates in two stages: detection (locating text regions) and recognition (reading the text).

Title: OCR Execution Flow

```
ONNXRuntime
TextRecognizer
TextDetector
OCR
Caller
ONNXRuntime
TextRecognizer
TextDetector
OCR
Caller
loop
[For each bounding
box]
__call__(img, device_id)
__call__(img)
preprocess (DetResizeForTest, NormalizeImage)
predictor.run(det.onnx)
probability maps
postprocess (DBPostProcess)
Extract contours, filter boxes
dt_boxes (bounding boxes)
sorted_boxes()
Sort top-to-bottom, left-to-right
get_rotate_crop_image()
Crop and rotate text region
__call__(img_crop_list)
Batch preprocessing
resize_norm_img()
predictor.run(rec.onnx)
CTC predictions
postprocess_op (CTCLabelDecode)
Convert to text
rec_res (text, confidence)
Filter by drop_score (0.5)
List[(bbox, (text, confidence))]
```

**Sources:** [deepdoc/vision/ocr.py142-151](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L142-L151) [deepdoc/vision/ocr.py509-537](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L509-L537)

### Detection Stage (TextDetector)

The detector identifies text regions using a DB (Differentiable Binarization) model.

| Step | Method | Description |
| --- | --- | --- |
| 1. Preprocessing | `DetResizeForTest` | Resize image to max side length 960px |
| 2. Normalization | `NormalizeImage` | Mean=[0.485, 0.456, 0.406], Std=[0.229, 0.224, 0.225] |
| 3. Inference | `predictor.run()` | ONNX model outputs probability map |
| 4. Post-processing | `DBPostProcess` | Extract contours and filter by score |

### Recognition Stage (TextRecognizer)

The recognizer converts cropped text regions to strings using a CTC (Connectionist Temporal Classification) model. It processes regions in batches of 16 by default [deepdoc/vision/ocr.py142](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L142-L142)

### Garbled Text Detection

The system identifies garbled text using regex patterns, specifically looking for CID patterns like `(cid:123)` which often indicate font encoding issues in PDFs [deepdoc/vision/layout_recognizer.py70-71](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/layout_recognizer.py#L70-L71)

## Layout Recognition System

Layout recognition classifies document regions into semantic types. RAGFlow supports both ONNX-based and Ascend-based recognizers [deepdoc/vision/layout_recognizer.py48-90](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/layout_recognizer.py#L48-L90)

Title: Layout Recognition Components

```
Processing_Pipeline

preprocess()
Resize, pad, normalize

ONNX/Ascend inference

postprocess()
NMS, coordinate scaling

Tag OCR boxes with layout types

layouts_cleanup()
Remove overlapping layouts

Layout_Recognizer_Classes

Recognizer
deepdoc/vision/recognizer.py

LayoutRecognizer
layout_recognizer.py

AscendLayoutRecognizer
(Ascend NPU)
```

**Sources:** [deepdoc/vision/layout_recognizer.py33-46](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/layout_recognizer.py#L33-L46) [deepdoc/vision/recognizer.py31-52](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/recognizer.py#L31-L52) [deepdoc/parser/pdf_parser.py75-90](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/pdf_parser.py#L75-L90)

### Garbage Layout Filtering

The system identifies repeated headers, footers, and references as "garbage" if they appear frequently across pages, using a `Counter` to track duplicates [deepdoc/vision/layout_recognizer.py154-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/layout_recognizer.py#L154-L162)

## Table Structure Recognition

Table structure recognition (TSR) identifies rows, columns, and headers to enable structured data extraction [deepdoc/vision/table_structure_recognizer.py30-52](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/table_structure_recognizer.py#L30-L52)

Title: TSR Data Flow

```
Table_Construction_Logic

TableStructureRecognizer_Entity

TableStructureRecognizer
table_structure_recognizer.py

labels = [
'table', 'table column',
'table row', 'table column header',
'table projected row header',
'table spanning cell']

is_caption()
table_structure_recognizer.py

blockType()
table_structure_recognizer.py

construct_table()
table_structure_recognizer.py
```

**Sources:** [deepdoc/vision/table_structure_recognizer.py30-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/table_structure_recognizer.py#L30-L40) [deepdoc/vision/table_structure_recognizer.py114-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/table_structure_recognizer.py#L114-L118) [deepdoc/vision/table_structure_recognizer.py152-193](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/table_structure_recognizer.py#L152-L193)

### Table Component Classification

The `blockType` function categorizes table cells into types like `Dt` (Date), `Nu` (Number), `Ca` (Code/Address), or `Tx` (Text) based on regex patterns [deepdoc/vision/table_structure_recognizer.py121-150](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/table_structure_recognizer.py#L121-L150)

## Figure Processing and Vision LLM Integration

RAGFlow can enhance figure extraction by using Vision-Language Models (VLMs) to describe image content. The `VisionFigureParser` class manages this enhancement [deepdoc/parser/figure_parser.py59-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/figure_parser.py#L59-L60)

### VLM Enhancement Flow

The system attempts to find a tenant's default `IMAGE2TEXT` model via `get_tenant_default_model_by_type` [deepdoc/parser/figure_parser.py51-52](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/figure_parser.py#L51-L52) If available, it wraps the image and context into a `VisionFigureParser` which uses `LLMBundle` to generate descriptions [deepdoc/parser/figure_parser.py59-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/figure_parser.py#L59-L61)

**Sources:** [deepdoc/parser/figure_parser.py51-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/figure_parser.py#L51-L61) [deepdoc/parser/figure_parser.py93-132](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/figure_parser.py#L93-L132) [deepdoc/parser/figure_parser.py135-163](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/figure_parser.py#L135-L163)

## PDF Processing and Vision Integration

The `RAGFlowPdfParser` coordinates OCR, layout, and table recognition. It initializes `OCR`, `LayoutRecognizer`, and `TableStructureRecognizer` in its constructor [deepdoc/parser/pdf_parser.py70-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/pdf_parser.py#L70-L91)

### Concatenation Features

The parser uses an XGBoost model `updown_cnt_mdl` to determine if two text blocks should be concatenated [deepdoc/parser/pdf_parser.py92-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/pdf_parser.py#L92-L101) Features include layout type matches, vertical distance, and linguistic patterns derived from the `rag_tokenizer` [deepdoc/parser/pdf_parser.py131-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/pdf_parser.py#L131-L175)

## MinerU Integration

RAGFlow supports the MinerU parser as an alternative high-performance vision-based PDF parsing backend. It supports multiple processing backends including multimodel pipelines and various VLM engines [deepdoc/parser/mineru_parser.py88-93](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/mineru_parser.py#L88-L93)

| Backend | Description |
| --- | --- |
| `pipeline` | Traditional multimodel pipeline (default) |
| `vlm-transformers` | VLM using HuggingFace Transformers |
| `vlm-vllm-engine` | Local vLLM engine requiring GPU |
| `vlm-http-client` | HTTP client for remote vLLM servers |

**Sources:** [deepdoc/parser/mineru_parser.py84-93](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/mineru_parser.py#L84-L93) [deepdoc/parser/mineru_parser.py127-138](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/mineru_parser.py#L127-L138)

## PaddleOCR Integration

RAGFlow integrates PaddleOCR as an external API-based parsing option. This allows offloading vision tasks to a specialized PaddleOCR server [deepdoc/parser/paddleocr_parser.py116-128](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/paddleocr_parser.py#L116-L128)

### PaddleOCR Configuration

The `PaddleOCRConfig` class handles API URLs, access tokens, and algorithm selection (e.g., `PaddleOCR-VL`, `PP-OCRv5`) [deepdoc/parser/paddleocr_parser.py116-128](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/paddleocr_parser.py#L116-L128) The system maps internal configuration fields to the external API's expected format [deepdoc/parser/paddleocr_parser.py176-205](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/paddleocr_parser.py#L176-L205)

**Sources:** [deepdoc/parser/paddleocr_parser.py116-128](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/paddleocr_parser.py#L116-L128) [deepdoc/parser/paddleocr_parser.py176-205](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/paddleocr_parser.py#L176-L205)

## Model Loading and Device Management

ONNX Runtime is used for inference, supporting both CPU and CUDA providers. The system provides explicit control over thread counts and memory allocation to prevent resource oversubscription in multi-worker environments.

| Parameter | Environment Variable | Default | Description |
| --- | --- | --- | --- |
| GPU Mem Limit | `OCR_GPU_MEM_LIMIT_MB` | 2048 | Max VRAM per session [deepdoc/vision/ocr.py107](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L107-L107) |
| Intra-op Threads | `OCR_INTRA_OP_NUM_THREADS` | 2 | Parallelism within an operator [deepdoc/vision/ocr.py100](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L100-L100) |
| Inter-op Threads | `OCR_INTER_OP_NUM_THREADS` | 2 | Parallelism between operators [deepdoc/vision/ocr.py101](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L101-L101) |
| Arena Strategy | `OCR_ARENA_EXTEND_STRATEGY` | `kNextPowerOfTwo` | Memory allocation strategy [deepdoc/vision/ocr.py108](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L108-L108) |
| Arena Shrinkage | `OCR_GPUMEM_ARENA_SHRINKAGE` | - | If set to "1", releases VRAM after execution [deepdoc/vision/ocr.py122-123](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L122-L123) |

**Sources:** [deepdoc/vision/ocr.py71-136](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/vision/ocr.py#L71-L136) [deepdoc/parser/pdf_parser.py70-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/pdf_parser.py#L70-L91) [deepdoc/parser/pdf_parser.py131-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/deepdoc/parser/pdf_parser.py#L131-L175)


---



<!-- ===== 6.5-advanced-features:-graphrag-and-raptor ===== -->

# Advanced Features: GraphRAG and RAPTOR

Relevant source files
- api/db/__init__.py
- api/db/db_models.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- common/asyncio_utils.py
- common/doc_store/doc_store_base.py
- common/doc_store/infinity_conn_pool.py
- common/settings.py
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- rag/graphrag/general/smoke.py
- rag/graphrag/light/smoke.py
- rag/graphrag/search.py
- rag/graphrag/utils.py
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/task_executor.py
- rag/utils/ob_conn.py
- test/unit_test/rag/graphrag/test_graphrag_utils.py

## Purpose and Scope

This document covers two advanced retrieval enhancement features in RAGFlow: **GraphRAG** and **RAPTOR**, along with visualization tools like **Mindmaps**. These features improve retrieval performance on complex documents by creating additional index structures beyond standard chunk-based vector search.

- GraphRAG: Constructs knowledge graphs from document content through entity and relationship extraction, enabling graph-based traversal for complex queries.
- RAPTOR: Builds hierarchical summaries through recursive clustering and summarization, creating multi-level representations for better long-context retrieval.
- Mindmap Generation: Uses the hierarchical structure to generate visual representations of document clusters.

Both features are optional enhancements configured per dataset via the `parser_config` object and executed as asynchronous background tasks managed by the `task_executor.py` [rag/svr/task_executor.py134-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L134-L140)

## RAPTOR: Recursive Abstractive Processing for Tree-Organized Retrieval

### Overview

RAPTOR creates a tree-structured index by recursively clustering document chunks and generating abstractive summaries at each level. This hierarchical organization enables retrieval at multiple granularities: original chunks for specific details and summary nodes for broader context.

The implementation resides primarily in `rag/raptor.py` [rag/raptor.py156](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L156-L156) with execution orchestration in the task executor layer.

### Architecture

```
Code_Entities

RAPTOR_Pipeline

Configuration

parser_config['raptor']

_max_cluster

_threshold

_max_token

_prompt

_tree_builder

Original Chunks
(content + embeddings)

GaussianMixture / AHC
RecursiveAbstractiveProcessing4TreeOrganizedRetrieval

LLM Summarization
_chat()

Embedding Generation
_embedding_encode()

class RecursiveAbstractiveProcessing4TreeOrganizedRetrieval

sklearn.mixture.GaussianMixture

sklearn.cluster.AgglomerativeClustering

umap.umap_.UMAP

Infinity / Elasticsearch / OceanBase

rag/svr/task_executor.py
```

**RAPTOR Architecture and Code Entity Mapping**

Sources: [rag/raptor.py159-182](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L159-L182) [rag/svr/task_executor.py136](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L136-L136) [rag/raptor.py22-24](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L22-L24)

### Configuration Parameters

RAPTOR is configured through parameters passed to the `RecursiveAbstractiveProcessing4TreeOrganizedRetrieval` constructor, typically sourced from the Knowledge Base parser settings:

| Parameter | Type | Description |
| --- | --- | --- |
| `max_cluster` | `int` | Maximum number of clusters per level. |
| `threshold` | `float` | Similarity threshold for clustering. |
| `max_token` | `int` | Maximum tokens in generated summaries. |
| `prompt` | `str` | LLM prompt template for summarization. |
| `tree_builder` | `str` | Strategy for building the tree (`raptor` or `psi`). |

Sources: [rag/raptor.py159-182](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L159-L182) [rag/raptor.py38-45](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L38-L45)

### Execution Flow

The `RecursiveAbstractiveProcessing4TreeOrganizedRetrieval` class implements the logic for building the hierarchical tree.

1. Task Triggering: The task_executor.py identifies a raptor task type and maps it to PipelineTaskType.RAPTOR rag/svr/task_executor.py136
2. Skip Logic: The system checks should_skip_raptor to avoid redundant processing if the document has not changed rag/svr/task_executor.py55
3. Clustering and Summarization: Chunks are grouped using Gaussian Mixture Models (GMM) or Agglomerative Hierarchical Clustering (AHC) rag/raptor.py23-24 For each cluster, an LLM generates a summary which is then embedded and indexed back into the document store.
4. Indexing: RAPTOR chunks are assigned a "fake" document ID GRAPH_RAPTOR_FAKE_DOC_ID to distinguish them from standard document chunks during retrieval api/db/services/task_service.py39

Sources: [rag/svr/task_executor.py134-136](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L134-L136) [rag/raptor.py156-182](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L156-L182) [api/db/services/task_service.py39](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L39-L39)

## GraphRAG and Mindmaps

### Knowledge Graph Extraction

GraphRAG tasks are triggered in the `task_executor.py` when the task type is `graphrag` [rag/svr/task_executor.py136](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L136-L136) It utilizes specialized extractors to process entities and relationships. The extraction process is governed by the `kg_limiter` to manage concurrency [rag/svr/task_executor.py100-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L100-L101)

### Mindmap Generation

Mindmaps are generated based on the hierarchical structure of document clusters or extracted entities. The `MindMapExtractor` [api/db/services/dialog_service.py45](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L45-L45) is used within the dialog service to extract hierarchical concepts for visualization during a chat session.

```
Data_Persistence

Code_Entity_Space

Natural_Language_Space

User Question

Retrieved Chunks

class MindMapExtractor

class DialogService

rag/svr/task_executor.py

rag/graphrag/general/graph_extractor.py

MySQL: Task table

Infinity / Elasticsearch / OceanBase
```

**GraphRAG and Mindmap Code Entity Mapping**

Sources: [api/db/services/dialog_service.py45](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L45-L45) [rag/svr/task_executor.py136-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L136-L137) [api/db/db_models.py152-156](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L152-L156)

### Implementation Details

1. Task Orchestration: The task_executor.py maps specific task labels to pipeline types: raptor, graphrag, and mindmap rag/svr/task_executor.py134-140
2. Entity Filtering: The Dealer class in rag/nlp/search.py supports filtering by graph-specific metadata fields such as knowledge_graph_kwd, entity_kwd, from_entity_kwd, and to_entity_kwd rag/nlp/search.py126-130
3. Search Integration: During retrieval, the Dealer.search method can include knowledge_graph_kwd in the source fields to fetch graph metadata alongside text chunks rag/nlp/search.py151-153

Sources: [rag/svr/task_executor.py134-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L134-L140) [rag/nlp/search.py126-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L126-L153)

## Integration and Caching

### Caching Mechanism

To minimize LLM costs and improve performance, RAGFlow implements a caching layer for GraphRAG and RAPTOR operations.

- LLM Cache: Stores summarization and extraction results via get_llm_cache and set_llm_cache rag/svr/task_executor.py59
- Embedding Cache: Stores vector results for chunks via get_embed_cache and set_embed_cache rag/raptor.py32-35
- Rate Limiting: GraphRAG LLM calls are governed by chat_limiter to prevent exceeding provider API limits during massive parallel extraction rag/svr/task_executor.py92

Sources: [rag/svr/task_executor.py59-92](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L59-L92) [rag/raptor.py30-36](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L30-L36)

### Data Storage for Advanced Features

GraphRAG and RAPTOR results are stored in the underlying document store (Elasticsearch, Infinity, or OceanBase) using specialized fields:

| Field Name | Description | Source |
| --- | --- | --- |
| `knowledge_graph_kwd` | Stores extracted entity/relationship data for GraphRAG. | [rag/nlp/search.py126](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L126-L126) |
| `entity_kwd` | Keyword field for entity filtering. | [rag/nlp/search.py126](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L126-L126) |
| `from_entity_kwd` | Source entity in a relationship. | [rag/nlp/search.py126](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L126-L126) |
| `to_entity_kwd` | Target entity in a relationship. | [rag/nlp/search.py126](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L126-L126) |
| `PAGERANK_FLD` | Stores importance scores for graph nodes. | [rag/svr/task_executor.py103](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L103-L103) |

Sources: [rag/nlp/search.py126-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L126-L130) [rag/svr/task_executor.py103](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L103-L103)


---



<!-- ===== 6.6-data-source-connectors ===== -->

# Data Source Connectors

Relevant source files
- api/apps/restful_apis/connector_api.py
- api/db/services/connector_service.py
- common/constants.py
- common/data_source/__init__.py
- common/data_source/blob_connector.py
- common/data_source/config.py
- common/data_source/confluence_connector.py
- common/data_source/discord_connector.py
- common/data_source/dropbox_connector.py
- common/data_source/jira/connector.py
- common/data_source/models.py
- common/data_source/notion_connector.py
- common/data_source/slack_connector.py
- common/data_source/utils.py
- common/http_client.py
- rag/svr/sync_data_source.py
- rag/utils/opendal_conn.py
- test/testcases/restful_api/test_connector_routes_unit.py
- test/testcases/test_web_api/test_connector_app/test_connector_routes_unit.py
- test/unit_test/common/test_dropbox_connector.py
- test/unit_test/data_source/test_slack_connector_unit.py
- web/src/assets/svg/data-source/slack.svg
- web/src/pages/user-setting/data-source/constant/index.tsx
- web/src/pages/user-setting/data-source/constant/s3-constant.tsx

RAGFlow provides a robust external data source connector system that allows users to synchronize content from over 30 external platforms directly into their knowledge bases. This system automates the ingestion of documents, spreadsheets, code repositories, and communication logs, integrating them into the RAGFlow document processing pipeline.

## Overview

The connector system is built on a multi-tier architecture that handles authentication, periodic polling, incremental synchronization, and error recovery. It abstracts various APIs (REST, GraphQL, WebDAV, etc.) into a unified `Document` model that RAGFlow can parse and index.

### Supported Sources

Supported sources are defined in the `FileSource` enum [common/constants.py124-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/constants.py#L124-L161) and `DocumentSource` enum [common/data_source/config.py41-75](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/data_source/config.py#L41-L75) They include:

- Storage: S3, Google Cloud Storage, OCI Storage, R2, Dropbox, Box, Seafile, WebDAV, OneDrive, Azure Blob.
- Collaboration: Notion, Confluence, Jira, Airtable, Asana, Zendesk, Moodle, SharePoint, Salesforce.
- Communication: Slack, Discord, Gmail, IMAP, Outlook, Teams.
- Development: GitHub, GitLab, Bitbucket.
- Databases: MySQL, PostgreSQL, DingTalk AI Table.
- Web: RSS, REST API.

## System Architecture and Data Flow

The connector system bridges the gap between external API spaces and RAGFlow's internal storage and task execution layers.

### Code Entity Mapping

The following diagram maps the logical components to their specific code implementations.

**Diagram: Connector System Code Entities**

```
Connector_Implementation

Service_&_DB_Layer

API_Layer_(Python/Flask)

Web_Frontend_(React)

DataSource_(web/src/pages/user-setting/data-source/index.tsx)

DataSourceKey_(web/src/pages/user-setting/data-source/constant/index.tsx)

DynamicForm_(web/src/components/dynamic-form.tsx)

connector_api.py_(api/apps/restful_apis/connector_api.py)

SyncBase_(rag/svr/sync_data_source.py)

ConnectorService_(api/db/services/connector_service.py)

SyncLogsService_(api/db/services/connector_service.py)

MySQL/PostgreSQL_db_models.py

interfaces.py_(common/data_source/interfaces.py)

models.py_(common/data_source/models.py)

ConfluenceConnector_/GoogleDriveConnector/_etc.
```

Sources: [web/src/pages/user-setting/data-source/constant/index.tsx18-53](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/user-setting/data-source/constant/index.tsx#L18-L53) [api/db/services/connector_service.py38-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/connector_service.py#L38-L40) [rag/svr/sync_data_source.py107-115](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/sync_data_source.py#L107-L115) [common/data_source/__init__.py62-100](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/data_source/__init__.py#L62-L100)

### Synchronization Pipeline

The synchronization process is managed by `rag/svr/sync_data_source.py`. It runs as a background worker that polls for scheduled tasks. The worker uses an `asyncio.Semaphore` named `task_limiter` to control concurrency, defaulting to 5 concurrent tasks [rag/svr/sync_data_source.py86-87](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/sync_data_source.py#L86-L87)

**Diagram: Sync Pipeline Data Flow**

```
"DocumentService_(api/db/services/document_service.py)"
"ConcreteConnector_(e.g._ConfluenceConnector)"
"SyncLogsService_(api/db/services/connector_service.py)"
"SyncBase_(rag/svr/sync_data_source.py)"
"DocumentService_(api/db/services/document_service.py)"
"ConcreteConnector_(e.g._ConfluenceConnector)"
"SyncLogsService_(api/db/services/connector_service.py)"
"SyncBase_(rag/svr/sync_data_source.py)"
loop
[Document Batches]
"SyncLogsService.start(task_id, connector_id)"
"task_limiter_(asyncio.Semaphore)"
"_run_task_logic(task)"
"yield_List[Document]"
"DocumentService.insert(doc_data)"
"update_by_id(task_id, stats)"
"SyncLogsService.schedule(connector_id, kb_id, poll_range_start)"
```

Sources: [rag/svr/sync_data_source.py151-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/sync_data_source.py#L151-L180) [api/db/services/connector_service.py94-127](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/connector_service.py#L94-L127) [rag/svr/sync_data_source.py86-87](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/sync_data_source.py#L86-L87)

## Core Components

### 1. The Connector Interface and Models

Every connector must implement or extend the logic found in `common/data_source/`. Key models include:

- Document: The standard data structure containing id, blob (content), extension, size_bytes, and metadata common/data_source/models.py89-108
- SyncBase: The base class for synchronization tasks in the server layer rag/svr/sync_data_source.py107
- BlobStorageConnector: Specialized base for object storage sources like S3, R2, and GCS common/data_source/__init__.py26
- TextSection & ImageSection: Used for structured extraction from sources like Discord or Notion common/data_source/models.py77-87

### 2. Database Services

`ConnectorService` and `SyncLogsService` manage the persistence of connector configurations and execution history.

- ConnectorService.schedule_tasks(): Determines the poll_range_start based on previous successful tasks and schedules new SYNC or PRUNE tasks api/db/services/connector_service.py94-127
- ConnectorService.rebuild(): Deletes all documents associated with a connector in a specific KB and schedules a full re-index api/db/services/connector_service.py141-152
- ConnectorService.cleanup_stale_documents_for_task(): Identifies and removes documents from RAGFlow that no longer exist in the external source by comparing against a remote snapshot (file_list) api/db/services/connector_service.py155-184
- SyncLogsService.start(): Initializes a log entry and sets the status to RUNNING api/db/services/connector_service.py228-243

### 3. Authentication & OAuth

RAGFlow handles complex OAuth2 flows for services like Google Drive, Gmail, Box, and Confluence.

- Token Handling: Connectors like OnyxConfluence implement custom credential renewal logic using Redis for distributed locking common/data_source/confluence_connector.py126-176
- Credential Management: CredentialsProviderInterface defines how connectors retrieve secrets dynamically or statically common/data_source/confluence_connector.py95-103
- Frontend Fields: Specific components like GoogleDriveTokenField, GmailTokenField, and BoxTokenField handle token acquisition in the UI web/src/pages/user-setting/data-source/constant/index.tsx8-10
- Secure HTTP: The common/http_client.py utility redacts sensitive parameters like client_secret and access_token from URLs before logging to prevent credential leakage common/http_client.py58-89

### 4. Frontend Integration

The frontend uses a `DataSourceKey` enum to manage visibility and configuration for 30+ sources [web/src/pages/user-setting/data-source/constant/index.tsx18-53](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/user-setting/data-source/constant/index.tsx#L18-L53)

- Feature Visibility: DataSourceFeatureVisibilityMap determines which sources support advanced features like syncDeletedFiles web/src/pages/user-setting/data-source/constant/index.tsx61-163
- Iconography: The system uses a centralized generateDataSourceInfo function to provide names, descriptions, and icons for the UI web/src/pages/user-setting/data-source/constant/index.tsx176-240

## Implementation Details: Confluence and Discord

### Confluence Connector

- Expansion Support: Uses specific fields like body.storage.value and history.lastUpdated to fetch complete page content common/data_source/config.py101-108
- Incremental Sync: Uses CONFLUENCE_TIMEZONE_OFFSET and CONFLUENCE_SYNC_TIME_BUFFER_SECONDS to ensure no updates are missed during overlapping poll windows common/data_source/config.py198-204
- OAuth Renewal: Automatically refreshes Cloud tokens using confluence_refresh_tokens when they reach half their expiration time common/data_source/confluence_connector.py165-176

### Discord Connector

- Message Conversion: Transforms Discord messages and thread history into Document objects with metadata like "Channel" and "Thread" common/data_source/discord_connector.py31-77
- Async Bridge: Uses _manage_async_retrieval to bridge the asynchronous discord.py client into the synchronous iteration pattern required by the sync pipeline common/data_source/discord_connector.py156-189

## Configuration Constants

Connectors are governed by several environment variables and constants in `common/data_source/config.py`:

- INDEX_BATCH_SIZE: Number of documents processed in one batch (Default: 2) common/data_source/config.py113
- REQUEST_TIMEOUT_SECONDS: Global timeout for external API requests (Default: 60s) common/data_source/config.py17
- CONTINUE_ON_CONNECTOR_FAILURE: If true, a single failed document won't stop the entire sync task common/data_source/config.py148-151
- BLOB_STORAGE_SIZE_THRESHOLD: 20MB limit for blob storage indexing common/data_source/config.py112

Sources: [rag/svr/sync_data_source.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/sync_data_source.py) [api/db/services/connector_service.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/connector_service.py) [common/data_source/models.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/data_source/models.py) [common/data_source/config.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/data_source/config.py) [common/constants.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/constants.py) [web/src/pages/user-setting/data-source/constant/index.tsx](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/user-setting/data-source/constant/index.tsx) [common/data_source/confluence_connector.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/data_source/confluence_connector.py) [common/data_source/discord_connector.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/data_source/discord_connector.py) [common/http_client.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/http_client.py)


---



<!-- ===== 7-backend-api-system ===== -->

# 7. Backend API System

Relevant source files
- api/apps/__init__.py
- api/apps/backward_compat.py
- api/settings.py
- api/utils/api_utils.py
- api/utils/validation_utils.py
- conf/service_conf.yaml
- docker/service_conf.yaml.template
- docs/references/http_api_reference.md
- docs/references/python_api_reference.md
- docs/release_notes.md
- sdk/python/ragflow_sdk/modules/__init__.py
- sdk/python/ragflow_sdk/modules/agent.py
- sdk/python/ragflow_sdk/modules/base.py
- sdk/python/ragflow_sdk/modules/chat.py
- sdk/python/ragflow_sdk/modules/chunk.py
- sdk/python/ragflow_sdk/modules/dataset.py
- sdk/python/ragflow_sdk/modules/document.py
- sdk/python/ragflow_sdk/modules/session.py
- sdk/python/ragflow_sdk/ragflow.py
- test/testcases/test_web_api/test_system_app/test_apps_init_unit.py
- web/src/components/document-preview/md/index.tsx
- web/src/constants/markdown-remark-plugins.ts

## Overview

The Backend API System provides programmatic access to RAGFlow's functionality through RESTful HTTP endpoints and a Python SDK. It enables developers to manage datasets, documents, chat assistants, agents, and execute RAG workflows without using the web interface. The API follows REST conventions and includes OpenAI-compatible endpoints for seamless integration with existing LLM tooling.

The system is built on the **Quart** framework, which provides an asynchronous implementation of the Flask API. This allows RAGFlow to handle long-lived connections for streaming LLM responses and concurrent background tasks efficiently [api/apps/__init__.py61-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L61-L85)

For information about the visual workflow system and canvas-based agent orchestration, see [Agent and Workflow System](/infiniflow/ragflow/8-agent-and-workflow-system). For details on the web frontend that consumes these APIs, see [Frontend Application](/infiniflow/ragflow/4-frontend-application).

## Scope and Purpose

This page covers:

- Overall API architecture and design patterns.
- Request/response formats and error handling.
- Endpoint organization across functional domains.
- Python SDK structure and design.
- OpenAI compatibility layer.

Detailed coverage of specific endpoint categories is provided in child pages:

- API Architecture and SDK — Document the API design, request/response patterns, SDK structure, and OpenAPI compatibility.
- Authentication and Authorization — Explain the authentication flow (JWT, OAuth, API tokens), the @login_required decorator, and current_user proxy.
- User and Tenant Management — Document user registration, OAuth integration, password reset flow with OTP, and multi-tenant architecture.
- Dataset and Knowledge Base APIs — Detail the dataset CRUD operations, parser configuration, metadata settings, and knowledge base management endpoints.
- Document and File Management APIs — Explain document upload, parsing triggers, file hierarchy, metadata updates, and the relationship between files and documents.
- Chat and Conversation APIs — Document the chat completion endpoint, session management, streaming responses, and the RAG retrieval flow.
- Agent and Memory APIs — Detail the agent/canvas management endpoints and memory system APIs for persistent agent state.
- System Status and Health Monitoring — Explain the health check endpoints, component status reporting, task executor heartbeat monitoring, and the MCP server integration.

## API Layer Architecture

The Backend API is structured to separate routing logic from business logic and database operations. It leverages `QuartSchema` for OpenAPI support [api/apps/__init__.py64-65](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L64-L65)

Title: API Layer Architecture

```

```

**Sources:** [api/apps/__init__.py61-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L61-L85) [api/apps/backward_compat.py50-57](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/backward_compat.py#L50-L57) [api/utils/api_utils.py45-52](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L45-L52) [api/db/db_models.py26-27](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L26-L27)

## Request Processing Flow

Every API request follows a standardized processing pipeline to ensure security and consistency.

1. Authentication: API requests typically use the @login_required decorator. This validates either a JWT from the Authorization: Bearer header, a session cookie, or an APIToken api/apps/__init__.py129-188
2. Request Parsing: The helper get_request_json() api/utils/api_utils.py90-91 asynchronously fetches the JSON body, utilizing _coerce_request_data() to handle various content types and caching the result api/utils/api_utils.py60-88
3. Validation: Modern endpoints use validate_and_parse_json_request which employs Pydantic models for schema validation api/utils/validation_utils.py36-112
4. Business Logic: Handlers invoke specific restful API modules. For example, deprecated_chat_completions forwards to chat_api.session_completion api/apps/backward_compat.py89-104
5. Response Formatting: Success results are returned via get_json_result(data=...) api/apps/backward_compat.py60-63 while errors use server_error_response(e) api/utils/api_utils.py133-151

**Sources:** [api/apps/__init__.py129-188](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L129-L188) [api/utils/api_utils.py60-93](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L60-L93) [api/utils/validation_utils.py36-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/validation_utils.py#L36-L112) [api/apps/backward_compat.py50-63](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/backward_compat.py#L50-L63)

## Standard Response and Error Codes

RAGFlow uses a uniform response envelope. Success is indicated by HTTP 200 and a specific code in the JSON body.

| Code | Message | Description |
| --- | --- | --- |
| `200` | (Success) | Request processed successfully. |
| `400` | `Bad Request` | Invalid request parameters [docs/references/http_api_reference.md20](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L20-L20) |
| `401` | `Unauthorized` | Invalid or missing authentication credentials [docs/references/http_api_reference.md21](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L21-L21) |
| `403` | `Forbidden` | Access denied to the specific resource [docs/references/http_api_reference.md22](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L22-L22) |
| `1001` | `Invalid Chunk ID` | The specified chunk identifier does not exist [docs/references/http_api_reference.md25](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L25-L25) |
| `1002` | `Chunk Update Failed` | Failure during the manual editing of a chunk [docs/references/http_api_reference.md26](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L26-L26) |

**Sources:** [docs/references/http_api_reference.md14-26](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L14-L26) [api/utils/api_utils.py118-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L118-L130)

## Python SDK and Object Mapping

The `ragflow-sdk` provides an object-oriented interface that mirrors the REST API hierarchy, bridging Natural Language space to code entities.

Title: SDK to Backend Mapping

```
"sdk/python/ragflow_sdk/ragflow.py"

"sdk/python/ragflow_sdk/modules/session.py"

"sdk/python/ragflow_sdk/modules/dataset.py"

"sdk/python/ragflow_sdk/modules/session.py"

RAGFlow

+api_key: str

+api_url: str

+create_dataset()

+create_chat()

DataSet

+id: str

+chunk_method: str

+parser_config: ParserConfig

+upload_documents()

Document

+id: str

+name: str

+list_chunks()

Session

+id: str

+ask(question, stream)

+update(update_message)

Message
```

**Key Features:**

- Resource Management: High-level methods like create_dataset sdk/python/ragflow_sdk/ragflow.py56-84 and list_chats sdk/python/ragflow_sdk/ragflow.py155-185
- Configuration: Allows setting chunk_method (e.g., naive, qa, paper) and parser_config during creation docs/references/python_api_reference.md165-181
- Document Pipeline: Document.add_chunk() allows manual chunk insertion sdk/python/ragflow_sdk/modules/document.py90-98 while list_chunks() retrieves indexed segments sdk/python/ragflow_sdk/modules/document.py78-88
- Conversation Interface: The Session.ask() method handles both Agent and Chat sessions, supporting SSE streaming sdk/python/ragflow_sdk/modules/session.py40-143

**Sources:** [sdk/python/ragflow_sdk/ragflow.py27-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/ragflow.py#L27-L185) [sdk/python/ragflow_sdk/modules/session.py25-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/session.py#L25-L181) [sdk/python/ragflow_sdk/modules/document.py23-105](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/document.py#L23-L105) [docs/references/python_api_reference.md124-212](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/python_api_reference.md?plain=1#L124-L212)

## OpenAI Compatibility Layer

RAGFlow implements a compatibility layer to allow standard LLM clients to interact with RAGFlow "Chats" and "Agents".

- Endpoint Paths:
Chats:/api/v1/openai/{chat_id}/chat/completionsdocs/references/http_api_reference.md62Agents:/api/v1/agents/chat/completionswithopenai-compatible: trueapi/apps/backward_compat.py125-143- Extended Parameters: Supports extra_body for RAG-specific features like reference (to return citations) and metadata_condition for complex retrieval filtering docs/references/http_api_reference.md96-112
- Backward Compatibility: A dedicated module api/apps/backward_compat.py maintains legacy v0.24.0 paths by forwarding them to new RESTful implementations api/apps/backward_compat.py16-45

**Sources:** [docs/references/http_api_reference.md30-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L30-L112) [api/apps/backward_compat.py16-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/backward_compat.py#L16-L143)

## Deployment and Configuration

The API server is configured through environment variables and integrated with the Quart framework.

- Timeout Configuration: Quart timeouts are configurable via QUART_RESPONSE_TIMEOUT (default 600s) to accommodate slow LLM responses from local backends api/apps/__init__.py71-73
- Session Management: Uses Redis for permanent session storage via SESSION_TYPE = "redis" api/apps/__init__.py77-80
- JSON Serialization: Employs a CustomJSONEncoder to handle complex objects like database models and specialized types api/apps/__init__.py68
- Validation Pipeline: Uses validate_and_parse_json_request to enforce schema constraints before logic execution api/utils/validation_utils.py36-112

**Sources:** [api/apps/__init__.py61-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L61-L85) [api/utils/api_utils.py54-59](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L54-L59) [api/utils/validation_utils.py36-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/validation_utils.py#L36-L112)


---



<!-- ===== 7.1-api-architecture-and-sdk ===== -->

# API Architecture and SDK

Relevant source files
- api/apps/__init__.py
- api/apps/backward_compat.py
- api/settings.py
- api/utils/api_utils.py
- api/utils/validation_utils.py
- conf/service_conf.yaml
- docker/service_conf.yaml.template
- docs/references/http_api_reference.md
- docs/references/python_api_reference.md
- docs/release_notes.md
- sdk/python/ragflow_sdk/modules/__init__.py
- sdk/python/ragflow_sdk/modules/agent.py
- sdk/python/ragflow_sdk/modules/base.py
- sdk/python/ragflow_sdk/modules/chat.py
- sdk/python/ragflow_sdk/modules/chunk.py
- sdk/python/ragflow_sdk/modules/dataset.py
- sdk/python/ragflow_sdk/modules/document.py
- sdk/python/ragflow_sdk/modules/session.py
- sdk/python/ragflow_sdk/ragflow.py
- test/testcases/test_web_api/test_system_app/test_apps_init_unit.py
- web/src/components/document-preview/md/index.tsx
- web/src/constants/markdown-remark-plugins.ts

## Purpose and Scope

This document describes RAGFlow's RESTful HTTP API architecture and Python SDK implementation. It covers the API server structure, authentication mechanisms (JWT tokens and API keys), request/response patterns, error handling, and how the Python SDK (`ragflow-sdk`) wraps HTTP endpoints for programmatic access.

For information about specific API endpoints and their parameters, see the HTTP API Reference [docs/references/http_api_reference.md8-183](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L8-L183) and Python API Reference [docs/references/python_api_reference.md8-245](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/python_api_reference.md?plain=1#L8-L245)

## API Server Architecture

### Quart-Based Application Framework

RAGFlow's API server is built on **Quart**, an asynchronous Python web framework compatible with Flask's API but supporting async/await patterns. The application is initialized in `api/apps/__init__.py`.

**Application Initialization**

```
Middleware_and_Error_Handling

Request_Validation_Layer

Application_Initialization

Quart App
[api/apps/init.py:61]

CORS Configuration
allow_origin='*'
[api/apps/init.py:62]

QuartSchema
OpenAPI Support
[api/apps/init.py:65]

App Configuration
RESPONSE_TIMEOUT=600s
BODY_TIMEOUT=600s
[api/apps/init.py:73-74]

validate_and_parse_json_request
[api/utils/validation_utils.py:36]

Pydantic Models
Schema enforcement
[api/utils/validation_utils.py:101]

server_error_response
[api/utils/api_utils.py:133]

CustomJSONEncoder
[api/utils/json_encode.py]

Session Configuration
Redis-backed
[api/apps/init.py:78-80]
```

Sources: [api/apps/__init__.py61-86](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L61-L86) [api/utils/api_utils.py133-151](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L133-L151) [api/utils/validation_utils.py36-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/validation_utils.py#L36-L112)

The server supports:

- Async request handling for non-blocking I/O operations.
- Extended timeouts (600s default) to accommodate slow LLM responses (e.g., local Ollama on CPU) api/apps/__init__.py73-74
- CORS enabled for cross-origin requests api/apps/__init__.py62
- OpenAPI/Swagger schema generation via QuartSchema api/apps/__init__.py65
- Global Error Handling: Unhandled exceptions are caught by server_error_response which classifies errors like Unauthorized or index_not_found_exception api/utils/api_utils.py133-151

## Authentication System

### Dual Authentication Mechanism

RAGFlow implements two parallel authentication methods within the `_load_user` proxy. It supports standard JWT-based sessions for the web UI and persistent API keys for SDK access.

**Authentication Flow Diagram**

```
API_Key_Authentication

JWT_Token_Authentication

Authentication_Resolution

Bearer/JWT

API Key

None

HTTP Request

Authorization Header
[api/apps/init.py:153]

_load_user()
[api/apps/init.py:146]

JWT Token Path

API Key Path

Session Fallback

Serializer.loads()
SECRET_KEY decryption
[api/apps/init.py:190]

UserService.query()
access_token match
[api/apps/init.py:200]

APIToken.query()
token match
[api/apps/init.py:174]

UserService.query()
tenant_id lookup
[api/apps/init.py:176]

current_user
LocalProxy
[api/apps/init.py:228]

_load_user_from_session()
[api/apps/init.py:112]
```

Sources: [api/apps/__init__.py112-228](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L112-L228) [api/db/db_models.py26](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L26-L26)

#### JWT Token Authentication (Web UI)

Used by the web frontend after user login. The `_load_user` function attempts to deserialize the token from the `Authorization` header using `URLSafeTimedSerializer` [api/apps/__init__.py189-206](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L189-L206)

#### API Key Authentication (Programmatic Access)

Used by SDK clients. If JWT decryption fails, the system checks the `APIToken` table for a matching key (including Beta tokens) to resolve the `tenant_id` and `current_user` [api/apps/__init__.py172-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L172-L185)

## HTTP API Structure

### Request and Response Standardization

RAGFlow standardizes web APIs to RESTful conventions. Recent versions have migrated many endpoints to a new structure while maintaining a backward compatibility layer.

- Request Parsing: _coerce_request_data handles JSON bodies and form-data fallback api/utils/api_utils.py60-88
- Validation: The system uses Pydantic-based validation via validate_and_parse_json_request to ensure payload integrity before processing api/utils/validation_utils.py36-112
- Error Codes: Standard error codes are used for resource failures (e.g., 401 for Unauthorized, 404 for Not Found, 1001 for Invalid Chunk ID) docs/references/http_api_reference.md18-26
- Backward Compatibility: Deprecated routes (e.g., /api/v1/file/upload -> /api/v1/files) are maintained through a backward compatibility layer docs/references/http_api_reference.md30-53

### OpenAI-Compatible API

RAGFlow provides endpoints that follow the OpenAI request and response format.

- Endpoint: POST /api/v1/openai/{chat_id}/chat/completions docs/references/http_api_reference.md62
- Extra Body: Supports extra_body for RAG-specific features like reference (citations) and metadata_condition (filtering) docs/references/http_api_reference.md96-112

## Python SDK Architecture

### SDK Structure

The `ragflow-sdk` provides an object-oriented interface to the API server, abstracting the raw HTTP calls into module-based classes.

**SDK to Code Entity Mapping**

```
Server_Entity_Space

SDK_Module_Space

SDK_Client_Space

POST /datasets

POST /chats

POST /agents/{id}/sessions

POST /agents/chat/completions

POST /chats/{id}/completions

POST /datasets/{id}/documents

RAGFlow Client
[ragflow_sdk/ragflow.py:27]

DataSet Module
[modules/dataset.py]

Chat Module
[modules/chat.py]

Agent Module
[modules/agent.py]

Session Module
[modules/session.py]

Memory Module
[modules/memory.py]

Document Module
[modules/document.py]

agent_api
[api/apps/restful_apis/agent_api.py]

chat_api
[api/apps/restful_apis/chat_api.py]

dataset_api
[api/apps/restful_apis/dataset_api.py]
```

Sources: [sdk/python/ragflow_sdk/ragflow.py27-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/ragflow.py#L27-L185) [sdk/python/ragflow_sdk/modules/session.py25-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/session.py#L25-L181) [sdk/python/ragflow_sdk/modules/document.py23-51](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/document.py#L23-L51)

### Key SDK Operations

- Session Management: The Session class handles both chat and agent types, routing the ask() method to _ask_chat() or _ask_agent() sdk/python/ragflow_sdk/modules/session.py101-106
- Streaming: Supports Server-Sent Events (SSE) by iterating over lines, parsing data: prefixes, and yielding Message objects sdk/python/ragflow_sdk/modules/session.py108-137
- Document Management: The Document class allows for document updates, chunk listing, and manual chunk addition via add_chunk() sdk/python/ragflow_sdk/modules/document.py53-98
- Agent Execution: The _ask_agent method targets /agents/chat/completions with openai-compatible set to False to use RAGFlow's native workflow engine sdk/python/ragflow_sdk/modules/session.py170-180

## Summary Table: SDK to API Mapping

| Feature | SDK Method | HTTP Endpoint |
| --- | --- | --- |
| Create Dataset | `create_dataset()` | `POST /api/v1/datasets` [sdk/python/ragflow_sdk/ragflow.py80](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/ragflow.py#L80-L80) |
| List Datasets | `list_datasets()` | `GET /api/v1/datasets` [sdk/python/ragflow_sdk/ragflow.py99](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/ragflow.py#L99-L99) |
| Create Chat | `create_chat()` | `POST /api/v1/chats` [sdk/python/ragflow_sdk/ragflow.py136](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/ragflow.py#L136-L136) |
| Chat Completion | `Session._ask_chat()` | `POST /api/v1/chats/{chat_id}/completions` [sdk/python/ragflow_sdk/modules/session.py166](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/session.py#L166-L166) |
| Agent Completion | `Session._ask_agent()` | `POST /api/v1/agents/chat/completions` [sdk/python/ragflow_sdk/modules/session.py179](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/session.py#L179-L179) |
| Document Update | `Document.update()` | `PATCH /api/v1/datasets/{ds_id}/documents/{doc_id}` [sdk/python/ragflow_sdk/modules/document.py57](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/document.py#L57-L57) |
| Health Check | N/A | `GET /api/v1/system/healthz` [docs/references/http_api_reference.md41](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L41-L41) |

Sources: [sdk/python/ragflow_sdk/ragflow.py27-203](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/ragflow.py#L27-L203) [sdk/python/ragflow_sdk/modules/session.py163-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/session.py#L163-L180) [sdk/python/ragflow_sdk/modules/document.py53-63](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/ragflow_sdk/modules/document.py#L53-L63) [docs/references/http_api_reference.md30-53](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/references/http_api_reference.md?plain=1#L30-L53)


---



<!-- ===== 7.2-authentication-and-authorization ===== -->

# Authentication and Authorization

Relevant source files
- api/apps/__init__.py
- api/settings.py
- api/utils/api_utils.py
- cmd/admin_server.go
- conf/service_conf.yaml
- docker/service_conf.yaml.template
- internal/admin/handler.go
- internal/admin/password.go
- internal/admin/router.go
- internal/admin/service.go
- internal/cli/admin_command.go
- internal/common/error_code.go
- internal/common/smtp_config.go
- internal/common/status_message.go
- internal/dao/user.go
- internal/engine/elasticsearch/client.go
- internal/handler/user.go
- internal/server/config.go
- internal/service/user.go
- internal/utility/captcha_png.go
- internal/utility/otp.go
- internal/utility/smtp.go
- internal/utility/token.go
- test/testcases/test_web_api/test_system_app/test_apps_init_unit.py
- web/src/components/document-preview/md/index.tsx
- web/src/constants/markdown-remark-plugins.ts

This document describes RAGFlow's multi-method authentication and authorization system. RAGFlow supports three primary authentication mechanisms: API key authentication (including Beta tokens), session-based authentication for web UI interactions, and OAuth/OIDC for single-sign-on integration. It also covers the specialized Go-based Admin Server authentication.

## Overview

RAGFlow implements a flexible authentication architecture that serves different client types:

- JWT/Session Authentication: Used by the React web frontend. It supports JWT decoding and falls back to server-side session cookies managed via Redis api/apps/__init__.py79-80
- API Key Authentication: Used by the Python SDK and HTTP API clients via the APIToken model api/apps/__init__.py172-185
- Go Admin Authentication: A dedicated path for the Admin system where only superusers can log in, using itsdangerous compatible tokens internal/admin/handler.go136-163
- OAuth/OIDC Authentication: Supports dynamic provider integration (GitHub, etc.) for SSO api/apps/restful_apis/user_api.py167-206

Authorization is tenant-based. Each authenticated user is associated with a `tenant_id` that controls access to datasets, chat assistants, and other resources.

Sources: [api/apps/__init__.py96-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L96-L109) [api/apps/restful_apis/user_api.py63-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/user_api.py#L63-L142) [internal/admin/handler.go146-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L146-L152)

## Authentication Methods

### API and Beta Token Authentication

API keys are used for programmatic access. Each key is stored in the `APIToken` table. The system distinguishes between standard `token` and `beta` tokens.

#### Token Validation Flow

The `_load_user` function in `api/apps/__init__.py` handles extraction from the `Authorization` header. It supports both raw tokens and the `Bearer ` prefix [api/apps/__init__.py158-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L158-L165)

Title: Token Resolution in api.apps._load_user

```
Found

Not Found

Success

Fail

Found

Not Found

HTTP Request with
'Authorization' Header

Extract auth_token
(Strip 'Bearer ')

Try Beta Token?

APIToken.query(beta=auth_token)

Try JWT?

Serializer.loads(auth_token)

Try API Token?

APIToken.query(token=auth_token)

Set g.user and g.auth_type

Return _load_user_from_session()
```

Implementation details for API token lookup:

```
# From api/apps/__init__.py:204-214
try:
    objs = APIToken.query(token=auth_token)
    if objs:
        user = UserService.query(id=objs[0].tenant_id, status=StatusEnum.VALID.value)
        if user:
            g.auth_type = AUTH_API
            g.user = user[0]
            return user[0]
except Exception as e_api_token:
    logging.warning(f"load_user from api token got exception {e_api_token}")
```

Sources: [api/apps/__init__.py153-220](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L153-L220) [api/db/db_models.py26](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L26-L26)

### Session-based Authentication

The web UI uses session-based authentication. When a user logs in, a unique `access_token` (UUID) is generated and stored in the database [api/apps/restful_apis/user_api.py127-131](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/user_api.py#L127-L131)

#### User Login Flow

Title: Web Login Sequence

```
"Quart Session"
"api.db.services.user_service.UserService"
"api.apps.restful_apis.user_api.login"
"Web Client"
"Quart Session"
"api.db.services.user_service.UserService"
"api.apps.restful_apis.user_api.login"
"Web Client"
alt
[Success]
[Failure]
POST /auth/login {email, password}
decrypt(password)
query_user(email, password)
User object
user.access_token = get_uuid()
login_user(user)
session['_user_id'] = user.id
user.save()
200 OK + User Info
401 Unauthorized
```

If a request lacks an `Authorization` header, the system invokes `_load_user_from_session()`, which retrieves the `_user_id` from the session cookie and validates the user's `access_token` against the database [api/apps/__init__.py112-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L112-L143)

Sources: [api/apps/__init__.py112-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L112-L143) [api/apps/restful_apis/user_api.py63-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/user_api.py#L63-L142)

### Admin Server Authentication (Go)

The Admin Server, implemented in Go, utilizes a separate authentication path. It requires the user to have `IsSuperuser` set to true [internal/admin/handler.go146-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L146-L152)

- Login: internal/admin/Handler.Login verifies credentials and generates a signed token using utility.DumpAccessToken, which is compatible with Python's itsdangerous internal/admin/handler.go163-173
- Logout: internal/admin/Service.Logout invalidates the token by prefixing it with INVALID_ in the database internal/admin/service.go95-99
- Middleware: AuthMiddleware (used in internal/admin/router.go) calls internal/admin/Service.GetUserByToken to validate the Authorization header internal/admin/router.go55 internal/admin/service.go189-204

Sources: [internal/admin/handler.go124-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L124-L185) [internal/admin/service.go93-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L93-L101) [internal/admin/service.go189-204](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L189-L204)

## Authorization and Decorators

### @login_required Decorator

The `@login_required` decorator protects Python endpoints. It allows specifying required authentication types (e.g., `AUTH_JWT`, `AUTH_API`) [api/apps/__init__.py223-228](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L223-L228)

```
# From api/apps/__init__.py:231-245
def login_required(auth_types=None):
    def wrapper(func):
        @wraps(func)
        async def decorated_function(*args, **kwargs):
            if current_app.config.get("LOGIN_DISABLED"):
                return await current_app.ensure_async(func)(*args, **kwargs)
 
            if _load_user(auth_types):
                return await current_app.ensure_async(func)(*args, **kwargs)
            
            # Handle unauthorized...
            return get_json_result(code=RetCode.UNAUTHORIZED, message="Unauthorized")
        return decorated_function
    return wrapper
```

### The current_user Proxy

The `current_user` is a `werkzeug.local.LocalProxy` that provides access to the authenticated `User` object stored in `g.user` [api/apps/__init__.py265-266](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L265-L266)

| Entity | Role | Source |
| --- | --- | --- |
| `_load_user` | Core logic for resolving user from headers/session | [api/apps/__init__.py146](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L146-L146) |
| `current_user` | Proxy to the authenticated User object | [api/apps/__init__.py266](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L266-L266) |
| `g.user` | Context-local storage for the user instance | [api/apps/__init__.py167](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L167-L167) |

Sources: [api/apps/__init__.py223-266](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L223-L266)

## Frontend Parameter Injection

The web frontend automatically adds tenant-specific model IDs to requests using `addTenantParams`. This ensures that even if a user selects a generic model name, the backend receives the correct `tenant_llm_id` for authorization and quota tracking [web/src/utils/llm-util.ts103-144](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/llm-util.ts#L103-L144)

Title: Frontend Model Parameter Mapping

```
Maps llm_id to

tenant_llm_id

Frontend UI Selection

getCachedLlmList()

web.src.utils.llm-util.addTenantParams

Outgoing API Request

Backend API
```

Sources: [web/src/utils/llm-util.ts103-144](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/llm-util.ts#L103-L144)

## Configuration and Error Handling

- Secret Key: app.secret_key is set via settings.get_secret_key() and used for JWT serialization api/apps/__init__.py84-85 In Go, the secret key is retrieved via server.GetSecretKey(cache.Get()) internal/admin/handler.go154
- Error Responses: server_error_response specifically catches 401/Unauthorized errors and returns a standardized JSON response with RetCode.UNAUTHORIZED api/utils/api_utils.py133-141 In the Go Admin server, errorResponse handles returning 401 status codes for unauthenticated requests internal/admin/handler.go84-89

Sources: [api/apps/__init__.py84-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/__init__.py#L84-L85) [api/utils/api_utils.py133-141](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/api_utils.py#L133-L141) [internal/admin/handler.go154](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L154-L154) [internal/admin/handler.go191](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L191-L191)


---



<!-- ===== 7.3-user-and-tenant-management ===== -->

# User and Tenant Management

Relevant source files
- api/apps/restful_apis/user_api.py
- api/constants.py
- api/db/__init__.py
- api/db/db_models.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- api/utils/nickname_validation.py
- cmd/admin_server.go
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- internal/admin/handler.go
- internal/admin/password.go
- internal/admin/router.go
- internal/admin/service.go
- internal/cli/admin_command.go
- internal/common/error_code.go
- internal/common/smtp_config.go
- internal/common/status_message.go
- internal/dao/user.go
- internal/engine/elasticsearch/client.go
- internal/handler/user.go
- internal/server/config.go
- internal/service/user.go
- internal/utility/captcha_png.go
- internal/utility/otp.go
- internal/utility/smtp.go
- internal/utility/token.go
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/task_executor.py
- test/testcases/restful_api/test_user_tenant_routes_unit.py
- test/testcases/test_web_api/test_user_app/test_user_app_unit.py
- test/unit_test/api/utils/test_nickname_validation.py
- web/src/pages/user-setting/profile/constants.ts
- web/src/pages/user-setting/profile/hooks/use-profile.ts
- web/src/pages/user-setting/profile/index.tsx

## Purpose and Scope

This document describes RAGFlow's user and tenant management system, covering user registration, authentication methods (password, OAuth, API tokens), and the multi-tenant architecture that provides workspace isolation. It details how the system initializes new workspaces and maintains isolation across different tenants. The implementation spans the Python-based API layer and the Go-based service layer for administrative and high-performance operations.

## Multi-Tenant Architecture

RAGFlow implements a multi-tenant system where each user can belong to multiple tenants (workspaces). Each tenant operates as an isolated workspace with its own resources, configuration, and access control.

### Entity Relationships

The following diagram illustrates the relationship between users, tenants, and their isolated resources, mapping logical concepts to code entities.

```
belongs_to

contains

owns

manages

User[api/db/db_models.py]

string

ID

PK

string

Email

UK

string

Password

Hashed via HashPassword()

string

Nickname

string

AccessToken

UK

bool

IsSuperuser

Checked by is_admin()

UserTenant[api/db/db_models.py]

string

ID

PK

string

TenantID

FK

string

UserID

FK

string

Role

Owner|Invite|Normal

Tenant[api/db/db_models.py]

string

ID

PK

string

Name

string

LLMID

Default Chat Model

string

EmbdID

Default Embedding Model

string

ASRID

Default ASR Model

string

Img2TxtID

Default Vision Model

Knowledgebase[api/db/db_models.py]

Document[api/db/db_models.py]
```

**Multi-Tenant Isolation**: Each tenant has its own namespace. The `UserTenant` entity tracks access and roles, such as `owner`, `invite`, or `normal` [api/db/__init__.py23-24](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/__init__.py#L23-L24) Isolation is enforced at the service layer; for example, `KnowledgebaseService` filters visibility based on the user's joined tenant IDs and the team permission status [api/db/services/knowledgebase_service.py52-68](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L52-L68)

**Sources**: [api/db/db_models.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L25-L25) [api/db/__init__.py23-24](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/__init__.py#L23-L24) [api/db/services/knowledgebase_service.py52-68](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L52-L68)

## User Registration and Initialization

User registration creates a complete workspace environment. In the Go implementation, `UserService.Register` performs an atomic transaction to set up the identity, the default tenant, and the root file system structure.

### Registration and Workspace Flow

The system uses `UserService` to manage the creation of identities and their associated environments.

**Atomic Registration Components**:

1. User Record: Stores profile and hashed credentials. The Go service uses HashPassword to secure credentials before storage internal/service/user.go134-137
2. Tenant Record: Creates the primary workspace, often named "[Nickname]'s Kingdom" internal/service/user.go173 It initializes default model IDs from the system configuration internal/service/user.go175-206
3. UserTenant Link: Establishes the user as the owner of the newly created tenant internal/service/user.go211-218
4. Root Folder: Every tenant is initialized with a root folder (/) in the File table to support document management internal/service/user.go224-233
5. Knowledge Base Creation: Users can then create Knowledge Bases within their tenant. The KnowledgebaseService ensures that only the creator can delete a specific dataset api/db/services/knowledgebase_service.py72-102

**Sources**: [internal/service/user.go109-218](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/user.go#L109-L218) [internal/service/user.go224-233](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/user.go#L224-L233) [api/db/services/knowledgebase_service.py72-102](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L72-L102)

## Authentication and Password Reset

RAGFlow supports traditional password-based login and a secure password reset flow using One-Time Passwords (OTP).

### Authentication Flow

The system utilizes an `access_token` for session management. `UserService.query` includes logic to reject empty, short, or invalidated tokens (those prefixed with `INVALID_`) [api/db/services/user_service.py46-66](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/user_service.py#L46-L66)

### Password Reset with OTP

For password recovery, the system implements a flow involving email verification and OTP generation.

```
"SMTPUtil [internal/utility/smtp.go]"
"OTPUtil [internal/utility/otp.go]"
"UserService [internal/service/user.go]"
"UserHandler [internal/handler/user.go]"
"User"
"SMTPUtil [internal/utility/smtp.go]"
"OTPUtil [internal/utility/otp.go]"
"UserService [internal/service/user.go]"
"UserHandler [internal/handler/user.go]"
"User"
Request Password Reset (Email)
Generate OTP
Create 6-digit Code
OTP Code
Send Email via SendEmail()
Email with OTP
Submit New Password + OTP
Verify OTP and Update Password
HashPassword(new_password)
Success
```

**Implementation Details**:

- OTP Generation: internal/utility/otp.go provides functions for generating cryptographically secure 6-digit codes.
- Email Delivery: internal/utility/smtp.go handles the SMTP connection and template rendering for the reset email.
- Verification: The system verifies the OTP against a cached value (often in Redis) before allowing the password update via HashPassword internal/service/user.go134-137

**Sources**: [internal/service/user.go134-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/user.go#L134-L137) [internal/utility/otp.go1-20](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/utility/otp.go#L1-L20) [internal/utility/smtp.go1-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/utility/smtp.go#L1-L30) [api/db/services/user_service.py46-66](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/user_service.py#L46-L66)

## Administrative Management

RAGFlow provides administrative capabilities for system-wide user and tenant oversight.

- Superuser Access: The UserService.is_admin method checks the is_superuser flag on the user record api/db/services/user_service.py159-162
- Admin Server: A dedicated Go-based admin server (cmd/admin_server.go) provides endpoints for system health and user management.
- CLI Management: The internal/cli/admin_command.go implements CLI tools for administrators to manage users and services directly from the terminal.

**Sources**: [api/db/services/user_service.py159-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/user_service.py#L159-L162) [cmd/admin_server.go1-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/cmd/admin_server.go#L1-L40) [internal/cli/admin_command.go1-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/admin_command.go#L1-L50)

## User Settings and Profile Management

Users can manage their profiles through the system, which interacts with `UserService`.

- Profile Updates: UserService.update_user allows modification of user metadata, automatically updating update_time and update_date api/db/services/user_service.py138-144
- Language and Theming: Users can set preferred language, color_schema, and timezone internal/service/user.go83-91
- Soft Deletion: The system supports deactivating users by setting their status to 0 instead of hard deletion api/db/services/user_service.py131-134

**Sources**: [api/db/services/user_service.py138-144](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/user_service.py#L138-L144) [internal/service/user.go83-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/user.go#L83-L91) [api/db/services/user_service.py131-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/user_service.py#L131-L134)


---



<!-- ===== 7.4-dataset-and-knowledge-base-apis ===== -->

# Dataset and Knowledge Base APIs

Relevant source files
- api/apps/restful_apis/dataset_api.py
- api/apps/services/dataset_api_service.py
- test/benchmark/chat.py
- test/testcases/configs.py
- test/testcases/test_http_api/common.py
- test/testcases/test_http_api/test_dataset_management/test_create_dataset.py
- test/testcases/test_http_api/test_dataset_management/test_delete_datasets.py
- test/testcases/test_http_api/test_dataset_management/test_knowledge_graph.py
- test/testcases/test_http_api/test_dataset_management/test_list_datasets.py
- test/testcases/test_http_api/test_dataset_management/test_update_dataset.py
- test/testcases/test_http_api/test_file_management_within_dataset/test_update_document.py
- test/testcases/test_http_api/test_session_management/test_chat_completions_openai.py
- test/testcases/test_http_api/test_session_management/test_related_questions.py
- test/testcases/test_sdk_api/test_dataset_mangement/test_create_dataset.py
- test/testcases/test_sdk_api/test_dataset_mangement/test_delete_datasets.py
- test/testcases/test_sdk_api/test_dataset_mangement/test_list_datasets.py
- test/testcases/test_sdk_api/test_dataset_mangement/test_update_dataset.py
- test/testcases/test_sdk_api/test_file_management_within_dataset/test_update_document.py
- test/testcases/test_web_api/test_dataset_management/test_dataset_sdk_routes_unit.py
- web/src/components/top-select.tsx
- web/src/hooks/use-knowledge-request.ts
- web/src/pages/dataset/dataset/generate-button/hook.ts
- web/src/pages/dataset/testing/index.tsx
- web/src/pages/dataset/testing/testing-form.tsx
- web/src/pages/dataset/testing/testing-result.tsx
- web/src/pages/next-search/hooks.ts
- web/src/pages/next-search/search-view.tsx

## Purpose and Scope

This page documents the REST API endpoints and underlying service architecture for creating, configuring, and managing datasets (also known as knowledge bases) in RAGFlow. Datasets serve as logical containers for documents and define essential processing parameters including parsing strategies, embedding models, and access permissions.

The dataset management system bridges the gap between the React-based web interface and the Python service layer, providing both high-level management and low-level configuration of parsing pipelines and metadata extraction.

## Dataset Resource Model

A dataset in RAGFlow is represented by the `Knowledgebase` model defined in `api/db/db_models.py`. It encapsulates the following technical properties:

- Identity: Unique ID, name, description, and avatar api/db/db_models.py595-605
- Parsing Configuration: parser_id (chunk method) and parser_config for method-specific settings api/db/db_models.py611-615
- Embedding Model: The specific model (embd_id) used to vectorize document chunks api/db/db_models.py610
- Access Control: Permission level (me or team) api/db/db_models.py616
- Statistics: Denormalized counters for document count, chunk count, and size api/db/db_models.py618-621
- Advanced Features: Settings for RAPTOR, GraphRAG, and Mindmap indices api/apps/services/dataset_api_service.py38-54 api/db/db_models.py623-625

### Dataset-Document-Chunk Relationship

Title: Dataset Hierarchy and Storage Mapping

```
Storage_Layer

Document_Layer

Dataset_Layer

1:N

1:N

1:N

Knowledgebase_(Dataset)
id, name, parser_id, embd_id
parser_config, permission

Document_1
kb_id, parser_id
name, size, status

Document_2

Document_N

Chunks_in_DocStore
Elasticsearch/Infinity/OceanBase
kb_id, doc_id, vectors

Raw_Files_in_MinIO
kb_id, location

Metadata_in_MySQL/PostgreSQL
kb_id, doc_id
```

**Sources:** [api/db/db_models.py595-754](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L595-L754) [api/db/services/knowledgebase_service.py32-48](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L32-L48) [api/db/services/document_service.py45-76](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L45-L76)

## Core Dataset Operations

### Create Dataset

**Endpoint:** `POST /api/v1/datasets`

Creates a new dataset. The request is processed through `create_dataset` in `api/apps/services/dataset_api_service.py`, which handles metadata configuration and embedding model verification [api/apps/services/dataset_api_service.py57-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L57-L110) It supports `auto_metadata_config` to automatically extract fields like author or date using LLMs [api/apps/services/dataset_api_service.py69-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L69-L85)

Title: Dataset Creation Flow

```
POST_/api/v1/datasets
Authorization:_Bearer_token
Content-Type:_application/json

ragflow_sdk.RAGFlow.create_dataset

dataset_api_service.create_dataset

KnowledgebaseService.create_with_name

verify_embedding_availability

KnowledgebaseService.save()

settings.docStoreConn.create_idx()
Initialize_vector_index
```

**Key Parameters:**

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | Unique dataset name (Max 128 chars) [api/apps/restful_apis/dataset_api.py107-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dataset_api.py#L107-L109) |
| `embedding_model` | string | No | The embedding model ID for the dataset [api/apps/restful_apis/dataset_api.py116-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dataset_api.py#L116-L118) |
| `chunk_method` | enum | No | Parser type (e.g., `naive`, `qa`, `manual`, `laws`) [api/apps/restful_apis/dataset_api.py123-127](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dataset_api.py#L123-L127) |
| `parser_config` | object | No | Method-specific configuration including chunk tokens and delimiters [api/apps/restful_apis/dataset_api.py128-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dataset_api.py#L128-L130) |
| `auto_metadata_config` | object | No | Configuration for LLM-based auto metadata extraction [api/apps/services/dataset_api_service.py68-84](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L68-L84) |

**Sources:** [api/apps/services/dataset_api_service.py57-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L57-L110) [api/apps/restful_apis/dataset_api.py81-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dataset_api.py#L81-L159) [test/testcases/test_http_api/common.py37-39](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_http_api/common.py#L37-L39)

### List and Delete Datasets

**Endpoints:** `GET /api/v1/datasets`, `DELETE /api/v1/datasets`

The deletion process is handled by `delete_datasets` in the service layer, which performs a cascading removal of associated documents from both the database and the document store (Elasticsearch/Infinity/OceanBase) [api/apps/services/dataset_api_service.py113-178](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L113-L178) It also removes physical files from storage via `FileService.filter_delete` [api/apps/services/dataset_api_service.py147-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L147-L152)

**Sources:** [api/apps/services/dataset_api_service.py113-178](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L113-L178) [api/apps/restful_apis/dataset_api.py161-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dataset_api.py#L161-L210) [test/testcases/test_http_api/common.py52-59](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_http_api/common.py#L52-L59)

## Metadata Management

RAGFlow provides comprehensive APIs for managing metadata at both the dataset and document levels. This allows for fine-grained retrieval filtering.

### Metadata Configuration and Operations

The system allows users to define custom metadata fields and utilize built-in fields. Metadata settings can be flattened for easy retrieval or configured specifically for a knowledge base.

**Service Endpoints:**

- Get Flattened Metadata: GET /api/v1/datasets/metadata/flattened api/apps/restful_apis/dataset_api.py59-62
- Aggregate Tags: GET /api/v1/datasets/tags/aggregation api/apps/restful_apis/dataset_api.py37-40

Frontend components use the `useKnowledgeRequest` hook to interact with these endpoints via `kbService` [web/src/hooks/use-knowledge-request.ts50-51](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/hooks/use-knowledge-request.ts#L50-L51)

**Sources:** [api/apps/restful_apis/dataset_api.py37-79](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dataset_api.py#L37-L79) [web/src/hooks/use-knowledge-request.ts43-54](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/hooks/use-knowledge-request.ts#L43-L54) [api/apps/services/dataset_api_service.py68-84](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L68-L84)

## Retrieval Testing Interface

The retrieval test API allows developers to verify the effectiveness of the knowledge base by simulating search queries.

### Retrieval Parameters

The `useTestRetrieval` hook manages the state for testing retrieval in the web UI [web/src/hooks/use-knowledge-request.ts62-126](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/hooks/use-knowledge-request.ts#L62-L126) It supports the following parameters [web/src/pages/dataset/testing/testing-form.tsx58-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/testing/testing-form.tsx#L58-L69):

- `similarity_threshold`: Filtering results below a certain score.
- `vector_similarity_weight`: Balancing keyword vs. vector search.
- `top_k`: Limiting the number of returned chunks.
- `use_kg`: Enabling Knowledge Graph (GraphRAG) retrieval.
- `MetadataFilter`: Applying complex boolean logic and comparison operators.

Title: Retrieval Test Flow

```
Backend_Processing

web/src/pages/dataset/testing/index.tsx

web/src/hooks/use-knowledge-request.ts::useTestRetrieval

web/src/services/knowledge-service.ts::kbService.retrievalTest

POST_/api/v1/retrieval

api/apps/services/dataset_api_service.py

rag/nlp/search.py

api/db/services/document_service.py
```

**Sources:** [web/src/hooks/use-knowledge-request.ts62-126](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/hooks/use-knowledge-request.ts#L62-L126) [web/src/pages/dataset/testing/testing-form.tsx49-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/testing/testing-form.tsx#L49-L146) [web/src/pages/dataset/testing/testing-result.tsx39-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/testing/testing-result.tsx#L39-L112) [test/testcases/test_http_api/common.py33](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_http_api/common.py#L33-L33)

## Implementation Reference

### Key Service Classes and Functions

| Entity | Role | File |
| --- | --- | --- |
| `create_dataset` | Logic for initializing a dataset and its parser configuration | `api/apps/services/dataset_api_service.py` |
| `KnowledgebaseService` | Database abstraction for Knowledgebase table operations | `api/db/services/knowledgebase_service.py` |
| `kbService` | Frontend service layer for knowledge base API calls | `web/src/services/knowledge-service.ts` |
| `CreateDatasetReq` | Request validation schema for dataset creation | `api/utils/validation_utils.py` |
| `delete_datasets` | Cascading deletion of datasets, documents, and indices | `api/apps/services/dataset_api_service.py` |

**Sources:** [api/apps/services/dataset_api_service.py56](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L56-L56) [api/db/services/knowledgebase_service.py28](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L28-L28) [api/apps/restful_apis/dataset_api.py140](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dataset_api.py#L140-L140) [api/apps/services/dataset_api_service.py112](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/dataset_api_service.py#L112-L112)


---



<!-- ===== 7.5-document-and-file-management-apis ===== -->

# Document and File Management APIs

Relevant source files
- api/apps/restful_apis/document_api.py
- api/db/services/common_service.py
- sdk/python/test/test_http_api/test_file_management_within_dataset/test_stop_parse_documents.py
- test/testcases/test_http_api/test_file_management_within_dataset/test_delete_documents.py
- test/testcases/test_http_api/test_file_management_within_dataset/test_doc_sdk_routes_unit.py
- test/testcases/test_http_api/test_file_management_within_dataset/test_list_documents.py
- test/testcases/test_http_api/test_file_management_within_dataset/test_parse_documents.py
- test/testcases/test_http_api/test_file_management_within_dataset/test_stop_parse_documents.py
- test/testcases/test_web_api/conftest.py
- test/testcases/test_web_api/test_chunk_app/conftest.py
- test/testcases/test_web_api/test_common.py
- test/testcases/test_web_api/test_document_app/conftest.py
- test/testcases/test_web_api/test_document_app/test_create_document.py
- test/testcases/test_web_api/test_document_app/test_document_metadata.py
- test/testcases/test_web_api/test_document_app/test_list_documents.py
- test/testcases/test_web_api/test_document_app/test_paser_documents.py
- test/testcases/test_web_api/test_document_app/test_rm_documents.py
- test/testcases/test_web_api/test_document_app/test_upload_documents.py
- web/src/hooks/use-document-request.ts
- web/src/services/knowledge-service.ts
- web/src/utils/api.ts

This page documents the API endpoints and services for managing documents and files within RAGFlow. These APIs handle file uploads, document lifecycle management, metadata operations, and the coordination between the file storage layer and the document processing pipeline.

## Overview

RAGFlow maintains a dual-layer architecture for content management:

1. File Layer: Physical file storage and hierarchical folder organization. Managed by FileService api/db/services/file_service.py38 and file_api.py. It tracks file metadata in the File model and supports operations like folder creation and moving files between directories.
2. Document Layer: Logical document records associated with a knowledge base (dataset). Managed by DocumentService api/db/services/document_service.py36 and document_api.py api/apps/restful_apis/document_api.py122-125 It tracks parsing status, chunk counts, and configurations.

The relationship is maintained via the `File2DocumentService` [api/db/services/file2document_service.py37](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/file2document_service.py#L37-L37) which maps physical files to their logical document counterparts in the RAG system.

## API Architecture

### Document Management Routes

The document management APIs are primarily handled by `document_api.py` (Backend) and `knowledge-service.ts` (Frontend). These routes manage ingestion, renaming, and parsing triggers for documents within a dataset.

**Document API to Code Entity Mapping**

```
Code Entity Space (Python/JS)

Natural Language Space

Service Layer

Backend Routes (api/apps/restful_apis/document_api.py)

Frontend Services (web/src/services/knowledge-service.ts)

Upload Document to Dataset

Upload Info (URL/File)

Update Document (Parser/Name)

Trigger Document Parsing

Update Bulk Metadata

uploadDocument

documentIngest

updateDocumentsMetadata

@manager.route('/documents/upload')

@manager.route('/datasets//documents/')

@manager.route('/documents/ingest')

@manager.route('/datasets//documents/metadatas')

FileService.upload_info

DocumentService

validate_document_update_fields

File2DocumentService
```

Sources: [api/apps/restful_apis/document_api.py59-62](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L59-L62) [api/apps/restful_apis/document_api.py122-125](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L122-L125) [web/src/services/knowledge-service.ts16-22](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L16-L22) [web/src/utils/api.ts149-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L149-L165) [api/apps/restful_apis/document_api.py29-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L29-L31)

### File Type Detection and Processing

The system automatically categorizes files using the `filename_type` utility [api/utils/file_utils.py53](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/file_utils.py#L53-L53) This categorization determines which parser is suggested and how thumbnails are generated.

**File Type Categorization Logic**

| Category | Extensions | Code Entity |
| --- | --- | --- |
| **PDF** | `.pdf` | `FileType.PDF` [api/db/db_models.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L32-L32) |
| **DOC** | `.docx`, `.txt`, `.md`, `.epub`, `.xlsx`, etc. | `FileType.DOC` [api/db/db_models.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L32-L32) |
| **AURAL** | `.mp3`, `.wav`, `.flac`, `.opus`, etc. | `FileType.AURAL` [api/db/db_models.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L32-L32) |
| **VISUAL** | `.jpg`, `.png`, `.webp`, `.mp4`, `.avi`, etc. | `FileType.VISUAL` [api/db/db_models.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L32-L32) |

Sources: [api/utils/file_utils.py53](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/file_utils.py#L53-L53) [api/db/db_models.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L32-L32) [api/db/services/common_service.py74-87](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/common_service.py#L74-L87)

## Document and File Relationship

In RAGFlow, a "File" is a storage entity, while a "Document" is a processing entity.

- File: Created upon upload to the file manager. It tracks physical storage location and hierarchical metadata.
- Document: Created when a file is imported into a knowledge base. It contains parsing configuration (parser_id), chunking status (run), and statistics like chunk_num test/testcases/test_http_api/test_file_management_within_dataset/test_doc_sdk_routes_unit.py73-88
- Mapping: File2DocumentService manages the link between these two entities api/db/services/file2document_service.py37

## Metadata Management

Metadata is handled through dedicated services that interface with both the relational database and the document store.

- DocMetadataService: Manages document-level metadata api/db/services/doc_metadata_service.py34
- Metadata Configuration: Endpoints exist to update metadata configuration at both the dataset and document levels web/src/utils/api.ts135-142
- Thumbnail Generation: The thumbnail function generates base64 strings for PDFs and images api/utils/file_utils.py53
- Data Normalization: Frontend services like mapDocumentToLegacy ensure that newer API fields (e.g., chunk_count) are mapped to legacy UI expectations (e.g., chunk_num) web/src/services/knowledge-service.ts92-97

Sources: [api/apps/restful_apis/document_api.py34](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L34-L34) [web/src/utils/api.ts135-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L135-L142) [web/src/services/knowledge-service.ts92-97](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L92-L97)

## Core API Endpoints

### Document Operations (Dataset Context)

| Endpoint | Method | Description |
| --- | --- | --- |
| `/datasets/<id>/documents` | GET | List documents in a dataset [web/src/utils/api.ts160-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L160-L161) |
| `/datasets/<id>/documents/<doc_id>` | PATCH | Update document name, parser, or status [api/apps/restful_apis/document_api.py122-125](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L122-L125) |
| `/documents/upload` | POST | Upload file/URL to get parsed info without dataset binding [api/apps/restful_apis/document_api.py59-62](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L59-L62) |
| `/datasets/<id>/documents/batch-update-status` | POST | Bulk update document enabled/disabled status [web/src/utils/api.ts162-163](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L162-L163) |
| `/datasets/<id>/documents` | DELETE | Delete documents or clear a dataset [web/src/utils/api.ts164-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L164-L165) |

### Metadata and Retrieval APIs

| Endpoint | Method | Description |
| --- | --- | --- |
| `/datasets/<id>/documents/<doc_id>/chunks` | GET | List chunks for a specific document [web/src/utils/api.ts153-154](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L153-L154) |
| `/datasets/search` | POST | Perform retrieval test across datasets [web/src/utils/api.ts157](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L157-L157) |
| `/datasets/<id>/documents/metadatas` | POST | Update metadata for multiple documents [web/src/utils/api.ts137-138](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L137-L138) |

Sources: [api/apps/restful_apis/document_api.py59-125](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L59-L125) [web/src/utils/api.ts135-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/utils/api.ts#L135-L165) [web/src/services/knowledge-service.ts138-159](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L138-L159)

## Parsing Lifecycle

The document parsing lifecycle is an asynchronous process managed via Redis-backed task queues.

**Parsing Task Lifecycle Diagram**

```
Task Executor (rag/svr/task_executor.py)
TaskService (api/db/services/task_service.py)
MySQL (api/db/db_models.py)
Document API (api/apps/restful_apis/document_api.py)
Frontend (web/)
Task Executor (rag/svr/task_executor.py)
TaskService (api/db/services/task_service.py)
MySQL (api/db/db_models.py)
Document API (api/apps/restful_apis/document_api.py)
Frontend (web/)
loop
[Polling]
POST /documents/ingest
TaskService.add_task(doc_id)
Insert into Task table (status=UNSTART)
Return success
GET /datasets/<id>/documents
Query Document.run status
RunningStatus.RUNNING
Update Task status to DONE
Update Document.run status to DONE
```

1. Ingestion: Triggered via documentIngest web/src/services/knowledge-service.ts37-40
2. Task Creation: The backend creates entries in the Task table to track the asynchronous chunking process api/db/services/task_service.py41
3. Status Tracking: The run field in the Document model tracks states using TaskStatus (UNSTART, RUNNING, CANCEL, DONE, FAIL) api/apps/restful_apis/document_api.py50
4. Reparsing: Triggered by reset_document_for_reparse, which clears existing chunks and resets the task status api/apps/services/document_api_service.py29-31
5. Validation: Update requests are validated using UpdateDocumentReq to ensure parameters like chunk_method and parser_config are correct api/apps/restful_apis/document_api.py189-190

Sources: [web/src/services/knowledge-service.ts37-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/knowledge-service.ts#L37-L40) [api/apps/services/document_api_service.py29-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/document_api_service.py#L29-L31) [api/db/services/task_service.py41](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L41-L41) [api/apps/restful_apis/document_api.py189-190](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/document_api.py#L189-L190)


---



<!-- ===== 7.6-chat-and-conversation-apis ===== -->

# Chat and Conversation APIs

Relevant source files
- api/apps/restful_apis/_generation_params.py
- api/apps/restful_apis/chat_api.py
- api/apps/restful_apis/dify_retrieval_api.py
- api/apps/restful_apis/openai_api.py
- api/apps/restful_apis/search_api.py
- test/testcases/restful_api/test_dify_retrieval_routes_unit.py
- test/testcases/test_http_api/test_chat_assistant_management/conftest.py
- test/testcases/test_http_api/test_chat_assistant_management/test_chat_sdk_routes_unit.py
- test/testcases/test_http_api/test_dataset_management/test_dify_retrieval_routes_unit.py
- test/testcases/test_http_api/test_session_management/test_session_sdk_routes_unit.py
- test/testcases/test_sdk_api/test_session_management/conftest.py
- test/testcases/test_sdk_api/test_session_management/test_delete_sessions_with_chat_assistant.py
- test/testcases/test_web_api/test_search_app/test_search_routes_unit.py
- test/unit_test/api/apps/sdk/test_dify_retrieval.py

This page documents the chat completion and conversation management APIs in RAGFlow, covering both standard RAG-based chat assistants and agent-based conversations. This includes session lifecycle management, the RAG retrieval flow, and OpenAI-compatible endpoints.

## Architecture Overview

RAGFlow provides two primary conversation modes: **Chat Assistants** (RAG-based) and **Agents** (workflow-based). Both use a session-based architecture where conversations are organized into sessions that maintain message history and context.

### Chat System Components

The following diagram bridges the Natural Language interaction space with the core Python classes and database entities that handle them.

```
Data Layer

LLM Integration

Service Layer

API Layer (api/apps/restful_apis/)

openai_api.py
_stream_chat_completion_sse()

chat_api.py
session_completion()

search_api.py
create()

DialogService
(api/db/services/dialog_service.py)

ConversationService
(api/db/services/conversation_service.py)

SearchService
(api/db/services/search_service.py)

TenantModelService
(api/db/joint_services/tenant_model_service.py)

LLMBundle
(api/db/services/llm_service.py)

get_model_config_from_provider_instance()

Dialog Table
(Assistant Config)

Conversation Table
(Session History)

Search App Table
```

**Sources:** [api/apps/restful_apis/openai_api.py24-26](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/openai_api.py#L24-L26) [api/apps/restful_apis/chat_api.py30-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L30-L35) [api/apps/restful_apis/search_api.py22-29](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/search_api.py#L22-L29) [api/db/services/dialog_service.py24](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L24-L24) [api/db/services/conversation_service.py34](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/conversation_service.py#L34-L34)

## Session Management

Sessions group related messages for a specific chat assistant or agent. Each session maintains its own message history and can be created, updated, and retrieved independently.

### Session Interaction Flow

Conversations are represented as `Conversation` records linked to a `Dialog` (Chat Assistant).

```
api/db/services/dialog_service.py
api/db/services/conversation_service.py
api/apps/restful_apis/chat_api.py
Client
api/db/services/dialog_service.py
api/db/services/conversation_service.py
api/apps/restful_apis/chat_api.py
Client
POST /sessions (chat_id, user_id)
_ensure_owned_chat(chat_id)
_create_session_for_completion()
New Session Record
Session ID & Initial Message
```

**Sources:** [api/apps/restful_apis/chat_api.py164-168](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L164-L168) [api/apps/restful_apis/chat_api.py188-193](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L188-L193) [api/db/services/conversation_service.py34](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/conversation_service.py#L34-L34)

### Persistence and History

- Dialog Table: Stores assistant settings like prompt, LLM parameters, and associated knowledge bases. Fields are persisted using DialogService.model._meta.fields api/apps/restful_apis/chat_api.py115
- Conversation Table: Stores the list of messages and references. In the REST API, dialog_id is mapped to chat_id and message is mapped to messages for client compatibility api/apps/restful_apis/chat_api.py157-161
- Read-only Fields: System fields like tenant_id, created_by, and create_time are protected from external updates api/apps/restful_apis/chat_api.py114

## Chat Completion Flow

The chat completion endpoint handles the core RAG logic, combining retrieval and generation.

### OpenAI-Compatible Completion

**Endpoint:** `POST /api/v1/openai/{chat_id}/chat/completions`

This endpoint translates RAGFlow's internal processing into the OpenAI standard.

1. Model Validation: _validate_llm_id ensures the requested model exists for the tenant and matches the required type (chat or image2text) api/apps/restful_apis/openai_api.py34-51
2. Streaming Transformation: _stream_chat_completion_sse translates RAGFlow's internal dialog events (start thinking, answer delta, final answer) into OpenAI-compatible SSE chunks api/apps/restful_apis/openai_api.py96-143
3. Reasoning Content: Support for models with reasoning capabilities is handled via reasoning_content in the delta api/apps/restful_apis/openai_api.py166-172
4. Float Sanitization: _sanitize_json_floats replaces NaN or Infinity floats with None in JSON responses to ensure RFC 8259 compliance for downstream Go consumers api/apps/restful_apis/chat_api.py55-84

**Sources:** [api/apps/restful_apis/openai_api.py34-51](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/openai_api.py#L34-L51) [api/apps/restful_apis/openai_api.py119-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/openai_api.py#L119-L173) [api/apps/restful_apis/chat_api.py55-84](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L55-L84)

## Assistant Configuration

Assistant behavior is governed by `PromptConfig` and `LlmSetting`.

| Configuration Field | Description | Default (RAG) |
| --- | --- | --- |
| `system` | The system prompt template with `{knowledge}` placeholder | "You are an intelligent assistant..." [api/apps/restful_apis/chat_api.py88-96](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L88-L96) |
| `empty_response` | Text returned when no relevant knowledge is found | "Sorry! No relevant content was found..." [api/apps/restful_apis/chat_api.py99](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L99-L99) |
| `quote` | Whether to include citations in the response | `True` [api/apps/restful_apis/chat_api.py100](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L100-L100) |
| `refine_multiturn` | Enable context refinement for multi-turn chat | `True` [api/apps/restful_apis/chat_api.py102](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L102-L102) |

### Default Configurations

RAGFlow provides sensible defaults:

- RAG Chat: Includes knowledge base placeholders and summary instructions api/apps/restful_apis/chat_api.py87-103
- Direct Chat: Minimal configuration for direct LLM interaction without knowledge base retrieval api/apps/restful_apis/chat_api.py104-112

**Sources:** [api/apps/restful_apis/chat_api.py87-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L87-L112)

## Retrieval and Search Integration

The retrieval pipeline is integrated directly into the chat flow or accessible via standalone search APIs.

- KB Resolution: _resolve_kb_names verifies the status and existence of datasets linked to a chat assistant api/apps/restful_apis/chat_api.py127-135
- Metadata Enrichment: _build_reference_chunks uses enrich_chunks_with_document_metadata to add document-level metadata (like specific fields from DocMetadataService) to retrieval chunks api/apps/restful_apis/openai_api.py57-84
- Search Apps: Users can create dedicated search applications via search_api.py. These apps use SearchService to manage search configurations and vector_similarity_weight api/apps/restful_apis/search_api.py42-74
- Dify Compatibility: A dedicated dify_retrieval_api.py provides compatibility with Dify-style retrieval requests, supporting both GET and POST methods api/apps/restful_apis/dify_retrieval_api.py111-114

**Sources:** [api/apps/restful_apis/chat_api.py127-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/chat_api.py#L127-L135) [api/apps/restful_apis/openai_api.py57-84](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/openai_api.py#L57-L84) [api/apps/restful_apis/search_api.py148-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/search_api.py#L148-L162) [api/apps/restful_apis/dify_retrieval_api.py41-95](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/dify_retrieval_api.py#L41-L95)


---



<!-- ===== 7.7-agent-and-memory-apis ===== -->

# Agent and Memory APIs

Relevant source files
- api/apps/restful_apis/agent_api.py
- api/apps/restful_apis/memory_api.py
- api/apps/services/memory_api_service.py
- api/db/joint_services/memory_message_service.py
- api/db/services/canvas_service.py
- api/db/services/conversation_service.py
- api/db/services/langfuse_service.py
- api/db/services/pipeline_operation_log_service.py
- api/db/services/search_service.py
- api/db/services/tenant_llm_service.py
- conf/mapping.json
- memory/services/messages.py
- memory/utils/es_conn.py
- memory/utils/infinity_conn.py
- test/testcases/test_http_api/test_session_management/test_agent_completions.py
- test/testcases/test_http_api/test_session_management/test_agent_sessions.py
- test/testcases/test_http_api/test_session_management/test_chat_completions.py
- test/testcases/test_web_api/test_agent_app/test_agents_webhook_unit.py
- test/unit_test/api/apps/restful_apis/test_get_agent_session.py
- test/unit_test/api/db/services/test_dialog_service_final_answer.py
- test/unit_test/api/db/services/test_dialog_service_use_sql_source_columns.py
- web/src/routes.tsx
- web/src/utils/authorization-util.ts
- web/src/wrappers/auth.tsx

## Overview

This document details the technical implementation and APIs for managing RAGFlow's Agent (Canvas) system and the persistent Memory subsystem. These APIs enable programmatic orchestration of visual workflows, agent lifecycle management, and conversational state retention.

The Agent system is built around a graph-based Domain Specific Language (DSL) interpreted by the `Canvas` engine [api/db/services/canvas_service.py22](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L22-L22) while the Memory system provides multi-tier persistence using both relational databases and vector document stores [api/db/joint_services/memory_message_service.py31-33](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L31-L33)

## Agent and Canvas Management

### HTTP API: Agent CRUD Operations

The Agent management layer provides endpoints for manipulating `UserCanvas` and `UserCanvasVersion` entities. These are primarily defined in `agent_api.py` and supported by `UserCanvasService` [api/apps/restful_apis/agent_api.py38-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/agent_api.py#L38-L50)

**Endpoint Structure:**

- Templates: GET /templates returns pre-defined agent structures via CanvasTemplateService api/apps/restful_apis/agent_api.py38-41
- List Sessions: GET /agents/<agent_id>/sessions retrieves conversation histories associated with a specific agent, using API4ConversationService api/apps/restful_apis/agent_api.py37-38
- Normalization: Sessions are processed via _normalize_agent_session to transform database records into standardized API responses, including chunk reference mapping api/apps/restful_apis/agent_api.py119-178

**Key Logic Flows:**

1. Access Control: Endpoints use decorators like _require_canvas_access_sync to verify that the tenant_id has permissions for the specific agent_id api/apps/restful_apis/agent_api.py74-80
2. Versioning: UserCanvasVersionService maintains a history of agent configurations, allowing for stable execution and rollback capabilities api/db/services/canvas_service.py27
3. Replica System: Execution state is managed via CanvasReplicaService, which commits the DSL state after workflow runs via commit_runtime_replica inside the session runner api/apps/restful_apis/agent_api.py35

**Title: Agent Persistence and Access Control Flow**

```

```

Sources: [api/apps/restful_apis/agent_api.py74-178](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/agent_api.py#L74-L178) [api/db/services/canvas_service.py44-65](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L44-L65) [api/db/services/api_service.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/api_service.py#L25-L25)

### Agent Execution and Session Management

Execution is driven by the `Canvas` class and exposed via completion endpoints. The system supports both native and OpenAI-compatible formats [api/apps/restful_apis/agent_api.py42-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/agent_api.py#L42-L44)

**Agent Sessions:**

- Interaction: The _run_workflow_session function manages the execution of the canvas, handling file attachments, user inputs, and streaming responses api/apps/restful_apis/agent_api.py185-192
- Streaming SSE: Workflow progress is streamed to clients using _build_sse_response which sets appropriate headers for server-sent events api/apps/restful_apis/agent_api.py110-116

**Completion Flow:** When a user interacts with an agent, the system uses `agent_completion` (aliased from `CanvasService.completion`) to process the request [api/apps/restful_apis/agent_api.py42](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/agent_api.py#L42-L42) This involves resolving variables and executing the component graph.

Sources: [api/apps/restful_apis/agent_api.py110-192](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/agent_api.py#L110-L192) [api/db/services/canvas_service.py22](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L22-L22)

## Memory System APIs

The Memory system provides persistent state for agents, supporting raw, semantic, episodic, and procedural memory types [api/db/joint_services/memory_message_service.py22](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L22-L22)

### Memory Configuration and Management

Memory entities are managed through the REST API and backed by `MemoryService` and `MemoryMessageService` [api/db/joint_services/memory_message_service.py27-28](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L27-L28)

**Core Memory Operations:**

- Creation: create_memory validates the name length against MEMORY_NAME_LIMIT and ensures the memory type is supported before calling MemoryService api/apps/services/memory_api_service.py75-111
- Saving: save_to_memory creates a raw message record and multiple extracted records if the memory type is not RAW api/db/joint_services/memory_message_service.py39-92
- ID Generation: Unique IDs for memory messages are generated using Redis auto-increment counters with the "memory" namespace api/db/joint_services/memory_message_service.py64
- Extraction Logic: extract_by_llm uses PromptAssembler to create system prompts based on memory types and processes the dialogue through a LLMBundle api/db/joint_services/memory_message_service.py145-158

**Title: Memory System Entity Mapping**

```

```

Sources: [api/db/joint_services/memory_message_service.py39-92](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L39-L92) [api/apps/services/memory_api_service.py75-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/memory_api_service.py#L75-L111) [memory/services/messages.py32-36](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L32-L36)

### Memory Data Flow and Persistence

Memory data is stored in the document store (Elasticsearch or Infinity) for semantic retrieval and in MySQL for metadata management.

1. Index Management: MessageService manages per-user indices (e.g., memory_{uid}) for storing messages memory/services/messages.py27-47
2. Extraction: For non-raw memory types, extract_by_llm distills key information from dialogues using LLM chat completions api/db/joint_services/memory_message_service.py145-160
3. Mapping: The ESConnection class handles the mapping of message dictionaries to storage fields like content_ltks and vector fields q_{dims}_vec memory/utils/es_conn.py53-78
4. Retrieval: list_message in MessageService retrieves raw messages and their associated extractions (linked by source_id) from the document store memory/services/messages.py71-123

**Title: Message to Memory Persistence Sequence**

```
ESConnection
MessageService
LLMBundle
memory_message_service.py
agent_api.py
User
ESConnection
MessageService
LLMBundle
memory_message_service.py
agent_api.py
User
Interaction (Chat/Workflow)
save_to_memory(memory_id, message_dict)
extract_by_llm (if not RAW)
Extracted Content
embed_and_save
insert(messages, index, memory_id)
Indexing Confirmation
```

Sources: [api/db/joint_services/memory_message_service.py39-92](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L39-L92) [memory/services/messages.py50-56](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L50-L56) [memory/utils/es_conn.py53-78](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L53-L78)


---



<!-- ===== 7.8-system-status-and-health-monitoring ===== -->

# System Status and Health Monitoring

Relevant source files
- admin/server/admin_server.py
- admin/server/auth.py
- admin/server/config.py
- admin/server/routes.py
- admin/server/services.py
- agent/component/list_operations.py
- api/db/joint_services/user_account_service.py
- api/db/services/doc_metadata_service.py
- api/db/services/system_settings_service.py
- api/ragflow_server.py
- api/utils/configs.py
- api/utils/health_utils.py
- conf/system_settings.json
- docker/launch_admin_service.sh
- docs/develop/launch_ragflow_from_source.md
- docs/develop/mcp/launch_mcp_server.md
- docs/guides/dataset/add_data_source/add_rss.md
- docs/guides/dataset/enable_excel2html.md
- internal/admin/service_variables_test.go
- internal/dao/mcp.go
- internal/dao/mcp_test.go
- internal/handler/mcp.go
- internal/handler/mcp_test.go
- internal/service/mcp.go
- internal/service/mcp_test.go
- internal/utility/mcp_client.go
- internal/utility/mcp_client_test.go
- internal/utility/oauth/client_test.go
- internal/utility/ssrf.go
- internal/utility/ssrf_test.go
- run_go_tests.sh
- test/testcases/test_web_api/test_canvas_app/test_list_operations_unit.py
- web/src/pages/agent/canvas/node/list-operations-node.tsx
- web/src/pages/agent/form/list-operations-form/index.tsx

## Purpose and Scope

This document describes RAGFlow's system health monitoring and status reporting infrastructure. It covers the health check endpoints exposed for operational monitoring, component status reporting, task executor heartbeat monitoring, and the Model Context Protocol (MCP) server integration. This system ensures high availability by providing real-time visibility into the backend services, document engines, and task processing layers.

## Health Check Endpoints

RAGFlow exposes multiple health check endpoints to support different monitoring use cases, from simple liveness probes to detailed component status reporting. The system uses a multi-layered approach involving the core Python backend (Quart/Flask), the Go-based administrative layer, and specialized storage engine probes.

### Endpoint Overview

| Endpoint | Authentication | Implementation | Purpose |
| --- | --- | --- | --- |
| `GET /api/v1/admin/ping` | None | `admin/server/routes.py` | Admin connectivity check [admin/server/routes.py38-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/routes.py#L38-L40) |
| `GET /api/v1/admin/auth` | Required | `admin/server/routes.py` | Verify admin session validity [admin/server/routes.py67-73](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/routes.py#L67-L73) |
| `GET /oceanbase/status` | Required | `api/utils/health_utils.py` | OceanBase specific metrics [api/utils/health_utils.py104-133](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/health_utils.py#L104-L133) |
| `GET /api/v1/mcp/list` | Required | `internal/handler/mcp.go` | List and monitor registered MCP servers [internal/handler/mcp.go98-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/handler/mcp.go#L98-L140) |

### Health Monitoring Architecture

The following diagram maps the health monitoring flow from external requests to internal code entities and storage systems.

**System Health Data Flow**

```
Backend Connection Entities

api/utils/health_utils.py

External Monitoring

Kubernetes Liveness/Readiness

Load Balancer

Operations Dashboard

check_db()

check_redis()

check_doc_engine()

check_storage()

get_oceanbase_status()

settings.docStoreConn.health()

settings.STORAGE_IMPL.health()

DB.execute_sql('SELECT 1')

REDIS_CONN.health()

/system/healthz

/api/v1/admin/ping

/oceanbase/status

Return 'pong'
```

**Sources:** [api/utils/health_utils.py34-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/health_utils.py#L34-L69) [admin/server/routes.py38-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/routes.py#L38-L40)

## Component Health Probes

### Healthz Implementation

The comprehensive health check logic resides in `api/utils/health_utils.py`. It uses a series of probes to verify the entire stack:

1. Relational Database: check_db() executes a lightweight SELECT 1 query via the DB model api/utils/health_utils.py34-41
2. Redis Connectivity: check_redis() uses the REDIS_CONN.health() method to verify connectivity api/utils/health_utils.py44-51
3. Document Engine: check_doc_engine() calls the health() method of the configured docStoreConn (Elasticsearch, Infinity, or OceanBase) api/utils/health_utils.py53-61
4. Object Storage: check_storage() verifies the health of the STORAGE_IMPL (MinIO or S3) api/utils/health_utils.py63-69

### Metadata Index Integrity

The system monitors the state of document metadata storage. During user creation or deletion, the system ensures that tenant-specific metadata indices (e.g., `ragflow_doc_meta_<tenant_id>`) are properly managed [api/db/services/doc_metadata_service.py70-80](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/doc_metadata_service.py#L70-L80) Failure to initialize or clean up these indices results in logged exceptions during the user account lifecycle [api/db/joint_services/user_account_service.py109-113](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/user_account_service.py#L109-L113)

**Sources:** [api/utils/health_utils.py34-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/health_utils.py#L34-L69) [api/db/services/doc_metadata_service.py70-80](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/doc_metadata_service.py#L70-L80) [api/db/joint_services/user_account_service.py106-113](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/user_account_service.py#L106-L113)

## Task Executor Heartbeat and Progress

RAGFlow monitors the background task processing layer through a distributed lock and progress update mechanism within the main server.

### Progress Update Thread

The `ragflow_server.py` initializes a dedicated thread `update_progress` that runs periodically [api/ragflow_server.py53-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L53-L69)

- It uses RedisDistributedLock named update_progress to ensure only one server instance updates progress at a time api/ragflow_server.py55-59
- It calls DocumentService.update_progress() to synchronize document parsing states and handle heartbeats api/ragflow_server.py60
- The thread starts with a 1-second delay after server initialization to ensure the DB and Redis are ready api/ragflow_server.py139-143

**Sources:** [api/ragflow_server.py53-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L53-L69) [api/ragflow_server.py139-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L139-L143)

## MCP Server Integration and Monitoring

The Model Context Protocol (MCP) server integration allows RAGFlow to connect to external tool providers. This integration is managed across the Python backend and the Go server layer.

### MCP Lifecycle and Session Management

The RAGFlow server manages the lifecycle of MCP sessions. Upon receiving interrupt signals (`SIGINT`, `SIGTERM`), the server triggers `shutdown_all_mcp_sessions()` to cleanly close connections to MCP servers [api/ragflow_server.py71-76](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L71-L76)

### Go-based MCP Management

The Go server provides robust handling for MCP server registration and health testing:

- Registration: CreateMCPServer in internal/service/mcp.go validates server types (e.g., sse, streamable-http) and URLs before persisting to the database via MCPServerDAO internal/service/mcp.go102-157
- Transport Modes: Supports legacy SSE transport and modern streamable-HTTP transport docs/develop/mcp/launch_mcp_server.md63-66
- Error Mapping: mcpErrorResponse in internal/handler/mcp.go maps internal Go errors (like ErrMCPInvalidURL or ErrMCPTestFailed) to standardized API responses for the frontend internal/handler/mcp.go193-207

**MCP Integration Mapping**

```
External MCP Servers

Python: api/ragflow_server.py

calls

terminates

Go: internal/service/mcp.go

persists via

MCPService.CreateMCPServer()

MCPServerDAO.CreateMCPServer()

signal_handler()

app.run()

MCP Session Context

shutdown_all_mcp_sessions()
```

**Sources:** [api/ragflow_server.py44](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L44-L44) [api/ragflow_server.py71-76](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/ragflow_server.py#L71-L76) [internal/service/mcp.go102-157](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/mcp.go#L102-L157) [internal/handler/mcp.go193-207](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/handler/mcp.go#L193-L207) [docs/develop/mcp/launch_mcp_server.md63-66](https://github.com/infiniflow/ragflow/blob/d32e05d5/docs/develop/mcp/launch_mcp_server.md?plain=1#L63-L66)

## Operational Database Monitoring: OceanBase

For high-scale deployments using OceanBase as the document engine, RAGFlow provides deep visibility into performance metrics through specialized health functions.

### OceanBase Status Reporting

The `get_oceanbase_status()` and `check_oceanbase_health()` functions collect comprehensive telemetry [api/utils/health_utils.py104-200](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/health_utils.py#L104-L200):

- Connection Health: Tracks if the OBConnection is "healthy" or "timeout" api/utils/health_utils.py120
- Performance Metrics: Aggregates query latency (ms), QPS (Queries Per Second), and slow query counts api/utils/health_utils.py175-179
- Resource Usage: Monitors storage used vs. total capacity api/utils/health_utils.py176-177
- Connection Pool: Reports active vs. maximum allowed connections api/utils/health_utils.py180-181

**Sources:** [api/utils/health_utils.py104-200](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/utils/health_utils.py#L104-L200)

## Admin Service Monitoring

The Admin Service (running on port 9381 by default) provides a dedicated management layer [admin/server/admin_server.py73](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/admin_server.py#L73-L73)

### Service Classification

The admin layer provides endpoints for managing users and monitoring system-wide configurations. It is initialized via `admin_server.py`, which sets up a Flask application and registers the `admin_bp` blueprint [admin/server/admin_server.py52-53](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/admin_server.py#L52-L53)

### Admin Authentication Monitoring

The Admin service uses `flask_login` and a custom `setup_auth` to monitor and validate administrative access [admin/server/auth.py39-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/auth.py#L39-L85) It verifies JWT tokens against the `UserService` and checks the `is_superuser` status via the `check_admin_auth` decorator to ensure only authorized operators can access system status routes [admin/server/auth.py144-157](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/auth.py#L144-L157)

**Sources:** [admin/server/admin_server.py41-84](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/admin_server.py#L41-L84) [admin/server/auth.py39-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/auth.py#L39-L85) [admin/server/auth.py144-157](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/auth.py#L144-L157) [admin/server/routes.py35](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/server/routes.py#L35-L35)


---



<!-- ===== 8-agent-and-workflow-system ===== -->

# Agent and Workflow System

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/retrieval.py
- common/mcp_tool_call_conn.py
- rag/prompts/generator.py
- web/src/constants/agent.tsx
- web/src/pages/agent/canvas/index.tsx
- web/src/pages/agent/canvas/node/dropdown/accordion-operators.tsx
- web/src/pages/agent/canvas/node/dropdown/operator-item-list.tsx
- web/src/pages/agent/canvas/node/extractor-node.tsx
- web/src/pages/agent/constant/index.tsx
- web/src/pages/agent/context.ts
- web/src/pages/agent/form-sheet/form-config-map.tsx
- web/src/pages/agent/hooks/use-add-node.ts
- web/src/pages/agent/hooks/use-cache-chat-log.ts
- web/src/pages/agent/hooks/use-show-drawer.tsx
- web/src/pages/agent/log-sheet/index.tsx
- web/src/pages/agent/log-sheet/tool-timeline-item.tsx
- web/src/pages/agent/log-sheet/workflow-timeline.tsx
- web/src/pages/agent/operator-icon.tsx

The Agent and Workflow System provides a visual programming environment for creating AI-powered workflows and autonomous agents. It implements a graph-based execution engine that orchestrates component pipelines, manages conversational state, and enables multi-step reasoning with tool use. This system bridges visual workflow design with backend execution through a Domain-Specific Language (DSL).

For information about LLM integration and model management, see [LLM Integration System](/infiniflow/ragflow/5-llm-integration-system). For details about the REST API layer, see [Backend API System](/infiniflow/ragflow/7-backend-api-system).

## Canvas DSL and Graph Model

The workflow system is built on a JSON-based DSL that represents workflows as directed graphs with support for loops and branching. Each workflow is called a "canvas" and is stored as a serialized DSL document.

### DSL Structure

The DSL consists of components, history, and global state. The `Graph` class in `agent/canvas.py` is the primary entity responsible for parsing this structure [agent/canvas.py43-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L82)

```
{
  "components": {
    "begin": {
      "obj": {
        "component_name": "Begin",
        "params": {}
      },
      "downstream": ["retrieval_0"],
      "upstream": []
    }
  },
  "history": [],
  "path": ["begin"],
  "retrieval": {"chunks": [], "doc_aggs": []},
  "globals": {
    "sys.query": "",
    "sys.user_id": "tenant_id",
    "sys.conversation_turns": 0,
    "sys.files": []
  }
}
```

**Components Section**: Each component contains:

- obj: Component configuration with component_name and params agent/canvas.py49-51
- downstream: Array of component IDs to execute next agent/canvas.py52
- upstream: Array of component IDs that precede this component agent/canvas.py53

Sources: [agent/canvas.py43-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L82)

### Graph Class Architecture

The `Graph` class [agent/canvas.py43](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L43) provides the foundation for DSL-based execution. It parses the DSL using `normalize_chunker_dsl` [agent/canvas.py89](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L89-L89) instantiates component objects via `component_class` [agent/canvas.py101-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L101-L109) and manages the execution path.

Graph-to-Code Mapping:

```
Execution_Entities [agent/component/]

Graph_Engine [agent/canvas.py]

Frontend_Mapping [web/src/pages/agent/canvas/index.tsx]

nodeTypes: Record

ReactFlowInstance

Graph Class

dsl: dict
components: dict
path: list

load()
Instantiates components

reset()
Clears REDIS logs/cancel

get_variable_value()
set_variable_value()

component_class()
Dynamic lookup

ComponentParam()
Parameter validation

ComponentBase instance
```

Sources: [agent/canvas.py43-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L109) [web/src/pages/agent/canvas/index.tsx79-107](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/canvas/index.tsx#L79-L107)

## Component System Architecture

Components are the building blocks of workflows. The system uses a factory pattern for dynamic component registration and parameter validation.

### Component Base Classes

Component Entity Space:

```
Concrete_Implementation [agent/component/]

Base_Definitions [agent/component/base.py]

ComponentParamBase

ComponentBase

update(conf)
Recursive config update

check()
Validation logic

LLM [llm.py]

Agent [agent_with_tools.py]

Message [message.py]

Categorize [categorize.py]

Retrieval [tools/retrieval.py]
```

**ComponentParamBase** [agent/component/base.py40-51](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L51):

- Defines parameters with validation.
- Implements recursive configuration updates update() agent/component/base.py127-187

**ComponentBase** [agent/component/base.py:28] (referenced in `agent/canvas.py:30`):

- Provides lifecycle methods (_invoke, _invoke_async).
- Implements variable resolution and I/O management via set_output() and get_input().

Sources: [agent/component/base.py40-187](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L187) [agent/canvas.py29-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L29-L30) [agent/component/llm.py83](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L83-L83)

## State and Variable Management

The Canvas maintains multiple levels of state and provides a variable resolution system.

### Variable System

**Variable Types**:

- sys.*: System variables (query, user_id, files, history) web/src/constants/agent.tsx30-37
- env.*: User-defined workflow variables.
- component_id@output_key: Reference to specific component outputs.

The regex pattern for variable detection [agent/canvas.py169](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L169-L169):

```
pat = re.compile(r"\{* *\{([a-zA-Z:0-9]+@[A-Za-z0-9_.-]+|sys\.[A-Za-z0-9_.]+|env\.[A-Za-z0-9_.]+)\} *\}*")
```

### Variable Resolution Flow

**Variable Resolution** [agent/canvas.py195-204](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L195-L204):

- If the expression is sys.* or env.*, it looks up the value in the globals dictionary.
- If it contains @, it splits the string into cpn_id and var_nm agent/canvas.py199
- It then retrieves the component using get_component(cpn_id) agent/canvas.py200 and accesses its output.

Sources: [agent/canvas.py166-204](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L166-L204) [web/src/constants/agent.tsx30-37](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/constants/agent.tsx#L30-L37)

## Agent Components and ReAct Loop

The Agent component implements the ReAct (Reasoning + Acting) pattern for autonomous multi-step task execution with tool use.

### Agent Implementation

**Agent Component** [agent/component/agent_with_tools.py72-74](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L74):

- Inherits from LLM and ToolBase.
- Loads tool components using _load_tool_obj() agent/component/agent_with_tools.py134-146
- Binds tools to the LLMBundle for model-driven tool calling agent/component/agent_with_tools.py114
- Supports MCP (Model Context Protocol) tool integration via MCPServerService agent/component/agent_with_tools.py102-110

**Tool Calling Infrastructure**:

- LLMToolPluginCallSession [agent/tools/base.py:51] (referenced in agent/component/agent_with_tools.py:112): Manages the execution session for tool plugins.
- MCPToolCallSession agent/component/agent_with_tools.py105: Handles connections to external MCP servers.

Sources: [agent/component/agent_with_tools.py72-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L146) [agent/tools/base.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/base.py#L29-L29)

## Workflow Execution and Streaming

The execution engine processes components while supporting dynamic branching and streaming responses.

- Message Streaming: The Message component supports asynchronous generation using _stream() agent/component/message.py194 yielding content chunks as variables are resolved.
- Prompt Generation: The system formats retrieved knowledge using kb_prompt rag/prompts/generator.py128 to inject context into LLM prompts, ensuring they fit within token limits via message_fit_in rag/prompts/generator.py69
- Dynamic Branching: The Categorize component agent/component/categorize.py99 uses LLM classification to determine the next execution path via the _next output agent/component/categorize.py159
- Task Cancellation: Execution state is monitored for cancellation signals via has_canceled agent/canvas.py34 which are checked during operations like message streaming agent/component/message.py200
- Retrieval Logic: The Retrieval tool handles dataset searching, including metadata filtering and cross-language support agent/tools/retrieval.py86-135

Sources: [agent/component/message.py194-228](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L194-L228) [rag/prompts/generator.py69-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L69-L162) [agent/component/categorize.py99-160](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L99-L160) [agent/canvas.py34-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L34-L35) [agent/tools/retrieval.py86-135](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L86-L135)

## Child Pages

- Canvas Engine and DSL — Document the Canvas class, DSL structure (v1 and v2 schemas), graph-based execution model, and how workflows are stored and loaded. Covers both the Python canvas (agent/canvas.py) and the Go port.
- Component System Architecture — Explain the component base classes, parameter validation, the component factory pattern, and dynamic registration. Covers both Python (agent/component/base.py) and Go registries.
- Built-in Components — Document the standard component library including LLM, Retrieval, Tool, Categorize, Generate, Message, Switch, Iteration, and Agent components.
- Workflow Execution and Streaming — Explain the path-based execution algorithm, streaming support, SSE event generation during workflow runs, and the Go workflowx primitives.
- State and Variable Management — Document the variable resolution system (sys., env., component@output), global state management, and context handling.
- Agent Tools and ReAct Loop — Explain the Agent component's ReAct pattern, tool calling mechanism, MCP integration, multi-step reasoning, and the plugin system for custom LLM tools.
- Canvas API and Management — Detail the canvas CRUD endpoints, version management, replica system, conversation session handling, and agent templates.


---



<!-- ===== 8.1-canvas-engine-and-dsl ===== -->

# Canvas Engine and DSL

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/retrieval.py
- common/mcp_tool_call_conn.py
- rag/flow/chunker/title_chunker/common.py
- rag/prompts/generator.py
- web/src/pages/agent/canvas/edge/index.tsx
- web/src/pages/agent/canvas/node/handle.tsx
- web/src/pages/agent/constant/pipeline.tsx
- web/src/pages/agent/form/parser-form/common-form-fields.tsx
- web/src/pages/agent/form/parser-form/index.tsx
- web/src/pages/agent/form/parser-form/pdf-form-fields.tsx
- web/src/pages/agent/form/parser-form/spreadsheet-form-fields.tsx
- web/src/pages/agent/form/parser-form/text-html-form-fields.tsx
- web/src/pages/agent/form/parser-form/word-form-fields.tsx
- web/src/pages/agent/form/title-chunker-form/hook.ts
- web/src/pages/agent/form/title-chunker-form/index.tsx
- web/src/pages/agent/store.ts
- web/src/pages/agent/utils.ts

The Canvas Engine is RAGFlow's workflow execution system that orchestrates AI agent interactions through a graph-based Domain-Specific Language (DSL). This document covers the `Graph` and `Canvas` class implementations, DSL structure (v1 and v2), variable resolution, and workflow persistence mechanisms across the Python and Go implementations.

## DSL Structure

The Canvas DSL is a JSON-based schema that defines the complete state and connectivity of a workflow. Every canvas is represented as a single DSL object that can be serialized and stored.

### Core DSL Schema

The DSL follows this structure as defined in [agent/canvas.py43-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L82):

```
{
  "components": {
    "begin": {
      "obj": {
        "component_name": "Begin",
        "params": {}
      },
      "downstream": ["retrieval_0"],
      "upstream": []
    },
    "retrieval_0": {
      "obj": {
        "component_name": "Retrieval",
        "params": {}
      },
      "downstream": ["generate_0"],
      "upstream": ["begin"]
    }
  },
  "history": [],
  "path": ["begin"],
  "retrieval": {"chunks": [], "doc_aggs": []},
  "globals": {
    "sys.query": "",
    "sys.user_id": "tenant_id",
    "sys.conversation_turns": 0,
    "sys.files": []
  }
}
```

**DSL Fields:**

| Field | Type | Purpose |
| --- | --- | --- |
| `components` | Object | Map of component IDs to their definition and graph connectivity [agent/canvas.py97](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L97-L97) |
| `history` | Array | Conversation history stored as role/content pairs [agent/canvas.py72](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L72-L72) |
| `path` | Array | Current execution path tracking component IDs [agent/canvas.py111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L111-L111) |
| `retrieval` | Object | Stores retrieved chunks and document aggregations [agent/canvas.py74](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L74-L74) |
| `globals` | Object | System-level variables accessible across all components [agent/canvas.py75-80](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L75-L80) |

**Component Definition:** Each entry in the `components` object contains:

- obj: Component instance data with component_name and params agent/canvas.py48-51
- downstream: List of IDs for components that follow this one agent/canvas.py52
- upstream: List of IDs for components that precede this one agent/canvas.py53

The engine supports legacy DSL versions through the `normalize_chunker_dsl` function, which migrates older schemas to the current version upon loading [agent/canvas.py31](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L31-L31) [agent/canvas.py89](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L89-L89)

Sources: [agent/canvas.py43-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L111) [web/src/pages/agent/utils.ts62-86](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/utils.ts#L62-L86) [agent/canvas.py31-89](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L31-L89)

## Canvas and Graph Class Hierarchy

### Class Diagram: Natural Language Space to Code Entity Space

This diagram maps conceptual workflow entities to the specific Python classes and methods that implement them.

```
"manages"

"configures"

«File: agent/canvas.py»

Graph

+dict components

+list path

+dict dsl

+load()

+get_variable_value(exp)

+get_component_obj(cpn_id)

«File: agent/canvas.py»

Canvas

+dict globals

+list history

+run(**kwargs)

+get_history(window_size)

+add_user_input(question)

«File: agent/component/base.py»

ComponentBase

+Canvas _canvas

+ComponentParamBase _param

+invoke(**kwargs)

+set_output(key, value)

«File: agent/component/base.py»

ComponentParamBase

+dict inputs

+dict outputs

+check()

+update(conf)
```

Sources: [agent/canvas.py43-94](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L94) [agent/canvas.py283-300](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L283-L300) [agent/component/base.py40-58](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L58) [agent/component/base.py189-205](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L189-L205)

### Graph Class Implementation

The `Graph` class [agent/canvas.py43-281](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L281) provides the base graph infrastructure:

**Key Responsibilities:**

- DSL Loading: The load() method agent/canvas.py96-111 instantiates component objects. It uses component_class(name + "Param") to create parameter objects and component_class(name) for the component itself agent/canvas.py101-109
- Variable Resolution: Resolves expressions like {sys.query} or {component_id@output} using regex patterns agent/canvas.py168-193
- Task Management: Tracks execution state and handles cancellation via task_id agent/canvas.py91 It monitors cancellation via has_canceled(self.task_id) agent/canvas.py34

### Canvas Class Implementation

The `Canvas` class extends `Graph` to handle conversation-specific logic:

**Conversation Context:**

- History Management: get_history(window_size) retrieves the last $N$ turns of conversation agent/canvas.py330-341
- Global Variables: Manages sys.* variables including sys.query, sys.user_id, and sys.files agent/canvas.py286-293
- Reference Tracking: add_reference() stores retrieved chunks for citation generation agent/canvas.py756-764 It uses chunks_format from rag.prompts.generator to normalize chunk data agent/canvas.py39-40

Sources: [agent/canvas.py43-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L111) [agent/canvas.py168-193](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L193) [agent/canvas.py283-341](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L283-L341) [agent/canvas.py756-764](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L756-L764) [rag/prompts/generator.py41-66](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L41-L66)

## Execution Model

The Canvas Engine uses a path-based execution model where the workflow progresses through a directed graph of components.

### Component Instantiation and Validation

During `load()`, the engine performs strict validation of component parameters:

1. It fetches the parameter class using the component_class factory agent/canvas.py101
2. It updates the parameter object with values from the DSL using update() agent/component/base.py127-187
3. It calls param.check() agent/canvas.py105 For example, the LLMParam check ensures parameters like temperature and max_tokens are valid agent/component/llm.py52-60

### Dynamic Routing and Control Flow

Execution is not strictly linear. Certain components can dynamically alter the path:

| Component | Logic Entity | Behavior |
| --- | --- | --- |
| `Categorize` | `Categorize` [agent/component/categorize.py98](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L98-L98) | Analyzes input and sets the `_next` output to a specific downstream branch based on LLM classification [agent/component/categorize.py159](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L159-L159) |
| `Agent` | `Agent` [agent/component/agent_with_tools.py72](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L72) | Implements a ReAct loop, calling tools via `LLMToolPluginCallSession` [agent/component/agent_with_tools.py112](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L112-L112) and `MCPToolCallSession` [agent/component/agent_with_tools.py105](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L105-L105) until a final answer is reached. |
| `LLM` | `LLM` [agent/component/llm.py82](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L82-L82) | Standard chat component that generates responses based on system and user prompts [agent/component/llm.py119-128](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L119-L128) |
| `Message` | `Message` [agent/component/message.py69](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L69-L69) | Handles final response generation, supporting streaming output and variable interpolation [agent/component/message.py194-224](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L194-L224) |

Sources: [agent/canvas.py96-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L96-L111) [agent/component/llm.py52-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L52-L60) [agent/component/agent_with_tools.py72-115](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L115) [agent/component/message.py194-224](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L194-L224) [agent/component/categorize.py159](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L159-L159)

## Variable and Data Flow

RAGFlow uses a string-based interpolation system to pass data between components.

### Variable Resolution Logic

The `get_variable_value` method [agent/canvas.py195-237](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L195-L237) resolves three types of variables:

1. System Variables: Prefixed with sys. (e.g., sys.query).
2. Environment Variables: Prefixed with env..
3. Component Outputs: Using the @ syntax (e.g., retrieval_0@content).

```
# Internal resolution of component outputs [agent/canvas.py:199-202]
cpn_id, var_nm = exp.split("@")
cpn = self.get_component(cpn_id)
# ...
root_val = cpn["obj"].output(root_key)
```

### Data Flow Diagram: Variable Interpolation

This diagram shows how data flows from the system into components and between components via the Canvas Engine.

```
Components

Canvas Engine (agent/canvas.py)

System State

output('chunks')

{retrieval_0@chunks}

output('answer')

{llm_0@answer}

all_content

sys.query

sys.history

get_variable_value()

Execution Path

Retrieval Component

LLM Component

Message Component
```

Sources: [agent/canvas.py168-237](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L237) [agent/component/base.py207-220](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L207-L220) [agent/component/message.py194-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L194-L210)

## Frontend Integration and State Management

The frontend uses `@xyflow/react` (React Flow) to visualize and manipulate the DSL.

### State Management with Zustand

The frontend state is managed via `useGraphStore` [web/src/pages/agent/store.ts123](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/store.ts#L123-L123) Key actions include:

- onConnect: Creates edges and updates component upstream/downstream logic web/src/pages/agent/store.ts159-166
- updateNodeForm: Persists configuration changes from the UI into the local node data web/src/pages/agent/store.ts71-75
- duplicateNode: Clones a component including its configuration web/src/pages/agent/store.ts90

### DSL Generation from UI

When saving a canvas, the frontend traverses the React Flow graph to rebuild the DSL:

- buildComponentDownstreamOrUpstream: Computes connectivity web/src/pages/agent/utils.ts57-87
- buildAgentTools: Recursively bundles sub-agents and tools into the Agent component's parameters web/src/pages/agent/utils.ts105-135
- buildCategorize: Maps classification branches to specific category descriptions web/src/pages/agent/utils.ts141-164

Sources: [web/src/pages/agent/store.ts123-166](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/store.ts#L123-L166) [web/src/pages/agent/utils.ts57-164](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/utils.ts#L57-L164)

## Persistence and Serialization

Workflows are stored as DSL strings in the database and reconstructed at runtime.

### Serialization and Persistence Service

The `Graph.__str__` method [agent/canvas.py113-132](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L113-L132) handles the serialization of the graph back into a DSL JSON string. It iterates through all loaded components and serializes their state using `json.loads(str(cpn["obj"]))` [agent/canvas.py129](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L129-L129)

### Workflow Storage Entities

| Entity | Purpose |
| --- | --- |
| `UserCanvas` | Database model storing the `dsl` string and metadata. |
| `UserCanvasVersion` | Stores versioned snapshots of the canvas DSL. |
| `CanvasTemplate` | Pre-defined DSL structures for bootstrapping agents. |

Sources: [agent/canvas.py113-132](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L113-L132) [web/src/pages/agent/store.ts173-178](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/store.ts#L173-L178)


---



<!-- ===== 8.2-component-system-architecture ===== -->

# Component System Architecture

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/data_operations.py
- agent/component/docs_generator.py
- agent/component/exit_loop.py
- agent/component/invoke.py
- agent/component/iteration.py
- agent/component/llm.py
- agent/component/loop.py
- agent/component/loopitem.py
- agent/component/message.py
- agent/component/variable_assigner.py
- agent/tools/base.py
- agent/tools/retrieval.py
- common/mcp_tool_call_conn.py
- rag/prompts/generator.py
- test/testcases/test_web_api/test_canvas_app/test_invoke_component_unit.py
- web/src/pages/agent/form/doc-generator-form/index.tsx
- web/src/pages/agent/form/doc-generator-form/use-values.ts

## Purpose and Scope

This document describes the component system architecture that forms the foundation of RAGFlow's Agent and Workflow system. Components are the building blocks of Canvas workflows—self-contained, configurable units that perform specific tasks such as LLM invocation, retrieval, categorization, and string transformation. This page covers the base classes, parameter validation, the component factory pattern, and dynamic registration.

For information about how components are orchestrated in workflows, see [Canvas Engine and DSL](https://github.com/infiniflow/ragflow/blob/d32e05d5/Canvas Engine and DSL) For details on specific built-in components, see [Built-in Components](https://github.com/infiniflow/ragflow/blob/d32e05d5/Built-in Components)

## Component Class Hierarchy

The component system is built on two foundational abstract classes defined in `agent/component/base.py` that all components inherit from: `ComponentParamBase` for configuration and `ComponentBase` for execution logic.

### Class Hierarchy Diagram

```
Execution Entities

Parameter Entities

Base Classes

uses

uses

uses

uses

uses

uses

ABC (Python Abstract Base Class)

ComponentParamBase
[agent/component/base.py:40]

ComponentBase
[agent/component/base.py:365]

LLMParam
[agent/component/llm.py:34]

AgentParam
[agent/component/agent_with_tools.py:38]

ToolParamBase
[agent/tools/base.py:97]

CategorizeParam
[agent/component/categorize.py:30]

MessageParam
[agent/component/message.py:44]

RetrievalParam
[agent/tools/retrieval.py:37]

DataOperationsParam
[agent/component/data_operations.py:22]

LLM
[agent/component/llm.py:83]

Agent
[agent/component/agent_with_tools.py:72]

ToolBase
[agent/tools/base.py:141]

Categorize
[agent/component/categorize.py:99]

Message
[agent/component/message.py:69]

Retrieval
[agent/tools/retrieval.py:86]

DataOperations
[agent/component/data_operations.py:47]
```

**Sources:** [agent/component/base.py40-365](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L365) [agent/component/llm.py34-83](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L34-L83) [agent/component/agent_with_tools.py38-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L38-L72) [agent/component/categorize.py30-99](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L30-L99) [agent/tools/base.py97-141](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/base.py#L97-L141) [agent/component/message.py44-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L44-L69) [agent/tools/retrieval.py37-86](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L37-L86) [agent/component/data_operations.py22-47](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/data_operations.py#L22-L47)

## ComponentParamBase: Parameter Validation and Configuration

`ComponentParamBase` defines the parameter schema and validation rules. Every component has an associated parameter class that handles the merging of runtime configuration with defaults.

### Core Attributes

| Attribute | Description | Defined In |
| --- | --- | --- |
| `inputs` | Dictionary defining expected input parameters and their types. | [agent/component/base.py43](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L43-L43) |
| `outputs` | Dictionary storing the output values and types after execution. | [agent/component/base.py44](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L44-L44) |
| `max_retries` | Number of times to retry on failure (default: 0). | [agent/component/base.py46](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L46-L46) |
| `delay_after_error` | Seconds to wait between retries (default: 2.0). | [agent/component/base.py47](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L47-L47) |

### Parameter Update and Validation

The `update(conf)` method in `agent/component/base.py` performs a recursive update of the parameter object from a configuration dictionary [agent/component/base.py127-187](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L127-L187) It tracks which parameters were "user-fed" versus defaults and handles deprecated parameters [agent/component/base.py160-167](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L160-L167)

Each parameter class must implement a `check()` method to enforce constraints. For example:

- LLMParam.check() validates that temperature, presence_penalty, and max_tokens are valid numeric values and llm_id is not empty agent/component/llm.py53-61
- CategorizeParam.check() ensures category_description is provided and that "To" targets are defined for each category agent/component/categorize.py42-50
- RetrievalParam.check() ensures similarity thresholds and top_n are valid agent/tools/retrieval.py73-77
- MessageParam.check() ensures content is not empty and validates boolean flags agent/component/message.py63-66
- DataOperationsParam.check() ensures the selected operation is within the supported list (e.g., select_keys, literal_eval, combine) agent/component/data_operations.py42-43

**Sources:** [agent/component/base.py40-187](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L187) [agent/component/llm.py53-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L53-L61) [agent/component/categorize.py42-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L42-L50) [agent/tools/retrieval.py73-77](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L73-L77) [agent/component/message.py63-66](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L63-L66) [agent/component/data_operations.py42-43](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/data_operations.py#L42-L43)

## Component Factory Pattern and Dynamic Registration

RAGFlow uses a dynamic discovery mechanism to register and load components at runtime. This avoids hard-coding imports and allows the system to be extended easily by adding new files to the `agent/component/` or `agent/tools/` directory.

### Dynamic Discovery Process

The `agent/component/__init__.py` file automatically scans directories for component modules and extracts classes.

1. `component_class(class_name)`: A factory function that searches for a class name across multiple modules: agent.component, agent.tools, and rag.flow. This is used throughout the codebase to dynamically fetch component classes by their string names agent/canvas.py29 agent/component/agent_with_tools.py135-138

### DSL Loading and Component Instantiation

The `Graph.load()` method in `agent/canvas.py` uses this factory to instantiate components from the DSL:

```
"Component Class"
"Param Class"
"component_class [agent/component/__init__.py]"
"Graph [agent/canvas.py]"
"Component Class"
"Param Class"
"component_class [agent/component/__init__.py]"
"Graph [agent/canvas.py]"
"component_class('LLMParam')"
"LLMParam Class"
"Instantiate & update(params)"
"check() validation"
"component_class('LLM')"
"LLM Class"
"Instantiate(graph, id, param)"
"Component Object"
```

**Sources:** [agent/canvas.py96-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L96-L109) [agent/component/agent_with_tools.py135-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L135-L146)

## Visual Discovery to Code Entity Map

This diagram bridges the gap between the high-level DSL concepts and the concrete Python classes that implement them.

```
Concrete Implementations

Python Entity Space

DSL Structure [agent/canvas.py]

contains

name resolved by

returns

updates

implemented by

implemented by

implemented by

implemented by

implemented by

components[id]

obj

params

component_class()
[agent/component/init.py]

ComponentBase
[agent/component/base.py:365]

ComponentParamBase
[agent/component/base.py:40]

LLM [agent/component/llm.py]

Message [agent/component/message.py]

Agent [agent/component/agent_with_tools.py]

Retrieval [agent/tools/retrieval.py]

Invoke [agent/component/invoke.py]
```

**Sources:** [agent/canvas.py43-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L109) [agent/component/base.py40-365](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L365) [agent/component/llm.py83](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L83-L83) [agent/component/message.py69](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L69-L69) [agent/component/agent_with_tools.py72](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L72) [agent/tools/retrieval.py86](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L86-L86) [agent/component/invoke.py55](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/invoke.py#L55-L55)

## ComponentBase: Execution and Data Flow

`ComponentBase` is the abstract base class for all executable components. It provides the framework for synchronous and asynchronous execution, variable resolution, and state management.

### Execution Framework

Components implement logic in `_invoke()` (sync) or `_invoke_async()` (async). The base class provides wrappers that handle timing, error logging, and output storage:

- `invoke(**kwargs)`: Wraps synchronous execution, handling cancellation checks and timing agent/component/base.py407-419
- `invoke_async(**kwargs)`: Wraps asynchronous execution, using thread_pool_exec for non-coroutine functions to prevent blocking the event loop agent/component/base.py421-447

### Variable Resolution and Data Flow

Components interact with the global state through the `Graph` (Canvas) object. The `get_value_with_variable(value)` method in `agent/canvas.py` resolves variable references within strings using regex patterns like `{{sys.query}}` or `{{component_id@variable}}` [agent/canvas.py168-193](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L193)

Variables are resolved using `Graph.get_variable_value(exp)` [agent/canvas.py195-240](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L195-L240) which supports:

- System Variables: sys.query, sys.user_id, etc. agent/canvas.py198
- Environment Variables: env.VAR_NAME agent/canvas.py169
- Component Outputs: component_id@variable_name agent/canvas.py199
- Nested Paths: Accessing deep fields via dot notation agent/canvas.py209-236

### Data Flow Diagram

```
Downstream Component (llm_0)

Graph (Canvas)

Upstream Component (retrieval_0)

outputs['chunks']
[agent/component/base.py:44]

get_variable_value('retrieval_0@chunks')
[agent/canvas.py:195]

_param.prompts
'{retrieval_0@chunks}'

get_value_with_variable()
[agent/canvas.py:168]
```

**Sources:** [agent/component/base.py407-447](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L407-L447) [agent/canvas.py168-240](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L240)

## Tool and Agent Integration

The `Agent` component extends standard capabilities by binding external tools, including MCP (Model Context Protocol) servers.

- Tool Binding: The Agent component loads tools dynamically and binds them to an LLMBundle using LLMToolPluginCallSession agent/component/agent_with_tools.py72-114
- MCP Integration: Agent can connect to MCP servers via MCPToolCallSession, resolving server variables and custom headers agent/component/agent_with_tools.py102-110
- Tool Execution: Tools are invoked via tool_call_async in LLMToolPluginCallSession, which handles native Python tool implementations, MCPToolBinding, and async coroutines agent/tools/base.py59-91

**Sources:** [agent/component/agent_with_tools.py72-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L114) [agent/tools/base.py51-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/base.py#L51-L91) [common/mcp_tool_call_conn.py27](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/mcp_tool_call_conn.py#L27-L27)

## Error Handling and Cancellation

The component architecture includes built-in mechanisms for task management and failure recovery.

### Cancellation Check

Long-running components must periodically call `check_if_canceled()` [agent/component/base.py396-405](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L396-L405) This method checks:

1. The _canvas.is_canceled() flag agent/canvas.py270-271
2. The has_canceled(task_id) service, which looks for a cancellation flag in Redis agent/canvas.py34

### Exception Handling

If an exception occurs during `invoke`, the base class:

1. Logs the exception agent/component/base.py416
2. Sets the _ERROR output key with the error message agent/component/base.py414
3. Supports an exception_goto parameter to redirect workflow execution on failure agent/component/base.py50

**Sources:** [agent/component/base.py396-419](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L396-L419) [agent/canvas.py270-271](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L270-L271) [api/db/services/task_service.py34](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L34-L34)


---



<!-- ===== 8.3-built-in-components ===== -->

# Built-in Components

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/retrieval.py
- common/mcp_tool_call_conn.py
- rag/prompts/generator.py
- web/src/components/ui/tabs.tsx
- web/src/pages/agent/canvas/node/agent-node.tsx
- web/src/pages/agent/canvas/node/begin-node.tsx
- web/src/pages/agent/canvas/node/categorize-node.tsx
- web/src/pages/agent/canvas/node/index.tsx
- web/src/pages/agent/canvas/node/message-node.tsx
- web/src/pages/agent/canvas/node/retrieval-node.tsx
- web/src/pages/agent/canvas/node/switch-node.tsx
- web/src/pages/agent/form/agent-form/index.tsx
- web/src/pages/agent/form/agent-form/use-show-structured-output-dialog.ts
- web/src/pages/agent/form/agent-form/use-values.ts
- web/src/pages/agent/form/agent-form/use-watch-change.ts
- web/src/pages/agent/form/components/output.tsx
- web/src/pages/agent/form/retrieval-form/next.tsx
- web/src/pages/agent/form/string-transform-form/index.tsx
- web/src/pages/agent/form/switch-form/index.tsx
- web/src/pages/agent/form/tool-form/retrieval-form/index.tsx

This page documents the standard components available in RAGFlow's Canvas workflow system. These components serve as building blocks for constructing AI agent workflows, ranging from simple LLM invocations to complex multi-tool agent orchestrations with retrieval and conditional routing.

## Component Overview

RAGFlow provides a library of built-in components that inherit from `ComponentBase` [agent/component/base.py407-585](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L407-L585) Each component consists of a parameter class (inheriting from `ComponentParamBase` [agent/component/base.py40-187](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L187)) and an implementation class that provides the `_invoke` or `_invoke_async` method.

### Component Categories

Built-in components fall into several functional categories:

| Category | Components | Purpose |
| --- | --- | --- |
| **LLM Interaction** | LLM, Agent | Direct LLM calls with prompt templates and tool-augmented ReAct agents. |
| **Information Retrieval** | Retrieval, Search tools | Knowledge base search and context-aware content generation. |
| **Control Flow** | Categorize, Switch, Iteration, Loop | Conditional branching and repetitive execution logic. |
| **Data & Logic** | DataOperations, ListOperations, VariableAssigner | Manipulation of variables and data structures within the workflow. |
| **Integration** | Tool, Message, Invoke | External API calls, specialized tool functions, and user communication. |

Sources: [agent/component/base.py40-187](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L187) [agent/component/llm.py83-214](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L83-L214) [agent/component/agent_with_tools.py73-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L73-L142) [agent/component/categorize.py98-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L98-L162) [agent/canvas.py29-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L29-L30) [agent/canvas.py159-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L159-L161)

## LLM Component

The `LLM` component [agent/component/llm.py83-214](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L83-L214) provides direct access to language models with configurable prompts and generation parameters.

### Component Structure

```
Natural Language Space

Code Entity Space

inherits

uses

initializes

maps to

maps to

stores

executes

class LLM
(agent/component/llm.py:83)

class LLMParam
(agent/component/llm.py:34)

class LLMBundle
(api/db/services/llm_service.py)

class ComponentBase
(agent/component/base.py)

User Prompt Template

System Instructions

Model Parameters (Temp, TopP)

_invoke_async()
```

Sources: [agent/component/llm.py34-81](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L34-L81) [agent/component/llm.py83-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L83-L110) [agent/component/base.py407-420](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L407-L420)

### Variable Resolution in Prompts

The LLM component supports variable references using the syntax `{component_id@output_var}` or `{sys.variable}`. Variables are resolved during invocation via `get_variable_value` [agent/canvas.py195-236](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L195-L236) The method uses a regex pattern `pat` to match references like `sys.query` or `env.variable` [agent/canvas.py169-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L169-L173)

### Image Input Handling

The component automatically detects base64-encoded images in input variables via `_extract_data_images` [agent/component/llm.py131-154](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L131-L154) It removes image data from text content to avoid token bloat while passing them to the model [agent/component/llm.py172-188](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L172-L188)

## Agent Component

The `Agent` component [agent/component/agent_with_tools.py72-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L142) implements a ReAct (Reasoning + Acting) loop that iteratively calls language models to plan actions, invoke tools, and reflect on results.

### Architecture

```
Tool Integration

Agent Logic (agent/component/agent_with_tools.py)

inherits

uses

creates

calls

instantiates

proxies to

class Agent

class AgentParam

_load_tool_obj()

class LLMToolPluginCallSession
(agent/tools/base.py)

class MCPToolCallSession
(common/mcp_tool_call_conn.py)

class ToolBase
(agent/tools/base.py)
```

Sources: [agent/component/agent_with_tools.py38-71](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L38-L71) [agent/component/agent_with_tools.py72-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L111) [agent/tools/base.py22-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/base.py#L22-L30) [agent/component/agent_with_tools.py102-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L102-L111)

### Tool Loading and ReAct Loop

The agent loads tool components during initialization [agent/component/agent_with_tools.py78-93](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L78-L93) It handles tool calls by binding them to the `LLMBundle` [agent/component/agent_with_tools.py113-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L113-L114) The core execution logic follows the ReAct pattern where the LLM decides which tool to call based on prompts like `full_question` or `structured_output_prompt` [agent/component/agent_with_tools.py35](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L35-L35) It supports MCP (Model Context Protocol) servers for external tool integration via `MCPServerService` [agent/component/agent_with_tools.py102-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L102-L110)

## Retrieval Component

The `Retrieval` component [agent/tools/retrieval.py86-177](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L86-L177) extracts information from specified datasets or memories to provide context for LLMs.

### Key Features

- Hybrid Search: Combines weighted keyword similarity and vector similarity. The parameter keywords_similarity_weight controls this balance agent/tools/retrieval.py59
- Reranking: Supports optional rerank models to improve result precision via rerank_id agent/tools/retrieval.py66-130
- Retrieval Sources: Can switch between Dataset and Memory sources agent/tools/retrieval.py94-114 In the UI, this is represented by retrieval_from field web/src/pages/agent/form/retrieval-form/next.tsx71-74
- Metadata Filtering: Implements structured filtering on document metadata via apply_meta_data_filter and DocMetadataService agent/tools/retrieval.py137-175

Sources: [agent/tools/retrieval.py37-71](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L37-L71) [agent/tools/retrieval.py94-131](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L94-L131) [agent/tools/retrieval.py137-142](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L137-L142) [web/src/pages/agent/form/retrieval-form/next.tsx44-59](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/form/retrieval-form/next.tsx#L44-L59)

## Categorize Component

The `Categorize` component [agent/component/categorize.py98-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L98-L162) classifies user input into predefined categories and routes execution to different downstream components.

### Execution Flow

```
"LLMBundle (api/db/services/llm_service.py)"
"Categorize (agent/component/categorize.py)"
"Canvas (agent/canvas.py)"
"LLMBundle (api/db/services/llm_service.py)"
"Categorize (agent/component/categorize.py)"
"Canvas (agent/canvas.py)"
_invoke(query="user question")
update_prompt() [line 60-96]
async_chat(sys_prompt, user_prompt) [line 138]
"Category Name String"
Count occurrences in ans [line 147-150]
Set outputs: category_name, _next [line 158-159]
Output Data
```

Sources: [agent/component/categorize.py30-96](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L30-L96) [agent/component/categorize.py99-160](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L99-L160) [agent/component/categorize.py127-138](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L127-L138)

## Message Component

The `Message` component [agent/component/message.py69-228](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L69-L228) handles communication with the end-user, supporting streaming and variable interpolation.

### Logic Flow

- Variable Interpolation: Uses regex variable_ref_patt to find and replace placeholders with actual values from the canvas agent/component/message.py199-216
- Streaming: Implements an asynchronous generator _stream to yield content chunks as they are resolved agent/component/message.py194-228
- Memory Integration: Can automatically save messages to the conversation memory via queue_save_to_memory_task agent/component/message.py41
- Download Extraction: Identifies document metadata (e.g., doc_id, filename) in values to populate the downloads output agent/component/message.py73-107

Sources: [agent/component/message.py44-66](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L44-L66) [agent/component/message.py194-228](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L194-L228) [agent/component/message.py73-76](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L73-L76)

## Control Flow Components

### Switch Component

The `Switch` component handles conditional branching based on logical operations. It supports multiple conditions with logical operators (AND/OR).

- Frontend Configuration: The SwitchForm manages a list of conditions web/src/pages/agent/form/switch-form/index.tsx153-211
- Logic Operators: Supports logical operators configured via switchLogicOperatorOptions web/src/pages/agent/form/switch-form/index.tsx188
- Comparison Operators: Includes operators configured via switchOperatorOptions such as =, ≠, >, etc. web/src/pages/agent/form/switch-form/index.tsx156

### Iteration and Loop Components

- Iteration: Processes lists of items by looping through a sub-graph.
- Loop: Structured repetitive logic within the workflow canvas.
- ExitLoop: Terminates a loop based on specific conditions.

Sources: [web/src/pages/agent/form/switch-form/index.tsx153-211](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/form/switch-form/index.tsx#L153-L211) [agent/component/base.py575-582](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L575-L582)

## Component Base Logic

All components share a common lifecycle managed by `ComponentBase` [agent/component/base.py407-585](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L407-L585)

### Output and Error Handling

Components store results using `set_output(key, value)` [agent/component/base.py458-461](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L458-L461) Special output keys include:

- _ERROR: Captures execution errors agent/component/base.py463
- _elapsed_time: Tracks execution duration agent/component/base.py418

### Exception Routing

Components can configure automated responses to failures via `ComponentParamBase` [agent/component/base.py47-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L47-L50):

- Method: Defines behavior on error (e.g., AgentExceptionMethod.Goto) web/src/pages/agent/form/agent-form/index.tsx144-152
- Goto: Jump to a specific component on error agent/component/base.py50

Sources: [agent/component/base.py40-58](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L58) [agent/component/base.py407-470](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L407-L470) [web/src/pages/agent/form/agent-form/index.tsx31-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/form/agent-form/index.tsx#L31-L35)


---



<!-- ===== 8.4-workflow-execution-and-streaming ===== -->

# Workflow Execution and Streaming

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/retrieval.py
- common/mcp_tool_call_conn.py
- rag/prompts/generator.py
- web/src/components/message-input/next.tsx
- web/src/components/message-item/group-button.tsx
- web/src/components/message-item/hooks.ts
- web/src/components/message-item/index.tsx
- web/src/components/next-message-item/index.tsx
- web/src/hooks/logic-hooks.ts
- web/src/hooks/use-send-message.ts
- web/src/interfaces/database/chat.ts
- web/src/pages/agent/chat/box.tsx
- web/src/pages/agent/chat/chat-sheet.tsx
- web/src/pages/agent/chat/use-send-agent-message.ts
- web/src/pages/next-chats/hooks/use-send-shared-message.ts
- web/src/pages/next-chats/share/index.tsx
- web/src/utils/chat.ts

This page documents the **Canvas workflow execution engine**, which orchestrates asynchronous component execution and streams real-time events via Server-Sent Events (SSE). The `Graph` class in `agent/canvas.py` implements a graph-based execution model with support for cancellation, variable resolution, and dynamic path planning.

## Workflow Execution Overview

### Graph Execution Logic

The `Graph` class is the primary execution engine for agent workflows. It processes the Domain Specific Language (DSL) that defines components and their connections.

**Execution Model:**

```
Yes

No

Graph.init(dsl)
agent/canvas.py:84

Graph.load()
agent/canvas.py:96

Graph.reset()
agent/canvas.py:134

Set Global Variables
sys.query, sys.user_id, sys.files

has_canceled(task_id)?
api/db/services/task_service.py:34

yield MessageEventType.WorkflowStarted

Component._invoke() / _invoke_async()

Post-process Results
Handle Message/Error

Update Execution Path
self.path.append(next_id)

has_canceled(task_id)?

More components
to execute?

yield MessageEventType.WorkflowFinished
```

**Key Characteristics:**

- Async Generator: Yields SSE events for real-time updates to the frontend.
- Path-driven: Executes components defined in self.path agent/canvas.py111
- Variable Resolution: Dynamically resolves variables like {component_id@variable} or {sys.query} during execution using regex patterns agent/canvas.py169-193

**Sources:** [agent/canvas.py43-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L82) [agent/canvas.py134-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L134-L143) [agent/canvas.py169-193](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L169-L193) [api/db/services/task_service.py34-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L34-L35)

### Initialization and State Setup

Before execution, the `Graph` loads the DSL and initializes component objects:

[agent/canvas.py96-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L96-L112)

**Initialization Steps:**

| Step | Code Entity | Purpose |
| --- | --- | --- |
| Component Loading | `component_class(name)` | Dynamically loads component and param classes from the registry [agent/canvas.py101-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L101-L109) |
| Param Validation | `param.check()` | Validates component parameters (e.g., LLM IDs, thresholds) [agent/canvas.py105](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L105-L105) |
| Thread Pool | `ThreadPoolExecutor(max_workers=5)` | Manages execution of synchronous component methods [agent/canvas.py93](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L93-L93) |
| Variable Map | `self.dsl["globals"]` | Stores `sys.*` and `env.*` variables for the session [agent/canvas.py75-80](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L75-L80) |

**Sources:** [agent/canvas.py84-112](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L84-L112) [agent/component/base.py57-58](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L57-L58)

## SSE Event Streaming

### Event Types and Structure

RAGFlow yields SSE events as JSON objects. On the frontend, these are processed by hooks like `useSendMessageBySSE` [web/src/pages/agent/chat/use-send-agent-message.ts14-15](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/use-send-agent-message.ts#L14-L15)

**Event Types defined in `MessageEventType`:**

| Event Type | Source File | Data Payload |
| --- | --- | --- |
| `Message` | [web/src/pages/agent/chat/use-send-agent-message.ts47-48](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/use-send-agent-message.ts#L47-L48) | `content` chunk, `audio_binary`, `start_to_think` flag |
| `WorkflowFinished` | [web/src/pages/agent/chat/use-send-agent-message.ts83-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/use-send-agent-message.ts#L83-L85) | Final aggregate outputs, attachments, and downloads |
| `UserInputs` | [web/src/pages/agent/chat/use-send-agent-message.ts96-98](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/use-send-agent-message.ts#L96-L98) | Triggers UI forms for manual user input during workflow |
| `MessageEnd` | [web/src/pages/agent/chat/use-send-agent-message.ts149-151](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/use-send-agent-message.ts#L149-L151) | `reference` (retrieved chunks/docs) |

**Sources:** [web/src/pages/agent/chat/use-send-agent-message.ts45-93](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/use-send-agent-message.ts#L45-L93) [web/src/pages/agent/chat/use-send-agent-message.ts148-163](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/use-send-agent-message.ts#L148-L163)

### Frontend Event Processing

The frontend parses the raw SSE stream into structured JSON events to update the UI state.

**SSE Data Flow:**

```
"Graph (agent/canvas.py)"
"api.agentChatCompletion (web/src/utils/api.ts)"
"useSendMessageBySSE (web/src/hooks/use-send-message.ts)"
"AgentChatBox (web/src/pages/agent/chat/box.tsx)"
"Graph (agent/canvas.py)"
"api.agentChatCompletion (web/src/utils/api.ts)"
"useSendMessageBySSE (web/src/hooks/use-send-message.ts)"
"AgentChatBox (web/src/pages/agent/chat/box.tsx)"
loop
[Stream Reading]
send(body)
fetch("/agent/completion", {method: 'POST'})
run()
yield event
"data: {...}"
update derivedMessages
```

**Key Frontend Utility Functions:**

- findMessageFromList: Aggregates multiple Message events into a single content string. It specifically handles reasoning model tags by detecting start_to_think and end_to_think flags to wrap content in <think> tags web/src/pages/agent/chat/use-send-agent-message.ts45-93
- getLatestError: Extracts specific error messages from the component output _ERROR field or the SSE error code web/src/pages/agent/chat/use-send-agent-message.ts110-118

**Sources:** [web/src/pages/agent/chat/use-send-agent-message.ts45-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/use-send-agent-message.ts#L45-L118) [web/src/pages/agent/chat/box.tsx24-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/chat/box.tsx#L24-L40)

## Component Execution Details

### LLM and Message Streaming

The `LLM` and `Message` components handle content generation and output formatting.

- LLM Component: Manages chat completion via LLMBundle agent/component/llm.py89-91 It generates model parameters like temperature and top_p via LLMParam.gen_conf() agent/component/llm.py61-79 It also supports multi-modal inputs by extracting and stripping data:image/ URIs agent/component/llm.py132-188
- Categorize Component: A specialized LLM component that uses few-shot examples and system prompts to classify queries and determine the next execution path (_next) agent/component/categorize.py99-160

**Sources:** [agent/component/llm.py61-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L61-L91) [agent/component/llm.py132-188](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L132-L188) [agent/component/categorize.py99-160](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L99-L160)

### Agent with Tools (ReAct Loop)

The `Agent` component implements a multi-step reasoning loop with tool invocation:

- Tool Binding: Components listed in self._param.tools are instantiated and bound to the LLMBundle agent/component/agent_with_tools.py113-114
- MCP Integration: Connects to Model Context Protocol (MCP) servers to fetch tool metadata and execute calls via MCPToolCallSession agent/component/agent_with_tools.py102-110
- Tool Execution: Uses LLMToolPluginCallSession to manage the lifecycle of tool calls, including a callback to the canvas for logging agent/component/agent_with_tools.py111-112

**Sources:** [agent/component/agent_with_tools.py72-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L114) [common/mcp_tool_call_conn.py34](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/mcp_tool_call_conn.py#L34-L34)

### Retrieval Component

The `Retrieval` component performs document search across datasets and memories:

- Multi-Dataset Support: Resolves dataset IDs and handles backward compatibility with legacy kb_ids agent/tools/retrieval.py90-93
- Variable-based Search: Allows dynamic selection of datasets via variables like {component_id@kb_name} agent/tools/retrieval.py100-101
- Metadata Filtering: Supports dynamic resolution of metadata filters using regex-based variable resolution within the filter configuration agent/tools/retrieval.py137-171

**Sources:** [agent/tools/retrieval.py90-171](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L90-L171)

## Variable and Context Management

### Variable Resolution

The `Graph` resolves variables using a regex-based replacement system [agent/canvas.py169-193](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L169-L193)

**Resolution Hierarchy:**

1. System Variables: sys.query, sys.user_id, sys.conversation_turns, sys.files agent/canvas.py75-80
2. Component Outputs: component_id@variable_name (e.g., retrieval_0@chunks) agent/canvas.py199-204
3. Environment Variables: env.VARIABLE_NAME agent/canvas.py169

**Sources:** [agent/canvas.py169-204](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L169-L204)

### Context Fitting

To stay within LLM context limits, `message_fit_in` prunes history while ensuring the system prompt and latest query are prioritized [rag/prompts/generator.py69-125](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L69-L125) It calculates token counts using the `encoder` and trims content to fit `max_length` [rag/prompts/generator.py70-84](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L70-L84)

**Sources:** [rag/prompts/generator.py69-125](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/prompts/generator.py#L69-L125)

## Cancellation Mechanism

Workflow execution can be aborted asynchronously using Redis-based signaling.

- Trigger: The cancellation logic sets a cancel flag in Redis: REDIS_CONN.delete(f"{self.task_id}-cancel") is used during reset to clear existing signals agent/canvas.py140
- Check: The execution engine checks has_canceled(task_id) at the start of component execution agent/canvas.py34
- Exception: Components or the graph can raise TaskCanceledException if the cancel flag is detected in Redis agent/canvas.py38

**Sources:** [agent/canvas.py134-143](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L134-L143) [api/db/services/task_service.py34-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L34-L35)


---



<!-- ===== 8.5-state-and-variable-management ===== -->

# State and Variable Management

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/retrieval.py
- common/mcp_tool_call_conn.py
- rag/prompts/generator.py
- web/src/pages/agent/form/components/prompt-editor/enter-key-plugin.tsx
- web/src/pages/agent/form/components/prompt-editor/index.css
- web/src/pages/agent/form/components/prompt-editor/index.tsx
- web/src/pages/agent/form/components/prompt-editor/paste-handler-plugin.tsx
- web/src/pages/agent/form/components/prompt-editor/utils.ts
- web/src/pages/agent/form/components/prompt-editor/variable-node.tsx
- web/src/pages/agent/form/components/prompt-editor/variable-on-change-plugin.tsx
- web/src/pages/agent/form/components/prompt-editor/variable-picker-plugin.tsx
- web/src/pages/agent/form/components/query-variable.tsx
- web/src/pages/agent/form/components/select-with-secondary-menu.tsx
- web/src/pages/agent/form/components/structured-output-secondary-menu.tsx
- web/src/pages/agent/form/variable-assigner-form/dynamic-variables.tsx
- web/src/pages/agent/form/variable-assigner-form/index.tsx
- web/src/pages/agent/hooks/use-build-structured-output.ts
- web/src/pages/agent/hooks/use-get-begin-query.tsx
- web/src/pages/agent/use-agent-history-manager.ts
- web/src/pages/agent/utils/filter-agent-structured-output.ts

## Purpose and Scope

This document describes the state and variable management system within RAGFlow's Agent and Workflow Engine (Canvas). It covers how variables are named, scoped, and resolved across both the Python backend execution environment and the React-based frontend. The system enables dynamic data flow between components, maintains conversation context, and provides a standardized syntax for referencing data across the workflow graph.

**Sources:** [agent/canvas.py43-82](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L43-L82) [web/src/pages/agent/constant.ts16-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/constant.ts#L16-L30)

## Variable Naming Conventions and Scopes

RAGFlow uses three primary variable scopes, identified by specific prefixes or separators:

| Scope | Syntax | Example | Purpose |
| --- | --- | --- | --- |
| **System** | `sys.<name>` | `sys.query`, `sys.user_id` | Built-in workflow state managed by the engine. |
| **Environment** | `env.<name>` | `env.api_key` | User-defined persistent variables (Conversation Variables). |
| **Component Output** | `<component_id>@<output_name>` | `retrieval_0@content` | Output values produced by specific components. |

Variables are referenced in prompts or parameters using double curly braces: `{{sys.query}}`. The backend resolves these using a regex pattern that accounts for both single and double brace formatting [agent/canvas.py169](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L169-L169)

**Sources:** [agent/canvas.py168-193](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L193) [web/src/pages/agent/hooks/use-get-begin-query.tsx174-183](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/hooks/use-get-begin-query.tsx#L174-L183)

## Backend Variable Resolution Mechanism

The `Graph` class in `agent/canvas.py` serves as the central orchestrator for variable resolution during workflow execution.

### Key Resolution Functions

1. `get_value_with_variable(value)`: The entry point for string interpolation. It scans a string for {{variable}} patterns and replaces them with resolved values agent/canvas.py168-193
2. `get_variable_value(exp)`: The core logic that determines the scope. If the expression contains @, it splits it into cpn_id and var_nm to fetch from component outputs via self.get_component(cpn_id) agent/canvas.py195-207

### Data Types and Handling

The resolution system handles multiple data types to ensure compatibility with LLM prompts:

- Generators/Partials: If a variable refers to a stream (like an LLM response or retrieval generator), it iterates through the generator to produce a combined string agent/canvas.py179-183
- Strings: Returned directly agent/canvas.py184-185
- Complex Objects: Non-string objects (lists, dicts) are serialized to JSON using json.dumps with ensure_ascii=False agent/canvas.py187

**Sources:** [agent/canvas.py168-207](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L207)

## System Variables (sys.*)

System variables are initialized in the `globals` dictionary of the Canvas DSL. They track the state of the current execution task and are essential for providing context to components like `Retrieval` or `LLM`.

| Variable Name | Description |
| --- | --- |
| `sys.query` | The user's input string for the current turn [agent/canvas.py76](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L76-L76) |
| `sys.user_id` | The unique identifier for the tenant/user executing the flow [agent/canvas.py77](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L77-L77) |
| `sys.conversation_turns` | Counter for the number of rounds in the current session [agent/canvas.py78](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L78-L78) |
| `sys.files` | A list of files or attachments provided in the current turn [agent/canvas.py79](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L79-L79) |

**Sources:** [agent/canvas.py75-80](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L75-L80) [web/src/pages/agent/hooks/use-get-begin-query.tsx211-230](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/hooks/use-get-begin-query.tsx#L211-L230)

## Component State and Outputs

Each component inherits from `ComponentBase` and maintains its own `inputs` and `outputs` dictionaries within its `param` object (derived from `ComponentParamBase`) [agent/component/base.py43-44](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L43-L44)

### Variable Lifecycle in Components

1. Input Mapping: Components use get_input_elements_from_text to identify which variables they depend on by parsing prompts or script contents agent/component/base.py254-263
2. Resolution: Before execution, get_kwargs resolves these references into actual values. In the Message component, this involves consuming async generators if the input is a stream agent/component/message.py153-186
3. Output Setting: After execution, components call set_output(key, value) to store results. For example, the Categorize component sets category_name and _next agent/component/categorize.py157-158

### Structured Outputs and Metadata

Advanced components support structured JSON and metadata-based resolution:

- Agent Component: Supports structured output schemas and reasoning paths agent/component/agent_with_tools.py162-180
- Retrieval Component: Resolves meta_data_filter variables at runtime to apply dynamic dataset filters agent/tools/retrieval.py145-170
- LLM Component: Supports output_structure for defining expected JSON schemas agent/component/llm.py48-49

**Sources:** [agent/component/base.py40-52](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L52) [agent/component/message.py153-186](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L153-L186) [agent/component/agent_with_tools.py162-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L162-L180) [agent/component/categorize.py157-158](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L157-L158) [agent/component/llm.py49](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L49-L49) [agent/tools/retrieval.py145-170](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L145-L170)

## Variable Resolution Data Flow

The following diagram bridges the Natural Language references in prompts to the code entities that resolve them.

### Prompt to Code Entity Mapping

```
Code Entity Space (Python Backend)

Natural Language Space (Prompt)

Input String

Extracts 'sys.query'

Lookup

Extracts 'retrieval_0@content'

Lookup Component

Return

Final Interpolated String

'Hello {{retrieval_0@content}}, help me with {{sys.query}}'

Graph (agent/canvas.py)

get_value_with_variable()

get_variable_value()

ComponentBase (agent/component/base.py)

self.globals (dict)

param.outputs['content']

'Hello relevant_doc_text, help me with user_input'
```

**Sources:** [agent/canvas.py168-207](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L207) [agent/component/base.py40-52](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L52)

## Frontend State Management

The frontend manages variables via `useGraphStore` (Zustand) and specialized hooks for building selection menus.

### Variable Selection UI Logic

The frontend provides a variable picker (`VariablePickerPlugin`) that filters available options based on the graph structure. It uses `useBuildUpstreamNodeOutputOptions` to ensure a node can only reference outputs from its ancestors [web/src/pages/agent/hooks/use-get-begin-query.tsx95-106](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/hooks/use-get-begin-query.tsx#L95-L106)

```
"useGraphStore (web/src/pages/agent/store.ts)"
"useBuildGlobalWithBeginVariableOptions"
"useBuildUpstreamNodeOutputOptions"
"VariablePickerPlugin (web/src/pages/agent/form/components/prompt-editor/variable-picker-plugin.tsx)"
"useGraphStore (web/src/pages/agent/store.ts)"
"useBuildGlobalWithBeginVariableOptions"
"useBuildUpstreamNodeOutputOptions"
"VariablePickerPlugin (web/src/pages/agent/form/components/prompt-editor/variable-picker-plugin.tsx)"
Get available node outputs
nodes & edges
RFState Graph Structure
List of component@output
Get sys.* and env.*
getNode(BeginId)
Begin Node Config
List of sys.query, sys.files, etc.
```

**Sources:** [web/src/pages/agent/hooks/use-get-begin-query.tsx95-106](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/hooks/use-get-begin-query.tsx#L95-L106) [web/src/pages/agent/hooks/use-get-begin-query.tsx208-238](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/hooks/use-get-begin-query.tsx#L208-L238) [web/src/pages/agent/form/components/prompt-editor/variable-picker-plugin.tsx67-106](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/form/components/prompt-editor/variable-picker-plugin.tsx#L67-L106)

## Persistence and Memory

### Global State Persistence

The `globals` section of the DSL is persisted within the `dsl` JSON object. This includes `sys.*` defaults and any `env.*` variables defined by the user [agent/canvas.py75-81](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L75-L81) The `Graph.load()` method reconstructs component objects and their parameters from this DSL [agent/canvas.py96-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L96-L111)

### Variable Updating during Runtime

Components can dynamically update variables. The `Message` component, for instance, consumes variables from the canvas and triggers `queue_save_to_memory_task` to ensure the conversation history reflects the resolved variable values [agent/component/message.py41](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L41-L41) [agent/component/message.py212-225](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L212-L225) The `Categorize` component dynamically updates its system prompt based on category descriptions before invocation [agent/component/categorize.py59-96](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L59-L96)

**Sources:** [agent/canvas.py75-81](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L75-L81) [agent/canvas.py96-111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L96-L111) [agent/component/message.py41](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L41-L41) [agent/component/message.py212-225](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L212-L225) [agent/component/categorize.py59-96](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L59-L96)


---



<!-- ===== 8.6-agent-tools-and-react-loop ===== -->

# Agent Tools and ReAct Loop

Relevant source files
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/code_exec.py
- agent/tools/exesql.py
- agent/tools/retrieval.py
- common/mcp_tool_call_conn.py
- rag/prompts/generator.py
- web/src/pages/agent/canvas/node/dropdown/next-step-dropdown.tsx
- web/src/pages/agent/canvas/node/tool-node.tsx
- web/src/pages/agent/form/agent-form/agent-tools.tsx
- web/src/pages/agent/form/agent-form/tool-popover/index.tsx
- web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx
- web/src/pages/agent/form/agent-form/tool-popover/use-update-tools.ts
- web/src/pages/agent/form/agent-form/use-get-tools.ts
- web/src/pages/agent/form/exesql-form/index.tsx
- web/src/pages/agent/form/exesql-form/use-submit-form.ts
- web/src/pages/agent/form/tool-form/constant.tsx
- web/src/pages/agent/form/tool-form/exesql-form/index.tsx
- web/src/pages/agent/hooks/use-agent-tool-initial-values.ts
- web/src/pages/agent/options.ts
- web/src/pages/next-chats/hooks/use-build-form-refs.ts

This document describes the Agent component's tool integration system and its implementation of the ReAct (Reasoning + Acting) pattern for multi-step problem-solving. The Agent component extends the base LLM component to orchestrate iterative workflows where language models reason about tasks, invoke external tools, observe results, and synthesize final answers.

For general information about the Canvas workflow system and component architecture, see **Canvas Engine and DSL (8.1)** and **Component System Architecture (8.2)**. For details on the base LLM component without tool use, see **Built-in Components (8.3)**.

## Agent Component Overview

The Agent component is a specialized LLM component that implements ReAct-style tool use. Unlike a standard LLM component that generates single-shot text responses, the Agent can analyze tasks, plan actions by selecting tools, and reflect on observations to determine next steps.

The Agent component is defined in `agent/component/agent_with_tools.py` [agent/component/agent_with_tools.py72-73](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L73) and inherits from both `LLM` and `ToolBase`. It utilizes an iterative loop governed by the `max_rounds` parameter [agent/component/agent_with_tools.py67](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L67-L67)

### Agent vs. LLM Component

| Feature | LLM Component | Agent Component |
| --- | --- | --- |
| **Base Class** | `ComponentBase` [agent/component/llm.py82](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L82-L82) | `LLM`, `ToolBase` [agent/component/agent_with_tools.py72](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L72) |
| **Tool access** | None | Built-in, MCP, and Sub-Agents [agent/component/agent_with_tools.py78-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L78-L110) |
| **Iterations** | Single-shot | Multi-turn (`max_rounds`) [agent/component/agent_with_tools.py91](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L91-L91) |
| **Parameter Class** | `LLMParam` [agent/component/llm.py33](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L33-L33) | `AgentParam` [agent/component/agent_with_tools.py38](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L38-L38) |

Sources: [agent/component/agent_with_tools.py38-93](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L38-L93) [agent/component/llm.py33-92](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/llm.py#L33-L92) [agent/component/agent_with_tools.py72-73](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L72-L73)

## Tool Integration Architecture

The Agent architecture bridges the gap between high-level LLM reasoning and low-level code execution by mapping model tool-calls to specific Python objects or remote MCP sessions.

### Tool Registry and Mapping

Title: Natural Language Space to Code Entity Space: Tool Mapping

```
MappingLogic

CodeEntitySpace (Backend Tools)

NaturalLanguageSpace (Frontend/LLM)

Operator.Code

factory

ToolCommand (web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx)

LLM Function Call (agent/component/agent_with_tools.py)

Agent (agent/component/agent_with_tools.py)

Retrieval (agent/tools/retrieval.py)

ExeSQL (agent/tools/exesql.py)

CodeExec (agent/tools/code_exec.py)

component_class (agent/canvas.py)

_load_tool_obj (agent/component/agent_with_tools.py)
```

Sources: [agent/component/agent_with_tools.py134-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L134-L146) [agent/canvas.py29-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L29-L30) [agent/tools/exesql.py80](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/exesql.py#L80-L80) [agent/tools/code_exec.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/code_exec.py#L29-L29) [web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx48](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/form/agent-form/tool-popover/tool-command.tsx#L48-L48)

### Built-in Component Tools

Built-in tools are specialized modules that the Agent can invoke. The backend initializes these via `_load_tool_obj` [agent/component/agent_with_tools.py134-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L134-L146) which instantiates the component and its parameter object using the `component_class` factory.

**Key Tools:**

- Retrieval: Searches configured datasets for relevant content based on a query. It supports metadata filtering and reranking agent/tools/retrieval.py43-71
- ExeSQL: Executes SQL queries across multiple database types (MySQL, Postgres, OceanBase, Trino, etc.) agent/tools/exesql.py56 It performs safety checks to prevent access to the core rag_flow database agent/tools/exesql.py65-69
- CodeExec: Provides a sandbox for executing arbitrary code. It enforces output contracts and type validation for business results agent/tools/code_exec.py192-207
- Categorize: Uses an LLM to classify user queries into predefined categories and determines the next execution path (_next) agent/component/categorize.py157-158
- Message: Handles final output generation and streaming. It supports variable interpolation and download extraction agent/component/message.py109-149

### MCP (Model Context Protocol) Integration

The Agent supports the Model Context Protocol for dynamic tool discovery and execution.

- Session Management: MCPToolCallSession agent/component/agent_with_tools.py105 manages connections to MCP servers using server variables and custom headers.
- Metadata Conversion: mcp_tool_metadata_to_openai_tool agent/component/agent_with_tools.py109 converts MCP tool schemas into the OpenAI-compatible function format.
- Tool Binding: MCP tools are bound to the LLMBundle as MCPToolBinding objects agent/component/agent_with_tools.py110

Sources: [agent/tools/retrieval.py43-71](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/retrieval.py#L43-L71) [agent/tools/exesql.py56-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/exesql.py#L56-L69) [agent/tools/code_exec.py192-207](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/code_exec.py#L192-L207) [agent/component/categorize.py157-158](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L157-L158) [agent/component/message.py109-149](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L109-L149) [agent/component/agent_with_tools.py105](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L105-L105) [agent/component/agent_with_tools.py109](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L109-L109) [agent/component/agent_with_tools.py110](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L110-L110)

## ReAct Loop and Execution Flow

The Agent's reasoning process follows the ReAct pattern, iteratively calling tools until a final answer is produced or the `max_rounds` limit is reached.

### Visual Workflow Integration

Title: Agent Tool Connectivity in Canvas

```
BackendExecutionSpace

bind_tools

callback

CanvasUISpace

triggers

AgentNode (web/src/pages/agent/canvas/node/agent-node.tsx)

NextStepDropdown (web/src/pages/agent/canvas/node/dropdown/next-step-dropdown.tsx)

Graph.run (agent/canvas.py)

Agent.invoke_async (agent/tools/base.py)

LLMBundle.chat (api/db/services/llm_service.py)

tool_use_callback (agent/canvas.py)
```

Sources: [agent/component/agent_with_tools.py111-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L111-L114) [agent/tools/base.py170-186](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/base.py#L170-L186) [web/src/pages/agent/canvas/node/dropdown/next-step-dropdown.tsx17-28](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/canvas/node/dropdown/next-step-dropdown.tsx#L17-L28)

### Execution Logic

The `Agent` component's execution flow involves:

1. Tool Binding: Tools (including built-in and MCP) are indexed and bound to the LLMBundle during initialization agent/component/agent_with_tools.py78-114
2. Message Fitting: System and user prompts are combined and truncated to fit the model's context window using message_fit_in agent/component/agent_with_tools.py116-121
3. Tool Call Session: An LLMToolPluginCallSession agent/component/agent_with_tools.py112 is created to handle the execution of tool calls generated by the LLM. This session wraps the actual tool objects and provides a unified interface for the LLM to interact with them agent/tools/base.py51-95
4. Iterative Reasoning: The LLMBundle manages the loop, invoking the callback (pointing to self._canvas.tool_use_callback) whenever a tool needs to be executed agent/component/agent_with_tools.py111 The LLMToolPluginCallSession.tool_call_async method agent/tools/base.py59-91 then dispatches the call to the appropriate tool's invoke_async or invoke method, handling both native Python tools and remote MCP tools.

Sources: [agent/component/agent_with_tools.py78-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L78-L114) [agent/component/agent_with_tools.py116-121](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L116-L121) [agent/component/agent_with_tools.py112](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L112-L112) [agent/tools/base.py51-95](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/base.py#L51-L95) [agent/component/agent_with_tools.py111](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L111-L111) [agent/tools/base.py59-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/base.py#L59-L91)

## Tool Parameter Management

Tools are configured via parameter classes that inherit from `ComponentParamBase` [agent/component/base.py40](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/base.py#L40-L40) or `ToolParamBase` [agent/tools/base.py97](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/base.py#L97-L97)

### Variable Resolution

The `Graph` class provides `get_variable_value` [agent/canvas.py195-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L195-L210) and `get_value_with_variable` [agent/canvas.py168-193](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L193) to resolve placeholders like `{sys.query}` or `{component_id@variable_name}`. This allows tools like `ExeSQL` to dynamically resolve SQL queries from user input [agent/tools/exesql.py111-121](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/exesql.py#L111-L121)

### Categorize Component Logic

The `Categorize` component [agent/component/categorize.py98](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L98-L98) performs classification by:

1. Building a dynamic system prompt containing category descriptions and examples agent/component/categorize.py59-89
2. Invoking the LLM to classify the input query agent/component/categorize.py137
3. Counting occurrences of category names in the response and routing to the corresponding downstream component via _next agent/component/categorize.py146-158

### Message Component and Streaming

The `Message` component [agent/component/message.py69](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L69-L69) handles output delivery:

- Streaming: An async generator _stream agent/component/message.py194-228 yields content chunks as they are resolved from variables or partial results.
- Download Extraction: Identifies file download info within content using _is_download_info agent/component/message.py73-76
- Stringification: Resolves complex values (lists, dicts, partials) into strings for display using _stringify_message_value agent/component/message.py109-149

Sources: [agent/canvas.py168-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L168-L210) [agent/component/categorize.py59-158](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/categorize.py#L59-L158) [agent/component/message.py194-228](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/message.py#L194-L228) [agent/component/agent_with_tools.py134-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/component/agent_with_tools.py#L134-L146)


---



<!-- ===== 8.7-canvas-api-and-management ===== -->

# Canvas API and Management

Relevant source files
- api/apps/restful_apis/agent_api.py
- api/db/services/canvas_service.py
- api/db/services/conversation_service.py
- api/db/services/tenant_llm_service.py
- test/testcases/test_http_api/test_session_management/test_agent_completions.py
- test/testcases/test_http_api/test_session_management/test_agent_sessions.py
- test/testcases/test_http_api/test_session_management/test_chat_completions.py
- test/testcases/test_web_api/test_agent_app/test_agents_webhook_unit.py
- test/unit_test/api/apps/restful_apis/test_get_agent_session.py
- test/unit_test/api/db/services/test_dialog_service_final_answer.py
- test/unit_test/api/db/services/test_dialog_service_use_sql_source_columns.py
- web/src/components/home-card.tsx
- web/src/components/ui/card.tsx
- web/src/hooks/use-agent-request.ts
- web/src/interfaces/database/agent.ts
- web/src/pages/agent/gobal-variable-sheet/index.tsx
- web/src/pages/agent/hooks/use-build-dsl.ts
- web/src/pages/agent/hooks/use-save-graph.ts
- web/src/pages/agent/index.tsx
- web/src/pages/agents/agent-card.tsx
- web/src/pages/agents/agent-dropdown.tsx
- web/src/pages/agents/agent-templates.tsx
- web/src/pages/agents/create-agent-dialog.tsx
- web/src/pages/agents/create-agent-form.tsx
- web/src/pages/agents/template-card.tsx
- web/src/pages/agents/template-sidebar.tsx
- web/src/pages/agents/use-rename-agent.ts
- web/src/services/agent-service.ts

This document describes the HTTP API endpoints and management operations for Canvas workflows. Canvas workflows (also called Agents or DataFlows) are visual workflow definitions that can be created, executed, versioned, and shared. For information about Canvas execution internals, see [Canvas Engine and DSL](/infiniflow/ragflow/8.1-canvas-engine-and-dsl). For component architecture, see [Component System Architecture](/infiniflow/ragflow/8.2-component-system-architecture).

**Scope**: This page covers the REST API layer, database services for Canvas persistence, session management, versioning, and runtime state handling across both Python and Go implementations.

## API Architecture

The Canvas API is organized into functional groups for template management, CRUD operations, and execution. These are primarily implemented in `api/apps/restful_apis/agent_api.py` and mirrored in the Go server's `internal/handler/agent.go`.

Title: Canvas API Entity Space

```
Data_Models_api/db/db_models.py

Database_Services_Python

Frontend_State_web/src/pages/agent/

Canvas_API_Python_api/apps/restful_apis/agent_api.py

list_agent_sessions [GET]

agent_completion [POST]

CanvasReplicaService
(canvas_replica_service.py)

useGraphStore (store.ts)

useSaveGraph (hooks/use-save-graph.ts)

useFetchDataOnMount (hooks/use-fetch-data.ts)

UserCanvasService
(api/db/services/canvas_service.py)

CanvasTemplateService
(api/db/services/canvas_service.py)

UserCanvasVersionService
(api/db/services/user_canvas_version.py)

ConversationService
(api/db/services/conversation_service.py)

UserCanvas (DB Table)

UserCanvasVersion (DB Table)

Conversation (DB Table)
```

**Sources**: [api/apps/restful_apis/agent_api.py34-51](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/agent_api.py#L34-L51) [api/db/services/canvas_service.py44-45](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L44-L45) [web/src/pages/agent/index.tsx93-110](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/index.tsx#L93-L110) [api/db/services/conversation_service.py34-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/conversation_service.py#L34-L35)

## Canvas Lifecycle Management

### Creating and Updating Canvases

The creation and update of canvases involve persisting the Domain Specific Language (DSL) that defines the graph. Access is strictly controlled via decorators that verify tenant permissions.

- Ownership Validation: Python uses _require_canvas_owner_sync to ensure only the agent owner can perform destructive or sensitive operations api/apps/restful_apis/agent_api.py94-100 General access is checked via UserCanvasService.accessible api/apps/restful_apis/agent_api.py74-91
- Tag Management: Agents support categorization via tags. The UserCanvasService handles tag filtering by wrapping values with commas to ensure precise matches (e.g., preventing 'ml' from matching 'ml-ops') api/db/services/canvas_service.py180-186
- Persistence: Updates are handled via updateAgent which modifies the UserCanvas record and can trigger version snapshots web/src/services/agent-service.ts128-140

### Retrieving Canvases

The system provides paginated retrieval with keyword filtering and category selection (e.g., `agent_canvas` vs `dataflow_canvas`).

| Feature | Implementation | File Reference |
| --- | --- | --- |
| **Keyword Search** | Case-insensitive `contains` on title | [api/db/services/canvas_service.py169-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L169-L173) |
| **Tenant Isolation** | Filters by `user_id` and `permission` (TEAM vs PRIVATE) | [api/db/services/canvas_service.py170-177](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L170-L177) |
| **Pagination** | `page_number` and `items_per_page` parameters | [api/db/services/canvas_service.py193-195](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L193-L195) |
| **Category Filter** | `canvas_category` enum check | [api/db/services/canvas_service.py178-179](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L178-L179) |

**Sources**: [api/db/services/canvas_service.py142-196](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L142-L196) [api/apps/restful_apis/agent_api.py74-100](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/agent_api.py#L74-L100) [web/src/hooks/use-agent-request.ts131-200](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/hooks/use-agent-request.ts#L131-L200)

## Canvas Execution and Sessions

### Session Management

Sessions track multi-turn interactions with Agent canvases. The frontend uses `createAgentSession` to initialize a new conversation context [web/src/services/agent-service.ts179-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/agent-service.ts#L179-L181)

- Session Normalization: The API layer maps internal database structures (like dialog_id) to the API-facing agent_id and formats references/citations for the frontend. This includes converting chunk metadata like docnm_kwd to document_name api/apps/restful_apis/agent_api.py133-178
- Deterministic IDs: For channel-based chats (e.g., external integrations), ConversationService generates deterministic IDs derived from channel_id and chat_id to maintain history across restarts api/db/services/conversation_service.py59-67

### Workflow Execution Flow

Execution can be triggered in several modes:

1. Debug Mode: Triggered from the canvas editor via useSaveGraphBeforeOpeningDebugDrawer which ensures the current graph state is saved before running web/src/pages/agent/index.tsx111-118
2. Standard Run: Executed via agent_completion (mapped to agentChatCompletion in the frontend) which handles streaming SSE responses api/apps/restful_apis/agent_api.py38-44 web/src/services/agent-service.ts65-68
3. Pipeline Mode: Specialized execution for data processing tasks, using useRunDataflow to manage log fetching and trace monitoring web/src/pages/agent/index.tsx223-227

Title: Agent Execution Sequence

```
"agent/canvas.py"
"ConversationService"
"CanvasReplicaService"
"api/apps/restful_apis/agent_api.py"
"web/src/pages/agent/index.tsx"
"agent/canvas.py"
"ConversationService"
"CanvasReplicaService"
"api/apps/restful_apis/agent_api.py"
"web/src/pages/agent/index.tsx"
agent_completion(agent_id, query)
get_replica(agent_id)
async_completion(tenant_id, chat_id, question)
run_workflow(canvas, query)
Yield SSE Events
Streamed Data
commit_runtime_replica()
SSE Response (text/event-stream)
```

**Sources**: [api/apps/restful_apis/agent_api.py185-200](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/agent_api.py#L185-L200) [api/db/services/conversation_service.py141-191](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/conversation_service.py#L141-L191) [web/src/pages/agent/index.tsx205-221](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/index.tsx#L205-L221)

## Version and Template Management

### Versioning System

The `UserCanvasVersion` model stores snapshots of the Canvas DSL.

- Service Layer: UserCanvasVersionService manages the persistence and retrieval of these snapshots api/db/services/user_canvas_version.py
- Frontend: The VersionDialog allows users to browse and switch between these snapshots, while useAgentHistoryManager tracks local changes web/src/pages/agent/index.tsx70-71

### Template System

Templates allow users to bootstrap agents from pre-defined logic.

- Listing: CanvasTemplateService provides access to the canvas_template table api/db/services/canvas_service.py34-35
- DSL Structure: Templates include a complete DSL object with components, graph layout, and global variables web/src/interfaces/database/agent.ts88-109
- Bootstrapping: When creating an agent from a template, the template's DSL and category are copied to the new UserCanvas record web/src/pages/agents/agent-templates.tsx44-62

### Global Settings and Widgets

Agents store UI-specific configuration within their DSL globals.

- Widget Settings: Key sys.widget_settings stores configuration for embedded chat widgets, including themes and initial messages web/src/pages/agent/index.tsx88
- Variables: The globals and variables dictionaries in the DSL interface manage environment-level state and user-defined parameters web/src/interfaces/database/agent.ts40-51

**Sources**: [api/db/services/canvas_service.py34-42](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/canvas_service.py#L34-L42) [web/src/interfaces/database/agent.ts40-109](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/interfaces/database/agent.ts#L40-L109) [web/src/pages/agent/index.tsx88-93](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agent/index.tsx#L88-L93) [web/src/pages/agents/agent-templates.tsx44-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/agents/agent-templates.tsx#L44-L72)


---



<!-- ===== 9-retrieval-and-search-system ===== -->

# Retrieval and Search System

Relevant source files
- api/db/__init__.py
- api/db/db_models.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- common/doc_store/es_conn_base.py
- common/doc_store/infinity_conn_base.py
- conf/infinity_mapping.json
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/cache_file_svr.py
- rag/svr/task_executor.py
- rag/utils/azure_sas_conn.py
- rag/utils/azure_spn_conn.py
- rag/utils/es_conn.py
- rag/utils/infinity_conn.py
- rag/utils/minio_conn.py
- rag/utils/opensearch_conn.py
- rag/utils/oss_conn.py
- rag/utils/redis_conn.py
- rag/utils/s3_conn.py
- test/unit_test/rag/utils/test_opensearch_doc_meta.py
- test/unit_test/rag/utils/test_opensearch_hybrid_search.py

## Purpose and Scope

This document provides an overview of RAGFlow's retrieval and search system, which combines BM25 text search with vector similarity search to retrieve relevant document chunks for conversational AI responses. The system supports hybrid search strategies, reranking, metadata filtering, and automatic citation insertion.

For information about the document storage layer implementations, see [Document Store Abstraction](/infiniflow/ragflow/9.1-document-store-abstraction). For details on query processing algorithms, see [Query Processing and Hybrid Search](/infiniflow/ragflow/9.2-query-processing-and-hybrid-search). For reranking model integration, see [Reranking and Filtering](/infiniflow/ragflow/9.3-reranking-and-filtering). For context formatting and LLM response generation, see [Response Generation and Citations](/infiniflow/ragflow/9.4-response-generation-and-citations).

## System Architecture

The retrieval system operates as a bridge between user queries and the document store, coordinating embedding generation, hybrid search, reranking, and citation insertion.

### Retrieval Orchestration Diagram

```
Models

Document_Stores

Search_Pipeline_rag_nlp_search_py

Retrieval_Orchestration

Entry_Points

DialogService.async_chat()
api/db/services/dialog_service.py

retrieval_test()
api/apps/chunk_app.py

get_tenant_default_model_by_type()
api/db/joint_services/tenant_model_service.py

settings.retriever
Dealer instance
common/settings.py

Dealer.search()
rag/nlp/search.py:132

get_vector()
rag/nlp/search.py:53

Hybrid Search
BM25 + Vector
rag/nlp/search.py:115-148

Dealer.rerank()
rag/nlp/search.py:296

_prune_deleted_chunks()
rag/nlp/search.py:76

DocStoreConnection
common/doc_store/doc_store_base.py

ESConnection
rag/utils/es_conn.py

InfinityConnection
rag/utils/infinity_conn.py

LLMBundle
api/db/services/llm_service.py:79

Embedding Model

Rerank Model

DocumentService.get_by_ids()
api/db/services/document_service.py
```

**Sources:**

- api/db/services/dialog_service.py36-42
- rag/nlp/search.py37-61
- rag/nlp/search.py132-172
- api/db/services/llm_service.py79-82

## Core Components

### Dealer Class

The `Dealer` class [rag/nlp/search.py37-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L37-L41) is the central orchestrator for retrieval operations. It encapsulates:

- Query processing with the FulltextQueryer rag/nlp/query.py28-40 for BM25 tokenization and expansion.
- Document store connection management via self.dataStore rag/nlp/search.py40
- Vector embedding generation through get_vector() rag/nlp/search.py53-61
- Hybrid search coordination using MatchDenseExpr and MatchTextExpr rag/nlp/search.py53-181
- Stale result cleanup via _prune_deleted_chunks() rag/nlp/search.py76-118

The system instantiates a singleton `Dealer` instance as `settings.retriever` [common/settings.py](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py) that is used throughout the application.

### Document Store Abstraction

RAGFlow supports multiple document store backends through a unified interface defined in `DocStoreConnection` [common/doc_store/doc_store_base.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/doc_store_base.py#L25-L25):

| Store | Implementation | Engine Flag |
| --- | --- | --- |
| Elasticsearch | `ESConnection` [rag/utils/es_conn.py62](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L62-L62) | `DOC_ENGINE_ES` |
| Infinity | `InfinityConnection` [rag/utils/infinity_conn.py30](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L30-L30) | `DOC_ENGINE_INFINITY` |
| OceanBase | `OceanBaseConnection` [rag/utils/opensearch_conn.py62](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L62-L62) | `DOC_ENGINE_OCEANBASE` |

Each implementation provides the `search()` method [rag/nlp/search.py132-172](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L132-L172) that accepts:

- Source fields to retrieve (src) rag/nlp/search.py149-153
- Highlight fields for keyword emphasis rag/nlp/search.py168-172
- Filter conditions (kb_id, doc_id, etc.) via get_filters() rag/nlp/search.py120-130
- Match expressions (text, dense vector) rag/nlp/search.py173-180

### LLMBundle for Models

The `LLMBundle` class [api/db/services/llm_service.py79](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L79-L79) provides a unified interface for:

- Embedding models: Used via emb_mdl.encode_queries to generate vector representations rag/nlp/search.py54
- Rerank models: Computes relevance scores between query and texts.
- Chat models: Used for response generation and query refinement.

**Sources:**

- rag/nlp/search.py37-61
- api/db/services/llm_service.py79-82
- rag/nlp/query.py28-40
- common/doc_store/doc_store_base.py25
- rag/utils/es_conn.py62
- rag/utils/infinity_conn.py30

## Retrieval Flow in Conversational AI

The following sequence diagram shows how retrieval integrates into the conversational AI pipeline:

```
RerankModel
DocStore
EmbedModel
Dealer
get_models
async_chat
User
RerankModel
DocStore
EmbedModel
Dealer
get_models
async_chat
User
opt
[Reranking enabled]
alt
[Has kb_ids]
Question + Dialog config
Load models config
embd_mdl, rerank_mdl, chat_mdl
Refine query if multiturn (full_question)
apply_meta_data_filter
search(req, idx_names, kb_ids, emb_mdl)
encode_queries(question)
query_vector
Build MatchTextExpr (BM25)
Build MatchDenseExpr (vector)
search(filters, matchExprs, orderBy, limit)
SearchResult (ids, fields, highlights)
rerank(sres, query, rerank_mdl)
similarity(query, chunks)
Rerank scores
sres (ids, field, highlight)
chunks_format() - format context
Generate LLM response (streaming)
Final answer with citations
```

**Key steps:**

1. Model Loading: Load embedding, chat, and optional rerank models for the dialog's knowledge bases via get_tenant_default_model_by_type api/db/joint_services/tenant_model_service.py82
2. Query Refinement: If multi-turn conversation is enabled, use LLM to generate a standalone question via full_question() rag/prompts/generator.py49
3. Metadata Filtering: Apply document metadata filters if configured via apply_meta_data_filter api/db/services/dialog_service.py37
4. Hybrid Search: Combine BM25 text search with vector similarity using the Dealer.search orchestration rag/nlp/search.py132-172
5. Reranking: Optionally rerank results using a specialized rerank model or similarity-based scores rag/nlp/search.py296
6. Citation Insertion: Inject reference markers into the answer by matching answer sentences back to source chunks rag/prompts/generator.py49

**Sources:**

- api/db/services/dialog_service.py30-55
- rag/nlp/search.py132-172
- rag/prompts/generator.py49
- api/db/joint_services/tenant_model_service.py82

## Hybrid Search Strategy

RAGFlow implements a hybrid search approach that combines lexical (BM25) and semantic (vector) retrieval:

### Match Expressions

The search pipeline builds match expressions [rag/nlp/search.py158-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L158-L181):

1. **MatchTextExpr** (BM25):
Generated fromFulltextQueryer.question()rag/nlp/query.py42-181Searches across tokenized fields defined inFulltextQueryer:title_tks,important_kwd,content_ltks, etc.rag/nlp/query.py32-40Uses field-specific weights (e.g.,title_tks^10,important_kwd^30).2. **MatchDenseExpr** (Vector):
Query embedding fromget_vector()rag/nlp/search.py53-61Matches againstq_{dim}_veccolumnrag/nlp/search.py60Uses cosine similarity metric with a configurable thresholdrag/nlp/search.py61

### Safety and Pruning

The `Dealer` includes a `_prune_deleted_chunks` mechanism [rag/nlp/search.py76-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L76-L118) to ensure that retrieved chunks correspond to existing documents in the database by checking `DocumentService.get_by_ids()` [rag/nlp/search.py72](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L72-L72) This prevents surfacing content from documents that were deleted but whose chunks haven't been fully cleaned from the document store.

**Sources:**

- rag/nlp/search.py132-172
- rag/nlp/query.py32-40
- rag/nlp/search.py53-61
- rag/nlp/search.py76-118

## Reranking and Scoring

### Rerank Method

The `Dealer.rerank()` method [rag/nlp/search.py296-387](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L296-L387) refines search results using multiple scoring mechanisms:

```
SearchResult
BM25+Vector scores

Extract chunk vectors

Tokenize content

hybrid_similarity()
Token + Vector similarity

Query vector

Query tokens

Combine:
tkweight * token_sim
+ vtweight * vector_sim

_rank_feature_scores()
Tag similarity + pagerank

Final Score:
hybrid_sim * 100
+ rank_feature

Re-sorted results
```

**Scoring components:**

1. **Hybrid Similarity** [rag/nlp/query.py183-187](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L183-L187):
Token overlap:token_similaritycomputes overlap between query and chunk tokens.Vector cosine: Between query embedding and chunk embedding.Formula:vtweight * vector_sim + tkweight * token_simrag/nlp/query.py1832. **Rank Feature** [rag/nlp/search.py269-294](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L269-L294):
Tag similarity: Cosine similarity between query tags and chunk tagsrag/nlp/search.py286PageRank: Document importance score fromPAGERANK_FLDrag/nlp/search.py28

**Sources:**

- rag/nlp/search.py296-387
- rag/nlp/query.py183-187
- rag/nlp/search.py269-294
- common/constants.py104

## API Integration

### Metadata Filtering

Metadata filtering is handled during the search request construction. The `get_filters` method [rag/nlp/search.py120-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L120-L130) maps request parameters to document store filters, including `kb_ids`, `doc_ids`, and specific metadata fields like `knowledge_graph_kwd`, `entity_kwd`, or `available_int`. Advanced filtering logic using boolean conditions is applied via `apply_meta_data_filter` [api/db/services/dialog_service.py37](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L37-L37)

**Sources:**

- rag/nlp/search.py120-130
- api/db/services/dialog_service.py37


---



<!-- ===== 9.1-document-store-abstraction ===== -->

# Document Store Abstraction

Relevant source files
- common/asyncio_utils.py
- common/doc_store/doc_store_base.py
- common/doc_store/es_conn_base.py
- common/doc_store/infinity_conn_base.py
- common/doc_store/infinity_conn_pool.py
- common/settings.py
- conf/infinity_mapping.json
- internal/dao/memory.go
- internal/dao/user_tenant.go
- internal/engine/elasticsearch/chunk.go
- internal/engine/elasticsearch/chunk_helpers_test.go
- internal/engine/elasticsearch/chunk_test.go
- internal/engine/elasticsearch/kg_test.go
- internal/engine/infinity/chunk.go
- internal/engine/types/types.go
- internal/handler/memory.go
- internal/handler/memory_message_test.go
- internal/service/memory.go
- internal/service/memory_message_test.go
- internal/service/nlp/query_builder_test.go
- internal/service/nlp/reranker.go
- internal/service/nlp/reranker_normalize_test.go
- internal/service/nlp/retrieval.go
- rag/graphrag/general/smoke.py
- rag/graphrag/light/smoke.py
- rag/graphrag/search.py
- rag/graphrag/utils.py
- rag/svr/cache_file_svr.py
- rag/utils/azure_sas_conn.py
- rag/utils/azure_spn_conn.py
- rag/utils/es_conn.py
- rag/utils/infinity_conn.py
- rag/utils/minio_conn.py
- rag/utils/ob_conn.py
- rag/utils/opensearch_conn.py
- rag/utils/oss_conn.py
- rag/utils/redis_conn.py
- rag/utils/s3_conn.py
- test/unit_test/rag/graphrag/test_graphrag_utils.py
- test/unit_test/rag/utils/test_opensearch_doc_meta.py
- test/unit_test/rag/utils/test_opensearch_hybrid_search.py

## Purpose and Scope

This document describes RAGFlow's document store abstraction layer, which provides a unified interface for interacting with multiple search engine backends. The abstraction enables RAGFlow to support Elasticsearch, Infinity, OceanBase, and OpenSearch as pluggable storage engines without modifying application code. This layer handles chunk storage, vector indexing, full-text search, and hybrid retrieval operations.

The abstraction ensures that the RAG pipeline remains agnostic of the underlying storage technology, whether it is a traditional search engine (Elasticsearch), a high-performance AI-native database (Infinity), or a distributed relational database with vector capabilities (OceanBase).

## Architecture Overview

The document store abstraction follows a three-tier architecture: an abstract interface, backend-specific base classes, and concrete implementations selected at runtime via environment configuration.

**Diagram: Document Store Abstraction Layers**

```

```

Sources: [common/doc_store/doc_store_base.py148](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/doc_store_base.py#L148-L148) [common/settings.py85-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L85-L91) [rag/utils/infinity_conn.py29-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L29-L30) [rag/utils/es_conn.py61-62](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L61-L62) [rag/utils/ob_conn.py33-34](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L33-L34) [rag/utils/opensearch_conn.py65](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L65-L65)

## DocStoreConnection Interface

The `DocStoreConnection` abstract base class defines the contract that all backend implementations must satisfy. It is organized into functional categories for index management, CRUD, and retrieval.

### Database and Index Operations

| Method | Parameters | Purpose |
| --- | --- | --- |
| `db_type()` | None | Returns backend type identifier (`"elasticsearch"`, `"infinity"`, `"oceanbase"`, `"opensearch"`) |
| `health()` | None | Returns cluster health status including availability and resource metrics |
| `create_idx()` | `indexName, knowledgebaseId, vectorSize, parser_id` | Creates index/table with schema for given vector dimensions |
| `delete_idx()` | `indexName, knowledgebaseId` | Drops index/table |
| `index_exist()` | `indexName, knowledgebaseId` | Checks if index/table exists |
| `create_doc_meta_idx()` | `index_name` | Creates a dedicated index for document metadata |
| `refresh_idx()` | `index_name` | Refreshes an index to make recent changes visible |

Sources: [common/doc_store/doc_store_base.py34-45](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/doc_store_base.py#L34-L45) [rag/utils/opensearch_conn.py163-169](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L163-L169) [rag/utils/opensearch_conn.py175-186](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L175-L186)

### CRUD and Search Operations

The `search()` method is the primary entry point for retrieval, supporting hybrid search, filtering, and aggregation.

```
@abstractmethod
def search(
    self,
    select_fields: list[str],
    highlight_fields: list[str],
    condition: dict,
    match_expressions: list[MatchExpr],
    order_by: OrderByExpr,
    offset: int,
    limit: int,
    index_names: str | list[str],
    knowledgebase_ids: list[str],
    agg_fields: list[str] | None = None,
    rank_feature: dict | None = None
):
```

**Key Parameters:**

- condition: A dictionary of filter criteria (e.g., {"available_int": 1}).
- match_expressions: A list of MatchTextExpr (BM25), MatchDenseExpr (Vector), or FusionExpr to execute.
- knowledgebase_ids: Used for multi-tenancy partitioning or table selection.

Sources: [common/doc_store/doc_store_base.py34-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/doc_store_base.py#L34-L50) [rag/utils/infinity_conn.py94-107](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L94-L107)

## Backend Implementations

### Infinity Implementation

Infinity uses a table-per-knowledgebase strategy for chunk storage. It performs significant field mapping to align RAGFlow's logical fields with physical database columns.

**Field Conversion Logic:** Infinity maps logical RAGFlow fields to specific columns and full-text indexes defined in `conf/infinity_mapping.json`.

| RAGFlow Field | Infinity Column | Full-Text Index |
| --- | --- | --- |
| `docnm_kwd` | `docnm` | `ft_docnm_rag_coarse` |
| `content_ltks` | `content` | `ft_content_rag_coarse` |
| `important_kwd` | `important_keywords` | `ft_important_keywords_rag_coarse` |

Sources: [rag/utils/infinity_conn.py61-86](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L61-L86) [conf/infinity_mapping.json9-16](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/infinity_mapping.json#L9-L16)

**Implementation Details:**

- Table Partitioning: Chunks are stored in tables named {index_name}_{kb_id} rag/utils/infinity_conn.py164-165
- Keyword Handling: Infinity treats *_kwd fields as keyword lists, except for specific exclusions like knowledge_graph_kwd rag/utils/infinity_conn.py38-42
- Meta Contention: Implements _retry_on_meta_contention with exponential backoff and jitter to handle RocksDB "Resource busy" errors (code 9003) during index creation or deletion common/doc_store/infinity_conn_base.py106-145

Sources: [rag/utils/infinity_conn.py34-88](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L34-L88) [rag/utils/infinity_conn.py111-165](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L111-L165) [common/doc_store/infinity_conn_base.py148-156](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/infinity_conn_base.py#L148-L156) [common/doc_store/infinity_conn_base.py162-173](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/infinity_conn_base.py#L162-L173)

### Elasticsearch Implementation

Elasticsearch uses a single index per tenant, using `kb_id` as a filter field for isolation.

**Key Features:**

- Hybrid Search: Combines boolean queries for text matching and KNN for vector similarity rag/utils/es_conn.py162-205
- Search After: Implements _search_with_search_after for efficient deep pagination beyond the SEARCH_AFTER_BATCH_SIZE (1,000) rag/utils/es_conn.py75-140
- Pagerank Adjustment: Uses a custom script _PAGERANK_FEA_ADJUST_SCRIPT for atomic updates to pagerank_fea during chunk feedback rag/utils/es_conn.py36-58

Sources: [rag/utils/es_conn.py31-33](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L31-L33) [rag/utils/es_conn.py75-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L75-L140) [rag/utils/es_conn.py141-161](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L141-L161)

### OpenSearch Implementation

The `OSConnection` class provides an implementation for OpenSearch, largely mirroring the Elasticsearch functionality with specialized pipeline support.

**Key Differences/Considerations:**

- Hybrid Search Pipeline: OpenSearch requires a normalization-processor on a search pipeline to merge BM25 and KNN scores. This is initialized in _init_hybrid_search rag/utils/opensearch_conn.py108-157
- Mapping Files: OpenSearch uses conf/os_mapping.json for index schemas rag/utils/opensearch_conn.py94-100
- Version Compatibility: Hybrid search requires OpenSearch 2.10+ rag/utils/opensearch_conn.py106-134

Sources: [rag/utils/opensearch_conn.py65-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L65-L101) [rag/utils/opensearch_conn.py108-157](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L108-L157)

### OceanBase Implementation

OceanBase implementation uses `pyobvector` and SQLAlchemy to provide vector search capabilities over a relational backend.

**Schema Definition:** The OceanBase implementation defines a rigid schema in `rag/utils/ob_conn.py` with columns for chunk metadata, content, and vector data.

- Vector Storage: Uses ARRAY types via pyobvector rag/utils/ob_conn.py26-27
- Full-Text Columns: content_with_weight, content_ltks, and title_tks are used for BM25 search rag/utils/ob_conn.py119-134
- Column Definitions: Specific columns like available_int, knowledge_graph_kwd, and pagerank_fea are explicitly defined with SQLAlchemy Column objects rag/utils/ob_conn.py54-102

Sources: [rag/utils/ob_conn.py54-102](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L54-L102) [rag/utils/ob_conn.py119-134](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/ob_conn.py#L119-L134)

## Match Expression System

The system uses a unified expression language to describe search intent across different backends.

**Diagram: Natural Language to Code Entity Space**

```

```

Sources: [common/doc_store/doc_store_base.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/doc_store_base.py#L25-L25) [rag/utils/es_conn.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L25-L25)

### MatchTextExpr

Used for keyword and semantic text matching. It includes parameters like `matching_text`, `fields`, and `extra_options` [common/doc_store/doc_store_base.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/doc_store_base.py#L25-L25)

### MatchDenseExpr

Used for vector similarity search. It specifies the target vector column and the query embedding data [common/doc_store/doc_store_base.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/doc_store/doc_store_base.py#L25-L25)

### FusionExpr

Enables Hybrid Search. Backends like Infinity and Elasticsearch use the parameters in `FusionExpr` to determine how to merge results from multiple search paths [rag/utils/infinity_conn.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L25-L25) [rag/utils/es_conn.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/es_conn.py#L25-L25)

## Document and Memory Storage

RAGFlow maintains document-level metadata and conversation memory in the document store.

### Document Metadata

Managed by `DocMetadataService`, document metadata is stored in indices named `ragflow_doc_meta_`. The `search` method in implementations like `InfinityConnection` handles specific logic for these tables [rag/utils/infinity_conn.py155-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/infinity_conn.py#L155-L162)

### Memory System

Conversation memory is stored in the document store using per-user indices.

- DocStore Selection: msgStoreConn in settings handles message storage persistence common/settings.py91
- Implementation: Specialized logic for memory is provided by backend-specific memory utilities like memory_es_conn and memory_infinity_conn common/settings.py44-46

Sources: [common/settings.py91](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L91-L91) [common/settings.py44-46](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L44-L46)

## Engine Selection and Initialization

The selection of the storage engine is determined during system initialization in `common/settings.py`.

**Diagram: System Initialization Flow**

```
docStoreConn
common/settings.py
OS Environment
docStoreConn
common/settings.py
OS Environment
alt
[DOC_ENGINE ==
'elasticsearch']
[DOC_ENGINE == 'infinity']
[DOC_ENGINE == 'oceanbase']
[DOC_ENGINE == 'opensearch']
Set DOC_ENGINE (e.g. 'infinity')
init_settings()
Instantiate ESConnection
Instantiate InfinityConnection
Instantiate OBConnection
Instantiate OSConnection
health() check
```

Sources: [common/settings.py85-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L85-L91) [common/settings.py246-260](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L246-L260)

### Storage Implementation Mapping

| Engine | Class Name | File Path |
| --- | --- | --- |
| Elasticsearch | `ESConnection` | `rag/utils/es_conn.py` |
| Infinity | `InfinityConnection` | `rag/utils/infinity_conn.py` |
| OceanBase | `OBConnection` | `rag/utils/ob_conn.py` |
| OpenSearch | `OSConnection` | `rag/utils/opensearch_conn.py` |

Sources: [common/settings.py29-32](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L29-L32) [rag/utils/opensearch_conn.py65](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/utils/opensearch_conn.py#L65-L65)


---



<!-- ===== 9.2-query-processing-and-hybrid-search ===== -->

# Query Processing and Hybrid Search

Relevant source files
- AGENTS.md
- CLAUDE.md
- api/db/__init__.py
- api/db/db_models.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- internal/common/json_types.go
- internal/common/json_types_test.go
- internal/common/metadata_utils.go
- internal/common/metadata_utils_test.go
- internal/handler/chat_session.go
- internal/handler/chunk.go
- internal/handler/chunk_test.go
- internal/handler/dify_retrieval_handler.go
- internal/handler/searchbot.go
- internal/handler/searchbot_test.go
- internal/handler/streaming.go
- internal/handler/streaming_test.go
- internal/service/chat_session.go
- internal/service/chat_session_test.go
- internal/service/enrich_metadata_test.go
- internal/service/metadata.go
- internal/service/metadata_filter.go
- internal/service/metadata_filter_test.go
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/prompts/meta_filter.md
- rag/raptor.py
- rag/svr/task_executor.py
- test/unit_test/common/test_apply_semi_auto_meta_data_filter.py
- web/src/components/metadata-filter/index.tsx
- web/src/components/metadata-filter/metadata-filter-conditions.tsx
- web/src/components/metadata-filter/metadata-semi-auto-fields.tsx
- web/src/components/originui/time-range-picker.tsx
- web/src/components/ui/input-select.tsx

This page documents the query processing pipeline and hybrid search mechanism that powers RAGFlow's retrieval system. It covers how user queries are preprocessed, transformed into search vectors, combined with keyword-based search, and executed against the document store.

## Overview

RAGFlow implements a hybrid search strategy that combines:

- Vector Search: Semantic similarity using embedding models via MatchDenseExpr rag/nlp/search.py61
- Full-Text Search: Keyword matching using BM25-like algorithms via MatchTextExpr rag/nlp/query.py178-180
- Fusion: Weighted combination of both search methods using FusionExpr rag/nlp/search.py25

The query processing flow includes language detection, query refinement for multi-turn conversations, keyword extraction, and adaptive search strategies managed by the `Dealer` class in `rag/nlp/search.py` [rag/nlp/search.py37-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L37-L41)

## Query Preprocessing Pipeline

Before executing search, user queries undergo several preprocessing steps to improve retrieval quality.

### Query Refinement for Multi-Turn Conversations

When users engage in multi-turn conversations, RAGFlow can optionally refine queries by incorporating context from previous turns. The `FulltextQueryer` class in `rag/nlp/query.py` handles the core logic for transforming a natural language question into a structured query expression [rag/nlp/query.py28-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L28-L40)

**Query Refinement Diagram**

```
True

False

User Query
(latest message)

Check conversation history

prompt_config
refine_multiturn?

full_question()
LLM-based refinement
rag/prompts/generator.py

Use query as-is

Query ready
for search
```

The refinement process often uses an LLM to rewrite the current query. This is managed within `DialogService` using the `full_question` prompt [api/db/services/dialog_service.py49](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L49) The `FulltextQueryer.question` method then prepares the `MatchTextExpr` containing the refined text [rag/nlp/query.py42-95](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L42-L95)

Sources: [rag/nlp/query.py28-95](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L28-L95) [api/db/services/dialog_service.py49](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L49) [rag/nlp/query.py178-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L178-L180)

### Cross-Language Query Processing

RAGFlow supports cross-language retrieval by detecting query language and optionally translating it. The `rag_tokenizer` provides utilities to handle traditional to simplified Chinese conversion and full-width to half-width character normalization [rag/nlp/query.py53-55](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L53-L55)

**Language Processing Flow**

```
Yes

No

User Query

is_chinese()
rag/nlp/query.py

Dialog
cross_language
enabled?

cross_languages()
rag/prompts/generator.py

Continue
processing
```

The `cross_languages` function in `rag/prompts/generator.py` is called by `DialogService` to normalize the question language [api/db/services/dialog_service.py49](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L49)

Sources: [rag/nlp/query.py52-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L52-L60) [api/db/services/dialog_service.py49](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L49) [rag/nlp/rag_tokenizer.py1-20](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/rag_tokenizer.py#L1-L20)

### Keyword Extraction and Term Weighting

For enhanced retrieval, RAGFlow extracts keywords and assigns weights.

- Term Weighting: Weights are calculated based on POS tags and frequency data via term_weight.Dealer rag/nlp/query.py30
- Synonyms: Looked up via synonym.Dealer and added to the query with reduced weight (e.g., w/4.0) rag/nlp/query.py31 rag/nlp/query.py73
- Extraction Logic: The FulltextQueryer splits the query and generates term weights for each token rag/nlp/query.py105-115

Sources: [rag/nlp/query.py28-32](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L28-L32) [rag/nlp/query.py67-75](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L67-L75) [rag/nlp/query.py105-115](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L105-L115)

## Hybrid Search Architecture

The core search mechanism combines vector similarity and keyword matching through a weighted fusion strategy.

### Search Components Architecture

```
Code Entity Space: Execution

Code Entity Space: Expressions

Natural Language Space

User Question Text

tokenize()
rag_tokenizer.py

Keywords & Weights
rag/nlp/query.py

MatchTextExpr
BM25 keyword search
fields: content_ltks, title_tks

MatchDenseExpr
Vector similarity search
metric: cosine

FusionExpr
type: weighted_sum

Dealer.search()
rag/nlp/search.py

DocStoreConnection.search()
common/doc_store/doc_store_base.py
```

Sources: [rag/nlp/search.py37-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L37-L61) [rag/nlp/query.py28-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L28-L40) [rag/nlp/search.py132-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L132-L180)

### Vector Search Component

The vector search component uses embedding vectors for semantic matching.

- Metric: Cosine similarity is the standard metric rag/nlp/search.py61
- Column Selection: The system identifies the appropriate vector column based on dimension, e.g., q_{len}_vec rag/nlp/search.py60
- Encoding: Embedding models are invoked via emb_mdl.encode_queries rag/nlp/search.py54

Sources: [rag/nlp/search.py53-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L53-L61)

### Full-Text Search Component

The full-text search targets multiple fields with specific boosts defined in `FulltextQueryer` [rag/nlp/query.py32-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L32-L40):

- title_tks^10
- important_kwd^30
- question_tks^20
- content_ltks^2

Sources: [rag/nlp/query.py32-40](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L32-L40) [rag/nlp/search.py173-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L173-L180)

### Fusion Strategy

The `Dealer` class implements hybrid fusion by combining `MatchTextExpr` and `MatchDenseExpr` with a `FusionExpr`.

- Fusion Execution: The system creates a FusionExpr and passes it to the data store's search method rag/nlp/search.py25

Sources: [rag/nlp/search.py25](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L25-L25) [rag/nlp/search.py175-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L175-L180)

## Metadata Filtering

RAGFlow supports complex metadata filtering during retrieval. The `Dealer.get_filters` method maps request parameters to database fields [rag/nlp/search.py120-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L120-L130)

| Parameter | Database Field | Description |
| --- | --- | --- |
| `kb_ids` | `kb_id` | Knowledge base identifier |
| `doc_ids` | `doc_id` | Document identifier |
| `available_int` | `available_int` | Status filter (active/inactive) |
| `knowledge_graph_kwd` | `knowledge_graph_kwd` | KG specific filter |
| `entity_kwd` | `entity_kwd` | Entity filter |
| `from_entity_kwd` | `from_entity_kwd` | Filter for entities in a "from" relationship |
| `to_entity_kwd` | `to_entity_kwd` | Filter for entities in a "to" relationship |
| `removed_kwd` | `removed_kwd` | Filter for removed keywords |

The `apply_meta_data_filter` function handles the logic for applying these rules, often used within `DialogService` to restrict search scope [api/db/services/dialog_service.py37](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L37-L37)

Sources: [rag/nlp/search.py120-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L120-L130) [api/db/services/dialog_service.py37](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L37-L37)

## Search Execution Flow

The search process is orchestrated by the `Dealer.search` method, which handles pagination, field selection, and result pruning.

```
DocStore (ES/Infinity)
Dealer
DialogService
DocStore (ES/Infinity)
Dealer
DialogService
alt
[is Keyword Search]
[is Hybrid Search]
search(req, idx_names, kb_ids, emb_mdl)
get_filters(req)
qryr.question(qst)
get_vector(qst, emb_mdl)
FusionExpr("weighted_sum")
search(src, highlightFields, filters, matchExprs, ...)
results (ids, fields, highlight)
_prune_deleted_chunks(sres)
SearchResult
```

Sources: [rag/nlp/search.py77-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L77-L118) [rag/nlp/search.py132-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L132-L180)

## RAPTOR and GraphRAG Integration

Query processing also interacts with advanced retrieval structures:

- RAPTOR: The task_executor.py manages the recursive processing of tree-organized retrieval via the RecursiveAbstractiveProcessing4TreeOrganizedRetrieval class rag/svr/task_executor.py88-90 rag/raptor.py156-186 It supports clustering methods like GMM and AHC rag/raptor.py38-45
- GraphRAG: The system can execute searches against knowledge graphs, utilizing fields like knowledge_graph_kwd and entity-based filters in the Dealer rag/nlp/search.py126-127 Metadata extraction for GraphRAG is handled during task execution rag/svr/task_executor.py58-60

Sources: [rag/svr/task_executor.py88-90](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L88-L90) [rag/raptor.py156-186](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/raptor.py#L156-L186) [rag/nlp/search.py126-127](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L126-L127) [rag/svr/task_executor.py58-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L58-L60)


---



<!-- ===== 9.3-reranking-and-filtering ===== -->

# Reranking and Filtering

Relevant source files
- api/apps/llm_app.py
- api/db/__init__.py
- api/db/db_models.py
- api/db/init_data.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/llm_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- conf/llm_factories.json
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/task_executor.py
- web/src/pages/user-setting/setting-model/constant.ts

This page documents RAGFlow's two-tier reranking system and filtering mechanisms that refine search results before they are presented to the LLM for answer generation. The system combines similarity-based scoring, optional model-based reranking, rank feature scoring (tags, PageRank), and threshold filtering to optimize chunk selection.

For information about the initial search and retrieval process, see **9.2 Query Processing and Hybrid Search**. For the subsequent step of formatting retrieved chunks into context, see **9.4 Response Generation and Citations**.

## System Overview

The reranking and filtering system operates as a post-processing layer after hybrid search retrieval. It receives initial search results from the document store (Elasticsearch, Infinity, or OpenSearch) and applies additional scoring and filtering logic. The core logic resides in the `Dealer` class within `rag/nlp/search.py` [rag/nlp/search.py37-41](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L37-L41)

**High-Level Reranking Pipeline**

```
Output Result

Filtering Logic

Scoring Logic

Dealer.search() Execution

Input Data

Yes

No, ES

No, Infinity

SearchResult
(from hybrid search)

User Query

Rerank Configuration
similarity_threshold
vector_similarity_weight
rerank_mdl

Dealer.search()
Hybrid search
(vector + BM25)

rerank_mdl
provided?

Dealer.rerank_by_model()
LLM-based reranking

Dealer.rerank()
Similarity-based reranking

Use Infinity fusion scores
(pre-normalized)

_rank_feature_scores()
TAG_FLD + PAGERANK_FLD

hybrid_similarity()
token + vector weighted

Final Scores
(sim + rank_fea)

Apply similarity_threshold

Filter available_int chunks

Select top N results

Ranked Chunks
(sorted by score)
```

Sources: [rag/nlp/search.py132-167](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L132-L167) [api/db/services/dialog_service.py176-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L176-L181)

## Similarity-Based Reranking

The `Dealer.rerank()` method implements similarity-based reranking by computing hybrid similarity scores that combine token-based (BM25-style) and vector-based similarity [rag/nlp/search.py296-300](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L296-L300) This is the default behavior when no external reranking model is specified.

### Implementation

The reranking process extracts searchable tokens from each chunk, including content, title, important keywords, and question keywords. These are then scored against the user query using both token overlap and vector cosine similarity.

**Similarity-Based Reranking Data Flow**

```
Output

Rank Features

Scoring (qryr.hybrid_similarity)

Vector Preparation

Token Extraction

Input (Dealer.rerank)

sres: SearchResult
ids, fields, query_vector

query: str

tkweight, vtweight
(default: 0.3, 0.7)

Extract tokens for each chunk:
content_ltks
title_tks x2
important_kwd x5
question_tks x6

query.FulltextQueryer.question()
Extract query keywords

Extract q_N_vec from fields
(or use zero vector if missing)

token_similarity()
Jaccard similarity
between query & chunk tokens

vector_similarity()
Cosine similarity
between query_vector & chunk vectors

sim = tkweight * tksim
+ vtweight * vtsim

_rank_feature_scores()
TAG_FLD cosine sim
+ PAGERANK_FLD

final_sim = sim + rank_fea

return (sim, tksim, vtsim)
```

Sources: [rag/nlp/search.py296-333](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L296-L333) [rag/nlp/query.py23-25](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L23-L25) [rag/nlp/query.py41-43](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/query.py#L41-L43)

### Token Weighting Strategy

The system applies differential weighting to different token types to reflect their semantic importance:

| Token Source | Weight Multiplier | Field Name | Purpose |
| --- | --- | --- | --- |
| Content tokens | 1x | `content_ltks` | Base content matching |
| Title tokens | 2x | `title_tks` | Document title relevance |
| Important keywords | 5x | `important_kwd` | LLM-extracted key concepts |
| Question tokens | 6x | `question_tks` | Generated questions for QA matching |

This weighting scheme prioritizes matches against LLM-generated metadata (questions, keywords) over raw content matches, improving semantic relevance [rag/nlp/search.py318-323](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L318-L323)

Sources: [rag/nlp/search.py318-323](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L318-L323)

### Vector Dimension Handling

The reranking logic handles cases where chunk vectors have different dimensions than the query vector (e.g., when embedding models have changed). Mismatched chunks receive zero vectors to prevent crashes [rag/nlp/search.py310-315](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L310-L315)

Sources: [rag/nlp/search.py310-315](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L310-L315)

## Model-Based Reranking

When a reranking model is configured via `rerank_id` in the `Dialog` model, the system uses `Dealer.rerank_by_model()` to compute LLM-based relevance scores [rag/nlp/search.py335-340](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L335-L340) This provides more sophisticated semantic understanding than pure similarity metrics.

### Reranker Model Interface

The reranking model is passed as an `LLMBundle` instance [api/db/services/llm_service.py85-86](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/llm_service.py#L85-L86) configured with `LLMType.RERANK`. The model's `similarity()` method [rag/llm/rerank_model.py34-35](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L34-L35) computes relevance scores between the query and each chunk's text. Concrete implementations like `JinaRerank` [rag/llm/rerank_model.py94-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L94-L114) or `XInferenceRerank` [rag/llm/rerank_model.py117-146](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L117-L146) wrap external API calls.

**Model-Based Reranking Architecture**

```
Output

Combination

Dual Scoring

Token Processing

Input (rerank_by_model)

rerank_mdl: LLMBundle
(LLMType.RERANK)

sres: SearchResult

query: str

tkweight, vtweight

Extract tokens:
content_ltks
title_tks
important_kwd

Concatenate tokens
remove_redundant_spaces()

token_similarity()
(traditional BM25-style)

rerank_mdl.similarity()
(LLM-based relevance)

tkweight * tksim
+ vtweight * vtsim

Unsupported markdown: list

(final_scores, tksim, vtsim)
```

Sources: [rag/nlp/search.py335-356](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L335-L356) [api/db/services/dialog_service.py181-183](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L181-L183) [rag/llm/rerank_model.py34-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/rerank_model.py#L34-L114)

## Rank Feature Scoring

The `_rank_feature_scores()` method adds domain-specific ranking signals beyond pure text similarity. It computes scores based on tag matching and PageRank values [rag/nlp/search.py269-272](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L269-L272)

### Tag Feature Scoring

Tag-based scoring performs cosine similarity between the query's tag features and each chunk's tags. Tags are stored in the `TAG_FLD` field [rag/nlp/search.py28](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L28-L28)

**Tag Feature Scoring Algorithm**

```
Combination

Per-Chunk Scoring

Query Normalization

Input

query_rfea: dict
{tag: score, ...}

chunk TAG_FLD
JSON-encoded dict

chunk PAGERANK_FLD

q_denor = sqrt(sum(score^2))
(excluding PAGERANK_FLD)

eval(chunk[TAG_FLD])

nor = sum(query[tag] * chunk[tag])
for matching tags

denor = sqrt(sum(chunk_score^2))

score = nor / (sqrt(denor) * q_denor)

tag_score * 10.0

Unsupported markdown: list

final_rank_score
```

Sources: [rag/nlp/search.py269-294](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L269-L294)

### PageRank Integration

PageRank values (`PAGERANK_FLD`) provide document-level importance signals independent of query content. These are added directly to the rank feature score [rag/nlp/search.py294](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L294-L294)

Sources: [rag/nlp/search.py272-294](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L272-L294) [common/constants.py103](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/constants.py#L103-L103)

## Threshold Filtering

The retrieval pipeline applies multiple filtering stages to remove low-quality or irrelevant chunks.

### Similarity Threshold

The `similarity_threshold` parameter filters chunks based on their final similarity scores. This is configured per-dialog and defaults to 0.2 [api/db/services/dialog_service.py178](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L178-L178)

**Threshold Application in Retrieval**

```
Output

Filtering Stages

Reranking

Initial Search

Hybrid search with
similarity parameter

Compute final scores
(sim + rank_fea)

Sort by score descending

Filter: score >= threshold

Select top K chunks

Filter: available_int == 1

Filtered & ranked chunks
```

Sources: [rag/nlp/search.py120-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L120-L130) [api/db/services/dialog_service.py176-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L176-L180)

### Availability Filtering

Chunks can be marked as unavailable using the `available_int` field. This allows manual or automated curation of chunk visibility without deleting them [rag/nlp/search.py125-129](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L125-L129)

```
# From search.py:150-153
src = req.get("fields",
              ["docnm_kwd", "content_ltks", "kb_id", "img_id", "title_tks", "important_kwd", "position_int",
               "doc_id", "chunk_order_int", "page_num_int", "top_int", "create_timestamp_flt", "knowledge_graph_kwd",
               "question_kwd", "question_tks", "doc_type_kwd",
               "available_int", "content_with_weight", "mom_id", PAGERANK_FLD, TAG_FLD, "row_id()"])
```

Sources: [rag/nlp/search.py150-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L150-L153)

## Integration in Retrieval Pipeline

The complete retrieval flow integrates reranking and filtering as a unified pipeline executed by `Dealer.search()` [rag/nlp/search.py132-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L132-L137)

### Rerank Limit and Pagination

The retrieval logic uses pagination parameters to control the number of candidates retrieved for reranking [rag/nlp/search.py144-147](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L144-L147)

```
# From search.py:144-147
pg = int(req.get("page", 1)) - 1
topk = int(req.get("topk", 1024))
ps = int(req.get("size", topk))
offset, limit = pg * ps, ps
```

Sources: [rag/nlp/search.py144-147](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L144-L147)

### Backend-Specific Reranking Logic

The system adapts its reranking strategy based on the document store backend (Elasticsearch vs. Infinity). Infinity pre-normalizes fusion scores, while Elasticsearch requires manual normalization via the `rerank()` method [rag/nlp/search.py174-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L174-L180)

**Backend-Specific Reranking Logic**

```
Output

Score Processing

Reranking Paths

Configuration Check

Yes

No

True

False

rerank_mdl
provided?

settings.DOC_ENGINE_INFINITY

Use rerank_by_model()
(works for both backends)

Use Infinity fusion scores
(pre-normalized)

Use rerank()
(ElasticSearch needs normalization)

Extract _score from sres.field

Normalize and combine
token + vector scores

Chunks sorted by final score
```

Sources: [rag/nlp/search.py132-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L132-L180) [common/settings.py31](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/settings.py#L31-L31)


---



<!-- ===== 9.4-response-generation-and-citations ===== -->

# Response Generation and Citations

Relevant source files
- api/db/__init__.py
- api/db/db_models.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/raptor.py
- rag/svr/task_executor.py
- web/src/components/message-input/next.tsx
- web/src/components/message-item/group-button.tsx
- web/src/components/message-item/hooks.ts
- web/src/components/message-item/index.tsx
- web/src/components/next-message-item/index.tsx
- web/src/hooks/logic-hooks.ts
- web/src/hooks/use-send-message.ts
- web/src/interfaces/database/chat.ts
- web/src/pages/agent/chat/box.tsx
- web/src/pages/agent/chat/chat-sheet.tsx
- web/src/pages/agent/chat/use-send-agent-message.ts
- web/src/pages/next-chats/hooks/use-send-shared-message.ts
- web/src/pages/next-chats/share/index.tsx
- web/src/utils/chat.ts

## Purpose and Scope

This document describes RAGFlow's response generation and citation system, which formats retrieved document chunks into prompts, streams LLM responses, and inserts inline citations linking answers back to source documents. The system handles context assembly, citation marker insertion, reference tracking, and empty response handling.

## System Overview

The response generation pipeline coordinates multiple components to produce cited answers from retrieved knowledge. It bridges the gap between raw document chunks and a coherent, attributed response.

### Response Generation Data Flow

```
Output

ReferenceAssembly

CitationProcessing

LLMGeneration

ContextAssembly

InputProcessing

User Query
+ Conversation History

Retrieved Chunks
from Dealer.search()

PromptConfig
Dialog.prompt_config

chunks_format()
Format chunks with [ID]

kb_prompt()
Build context prompt

message_fit_in()
Truncate to token limit

LLMBundle.chat()
Unified Model Access

SSE Streaming
via Response()

citation_prompt()
Instruct LLM to cite

MarkdownContent
Frontend Citation Rendering

DocMetadataService
Fetch flattened metadata

DocumentService
Get doc details

Reference List
chunks + metadata

Streamed Answer
with inline citations

MessageItem
React Component
```

**Sources:**

- DialogService coordination: api/db/services/dialog_service.py138-140
- Prompt generation functions: api/db/services/dialog_service.py49-50
- Search logic integration: rag/nlp/search.py132-137
- Dialog model fields: api/db/services/dialog_service.py177-184

## Context Formatting and Token Management

The system uses `kb_prompt()` to assemble retrieved chunks into a structured prompt while respecting LLM token limits.

### Chunk Formatting Implementation

The `chunks_format()` function standardizes retrieved chunks into a format containing content, document IDs, and similarity scores [api/db/services/dialog_service.py49-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L50) In the `DialogService`, these chunks are processed to include metadata like `kb_id` for multi-dataset queries via `_chunk_kb_id_for_doc` [api/db/services/dialog_service.py56-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L56-L60)

### Citation Vector Hydration

For backends like Elasticsearch where vectors are not fetched during the primary search to save bandwidth, `_hydrate_chunk_vectors` is called [api/db/services/dialog_service.py62-107](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L62-L107) This function pulls vectors for just the candidate chunks right before computing answer-vs-chunk similarity for citation accuracy [api/db/services/dialog_service.py62-70](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L62-L70)

### Token Truncation

To prevent context overflow, `message_fit_in()` calculates the token count for each message in the conversation history and truncates content to ensure the total fits within the model's limit [api/db/services/dialog_service.py49-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L50) It uses `num_tokens_from_string` to estimate token usage [rag/svr/task_executor.py90](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L90-L90)

**Sources:**

- Prompt generation logic: api/db/services/dialog_service.py49-50
- Vector hydration: api/db/services/dialog_service.py62-107
- Token management: rag/svr/task_executor.py90

## Citation Mechanism and UI Rendering

RAGFlow implements a citation system that bridges backend LLM outputs with frontend document previews.

### Citation Generation

The `citation_prompt()` function provides the LLM with instructions on how to cite sources, typically using markers corresponding to the formatted context [api/db/services/dialog_service.py49-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L50) The `kb_prompt()` function constructs the context by assigning numeric IDs to chunks, which the LLM uses for referencing.

### Search Result Pruning

To ensure citations never point to deleted documents, the `Dealer` class implements `_prune_deleted_chunks` [rag/nlp/search.py76-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L76-L118) This method checks the database for the existence of `doc_id`s returned by the search engine and removes chunks belonging to documents that have been deleted from the DB but not yet purged from the vector store [rag/nlp/search.py82-108](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L82-L108)

#### Citation Generation and Pruning Components

```
uses for context

checks doc existence

DialogService

+citation_prompt()

+kb_prompt()

Dealer

+SearchResult

+_prune_deleted_chunks()

DocumentService

+get_by_ids()
```

**Sources:**

- Citation prompt definition: api/db/services/dialog_service.py49-50
- Search pruning: rag/nlp/search.py76-118
- DocumentService.get_by_ids: rag/nlp/search.py72-73

## Metadata Enrichment

Citations are supported by detailed document metadata retrieved via the `DocMetadataService`.

### Metadata Flow

1. Retrieval: Chunks are retrieved with fields like doc_id, kb_id, and img_id using the Dealer class rag/nlp/search.py149-153
2. Metadata Fetching: DocMetadataService.get_metadata_for_documents is used to fetch metadata for the documents appearing on the current page api/db/services/document_service.py107
3. Enrichment: The enrich_chunks_with_document_metadata function maps these metadata fields back to chunks to provide context for citations api/db/services/dialog_service.py38-41

#### Metadata Enrichment Process

```
Retrieved Chunks

Dealer.search()

DocMetadataService.get_metadata_for_documents()

enrich_chunks_with_document_metadata()

Enriched Chunks with Metadata
```

**Sources:**

- Search service fields: rag/nlp/search.py149-153
- Document metadata mapping: api/db/services/document_service.py107-109
- Reference enrichment: api/db/services/dialog_service.py38-41

## Empty Response and Error Handling

RAGFlow manages scenarios where retrieval fails or tasks are incomplete.

### Empty Response Handling

The `ASK_SUMMARY` prompt is used as a fallback for summarization tasks when standard retrieval context is insufficient [api/db/services/dialog_service.py49-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L49-L50)

### Task and Document Status

The system monitors document health and task progress to prevent generation from stale or incomplete data. If a task exceeds its retry limit (3 attempts), it is abandoned [api/db/services/task_service.py130-141](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L130-L141)

| Scenario | Handling Mechanism | Code Site |
| --- | --- | --- |
| **Task Abandoned** | Progress set to -1 after 3 retries | [api/db/services/task_service.py130-141](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L130-L141) |
| **Stale Chunks** | `_prune_deleted_chunks` in Dealer | [rag/nlp/search.py76-118](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/nlp/search.py#L76-L118) |
| **Web Search** | `_should_use_web_search` toggle | [api/db/services/dialog_service.py122-126](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L122-L126) |
| **Parsing Error** | `set_progress` with negative value | [rag/svr/task_executor.py165-181](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L181) |

**Sources:**

- Prompt fallbacks: api/db/services/dialog_service.py49-50
- Task retry logic: api/db/services/task_service.py130-141
- Stale data pruning: rag/nlp/search.py76-118
- Progress messaging: rag/svr/task_executor.py165-181


---



<!-- ===== 10-memory-system ===== -->

# Memory System

Relevant source files
- api/apps/restful_apis/memory_api.py
- api/apps/services/memory_api_service.py
- api/db/joint_services/memory_message_service.py
- api/db/services/langfuse_service.py
- api/db/services/pipeline_operation_log_service.py
- api/db/services/search_service.py
- conf/mapping.json
- memory/services/messages.py
- memory/utils/es_conn.py
- memory/utils/infinity_conn.py
- web/src/pages/dataset/dataset/reparse-dialog.tsx
- web/src/pages/dataset/process-log-modal.tsx
- web/src/pages/memories/add-or-edit-modal.tsx
- web/src/pages/memories/constants/index.tsx
- web/src/pages/memories/hooks.ts
- web/src/pages/memories/interface.ts
- web/src/pages/memory/memory-message/hook.ts
- web/src/pages/memory/memory-message/index.tsx
- web/src/pages/memory/memory-message/interface.ts
- web/src/pages/memory/memory-message/message-table.tsx
- web/src/pages/memory/memory-setting/advanced-settings-form.tsx
- web/src/pages/memory/memory-setting/hook.ts
- web/src/pages/memory/memory-setting/index.tsx
- web/src/pages/memory/memory-setting/memory-model-form.tsx
- web/src/routes.tsx
- web/src/services/memory-service.ts
- web/src/utils/authorization-util.ts
- web/src/wrappers/auth.tsx

The **Memory System** in RAGFlow provides a persistent, searchable storage layer for agentic workflows. Unlike standard conversation history, which is often ephemeral or limited by context window sizes, the Memory System treats dialogue and extracted insights as structured data. It allows agents to maintain long-term state, recall past interactions across sessions, and organize information into different cognitive categories (e.g., raw dialogue vs. semantic facts).

### System Overview

The memory subsystem bridges the gap between the LLM's "Natural Language Space" and the "Code Entity Space" by mapping conversation turns into vector-searchable documents stored in specialized indices.

Title: Memory System Architecture

```
Storage Layer

Code Entity Space

Natural Language Space

User Interaction

LLM Extraction (extract_by_llm)

Memory Types: raw, semantic, episodic, procedural

MessageService (memory/services/messages.py)

DocStore Connection (settings.msgStoreConn)

Per-User Indices: memory_{uid}

InfinityConnection / ESConnection

Infinity (memory/utils/infinity_conn.py) / ES (memory/utils/es_conn.py)
```

**Sources:** [memory/services/messages.py24-32](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L24-L32) [api/db/joint_services/memory_message_service.py39-63](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L39-L63) [memory/utils/es_conn.py36-37](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L36-L37)

### Memory Types and Storage

RAGFlow categorizes memory into four distinct types to optimize how agents retrieve information:

1. Raw: The literal transcript of the conversation api/db/joint_services/memory_message_service.py64-76
2. Semantic: General knowledge and facts extracted from interactions api/db/joint_services/memory_message_service.py58 common/constants.py22
3. Episodic: Records of specific events and experiences api/db/joint_services/memory_message_service.py58 common/constants.py22
4. Procedural: Learned habits and procedures api/db/joint_services/memory_message_service.py58 common/constants.py22

When a message is processed via `save_to_memory`, the system uses an LLM to extract content unless the memory type is strictly `RAW` [api/db/joint_services/memory_message_service.py39-63](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L39-L63) Messages are stored in the document store using per-user indices named via `index_name(uid)` which adds a `memory_` prefix [memory/services/messages.py24-30](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L24-L30) The `MessageService` acts as the primary interface for CRUD operations on these messages, abstracting the underlying `msgStoreConn` [memory/services/messages.py32-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L32-L69)

For a deep dive into the underlying storage schemas and the extraction pipeline, see **[Memory Types and Storage](/infiniflow/ragflow/10.1-memory-types-and-storage)**.

**Sources:** [api/db/joint_services/memory_message_service.py39-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L39-L91) [memory/services/messages.py24-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L24-L69) [memory/utils/es_conn.py53-78](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L53-L78) [common/constants.py22](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/constants.py#L22-L22)

### Memory Management UI and API

The system provides a REST API and a React-based frontend for managing these persistent states. Users can configure memory parameters, such as forgetting policies and storage types, through the UI.

#### Core Management Flow

Title: Memory Configuration and Storage Flow

```
"DocStore (Infinity/ES)"
"MessageService (memory/services/messages.py)"
"MemoryApiService (api/apps/services/memory_api_service.py)"
"Memory API (api/apps/restful_apis/memory_api.py)"
"Frontend UI (web/src/pages/memory/)"
"DocStore (Infinity/ES)"
"MessageService (memory/services/messages.py)"
"MemoryApiService (api/apps/services/memory_api_service.py)"
"Memory API (api/apps/restful_apis/memory_api.py)"
"Frontend UI (web/src/pages/memory/)"
POST /memories (create_memory)
create_memory(memory_info)
create_idx(memory_{uid})
GET /memories/<id> (get_memory_messages)
list_message(uid, memory_id)
search(settings.msgStoreConn)
Success (message_list)
```

Key management features include:

- Memory CRUD: Endpoints for creating, updating, listing, and deleting memories in memory_api.py api/apps/restful_apis/memory_api.py29-120
- Forgetting Policies: Support for managing memory lifecycle via parameters like forgetting_policy (e.g., FIFO) and memory_size api/apps/restful_apis/memory_api.py87-90 api/apps/services/memory_api_service.py124-182
- Message Inspection: Retrieving detailed message history for a specific memory via get_memory_messages, including filtering by agent_ids or keywords api/apps/restful_apis/memory_api.py153-175
- Advanced Configuration: Tuning parameters like temperature for extraction and defining system_prompt or user_prompt for the extraction LLM api/apps/restful_apis/memory_api.py88-89 web/src/pages/memory/memory-setting/advanced-settings-form.tsx12-27
- Permissions: Granular control to restrict memory access to "team" or private use via TenantPermission api/apps/services/memory_api_service.py53-58 api/apps/restful_apis/memory_api.py88

For details on the API endpoints and frontend configuration, see **[Memory Management UI and API](/infiniflow/ragflow/10.2-memory-management-ui-and-api)**.

**Sources:** [api/apps/restful_apis/memory_api.py29-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/memory_api.py#L29-L175) [api/apps/services/memory_api_service.py75-182](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/memory_api_service.py#L75-L182) [memory/services/messages.py71-123](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L71-L123) [web/src/pages/memory/memory-setting/advanced-settings-form.tsx12-27](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/advanced-settings-form.tsx#L12-L27)

### Integration in Workflows

The Memory System is integrated into agent workflows to provide long-term context:

- Automated Extraction: The extract_by_llm function uses the PromptAssembler to generate prompts that guide the LLM in identifying semantic or episodic information from raw dialogues api/db/joint_services/memory_message_service.py145-157
- Hybrid Retrieval: Agents retrieve memories using search_message, which supports both keyword filtering and vector similarity via match_expressions memory/services/messages.py155-175
- Embedding Support: All extracted memories are vectorized before storage using the tenant's configured embedding model to enable semantic search via embed_and_save api/db/joint_services/memory_message_service.py174-196
- Search Optimization: The ESConnection in the memory subsystem handles field mapping specifically for messages, such as converting message_type to message_type_kwd and mapping content to content_ltks memory/utils/es_conn.py38-51
- Storage Mapping: The system uses dynamic templates in conf/mapping.json to ensure memory fields like *_ltks (text) and *_vec (vectors) are correctly indexed in the document store conf/mapping.json83-201

**Sources:** [api/db/joint_services/memory_message_service.py145-196](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L145-L196) [memory/services/messages.py155-175](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L155-L175) [memory/utils/es_conn.py38-78](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L38-L78) [conf/mapping.json83-201](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/mapping.json#L83-L201)


---



<!-- ===== 10.1-memory-types-and-storage ===== -->

# Memory Types and Storage

Relevant source files
- api/apps/restful_apis/memory_api.py
- api/apps/services/memory_api_service.py
- api/db/joint_services/memory_message_service.py
- api/db/services/langfuse_service.py
- api/db/services/pipeline_operation_log_service.py
- api/db/services/search_service.py
- conf/mapping.json
- internal/dao/memory.go
- internal/dao/user_tenant.go
- internal/engine/elasticsearch/chunk.go
- internal/engine/elasticsearch/chunk_helpers_test.go
- internal/engine/elasticsearch/chunk_test.go
- internal/engine/elasticsearch/kg_test.go
- internal/engine/infinity/chunk.go
- internal/engine/types/types.go
- internal/handler/memory.go
- internal/handler/memory_message_test.go
- internal/service/memory.go
- internal/service/memory_message_test.go
- internal/service/nlp/query_builder_test.go
- internal/service/nlp/reranker.go
- internal/service/nlp/reranker_normalize_test.go
- internal/service/nlp/retrieval.go
- memory/services/messages.py
- memory/utils/es_conn.py
- memory/utils/infinity_conn.py
- web/src/routes.tsx
- web/src/utils/authorization-util.ts
- web/src/wrappers/auth.tsx

This page documents the persistent memory subsystem in RAGFlow. The memory system provides agents with long-term context by categorizing interaction data into specific types, extracting structured information using LLMs, and storing them in specialized document store indices with per-user isolation.

## Memory Types

RAGFlow implements four distinct memory types to handle different aspects of agent-user interactions. While "Raw" memory is a mandatory baseline, the other three types use LLMs to analyze and structure dialogue for better retrieval.

| Memory Type | Description | Implementation Detail |
| --- | --- | --- |
| **Raw** | The literal transcript of the conversation between the user and the agent. | Stored as baseline dialogue content. Extraction is skipped if only RAW is selected [api/db/joint_services/memory_message_service.py63](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L63-L63) |
| **Semantic** | General knowledge and facts about the user and the world. | Extracted to represent abstract concepts and persistent facts [api/db/joint_services/memory_message_service.py55-62](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L55-L62) |
| **Episodic** | Time-stamped records of specific events and experiences. | Captures unique interactions with specific temporal context [api/db/joint_services/memory_message_service.py55-62](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L55-L62) |
| **Procedural** | Learned skills, habits, and automated procedures. | Stores behavioral patterns or specific instruction-following logic [api/db/joint_services/memory_message_service.py55-62](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L55-L62) |

**Sources:** [api/db/joint_services/memory_message_service.py39-63](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L39-L63) [common/constants.py22](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/constants.py#L22-L22)

## Data Flow and Storage Architecture

RAGFlow uses a polyglot persistence strategy. Metadata such as configurations and memory definitions are managed via the database service layer (MySQL/PostgreSQL) using `MemoryService` [api/db/services/memory_service.py28](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/memory_service.py#L28-L28) while high-volume message content and vectors are handled by specialized document store connectors via `MessageService` [memory/services/messages.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L32-L32)

### Per-User Indexing and Isolation

Messages are stored in indices partitioned by user ID to ensure data isolation. The naming convention for these indices is `memory_{prefix}_{uid}` if a prefix is configured, otherwise `memory_{uid}` [memory/services/messages.py27-29](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L27-L29) Within these indices, operations are scoped by `memory_id` [memory/services/messages.py37](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L37-L37)

### Message Storage Sequence

The following diagram illustrates the flow from a user interaction to persistent storage, including the LLM extraction step for non-raw memory types.

**Sequence: Message Extraction and Persistence**

```
"common.doc_store"
"memory.services.messages.MessageService"
"api.db.services.llm_service.LLMBundle"
"api.db.joint_services.memory_message_service"
"AgentFlow"
"common.doc_store"
"memory.services.messages.MessageService"
"api.db.services.llm_service.LLMBundle"
"api.db.joint_services.memory_message_service"
"AgentFlow"
Check MemoryType
Skip extraction
alt
["Type != RAW"]
["Type == RAW"]
"save_to_memory(memory_id, message_dict)"
"extract_by_llm()"
"chat(system_prompt, user_prompt)"
"extracted_json"
"embed_and_save()"
"insert_message(message_list, uid, memory_id)"
"settings.msgStoreConn.insert()"
```

**Sources:** [api/db/joint_services/memory_message_service.py39-92](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L39-L92) [memory/services/messages.py50-56](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L50-L56)

### Code Entity Mapping

This diagram bridges the conceptual memory space with the internal Python classes and storage connectors.

**Memory Entity Mapping**

```
Code_Entity_Space

Natural_Language_Space

save_to_memory

settings.msgStoreConn

settings.msgStoreConn

settings.msgStoreConn

User Conversation

Agent Long-term Memory

api.db.joint_services.memory_message_service

memory.services.messages.MessageService

memory.utils.es_conn.ESConnection

memory.utils.infinity_conn.InfinityConnection

rag.utils.ob_conn.OBConnection
```

**Sources:** [api/db/joint_services/memory_message_service.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/joint_services/memory_message_service.py#L32-L32) [memory/services/messages.py32](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L32-L32) [memory/utils/es_conn.py36](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L36-L36) [memory/utils/infinity_conn.py31](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/infinity_conn.py#L31-L31)

## The MessageService Interface

The `MessageService` is the core interface for memory content operations. It abstracts the underlying storage engine (Elasticsearch, Infinity, or OceanBase) to provide a unified API.

### Key Interface Methods

| Method | Description | Reference |
| --- | --- | --- |
| `create_index` | Creates a new index for a user's memory with specified vector size. | [memory/services/messages.py40-42](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L40-L42) |
| `insert_message` | Formats and persists messages into the doc store. | [memory/services/messages.py50-56](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L50-L56) |
| `list_message` | Retrieves messages with support for keywords, agent filtering, and pagination. | [memory/services/messages.py71-123](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L71-L123) |
| `search_message` | Performs hybrid search (vector + scalar filters) to recall relevant memories. | [memory/services/messages.py155-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L155-L180) |
| `update_message` | Updates message status or content. | [memory/services/messages.py59-63](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L59-L63) |
| `delete_message` | Deletes messages based on a condition. | [memory/services/messages.py66-68](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L66-L68) |

**Sources:** [memory/services/messages.py32-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/services/messages.py#L32-L180)

## Storage Schema and Configuration

Memory entries are stored as documents in the chosen `DocStore`. Each document contains the raw or extracted text, its vector representation, and operational metadata.

### Common Storage Fields

The system maps internal message dictionaries to storage-specific schemas via `map_message_to_es_fields` [memory/utils/es_conn.py53](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L53-L53)

| Field Name | Type | Description |
| --- | --- | --- |
| `message_id` | String | Unique identifier for the message [memory/utils/es_conn.py62](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L62-L62) |
| `message_type_kwd` | String | Type of memory (raw, semantic, etc.) [memory/utils/es_conn.py63](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L63-L63) |
| `content_ltks` | Text | The text content, tokenized for full-text search [memory/utils/es_conn.py74](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L74-L74) |
| `tokenized_content_ltks` | Text | Fine-grained tokenized content [memory/utils/es_conn.py75](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L75-L75) |
| `q_{n}_vec` | Vector | Embedding vector of size `n` [memory/utils/es_conn.py76](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L76-L76) |
| `status_int` | Integer | Active (1) or Inactive (0) status [memory/utils/es_conn.py72](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L72-L72) |
| `valid_at` | Date | ISO 8601 formatted timestamp [memory/utils/es_conn.py69](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L69-L69) |

**Sources:** [memory/utils/es_conn.py53-78](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L53-L78) [conf/mapping.json17-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/conf/mapping.json#L17-L210)

### Indexing and Retrieval Strategies

- Elasticsearch: Uses Q("bool") queries with must_not filters for forget_at to implement forgetting logic memory/utils/es_conn.py140-142 It supports weighted_sum fusion for hybrid search memory/utils/es_conn.py161-167
- Infinity: Implements table-per-user-per-memory naming logic (e.g., memory_{indexName}_{mem_id}) memory/utils/infinity_conn.py166 It maps content to ft_content_rag_fine for full-text search memory/utils/infinity_conn.py79
- Go Engine (Elasticsearch): Provides a native implementation for index creation with CreateChunkStore internal/engine/elasticsearch/chunk.go56 and document persistence with InsertChunks internal/engine/elasticsearch/chunk.go138 supporting bulk upsert behavior internal/engine/elasticsearch/chunk.go149-165

**Sources:** [memory/utils/es_conn.py113-167](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/es_conn.py#L113-L167) [memory/utils/infinity_conn.py103-183](https://github.com/infiniflow/ragflow/blob/d32e05d5/memory/utils/infinity_conn.py#L103-L183) [internal/engine/elasticsearch/chunk.go56-180](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/engine/elasticsearch/chunk.go#L56-L180)


---



<!-- ===== 10.2-memory-management-ui-and-api ===== -->

# Memory Management UI and API

Relevant source files
- web/src/locales/de.ts
- web/src/locales/en.ts
- web/src/locales/es.ts
- web/src/locales/fr.ts
- web/src/locales/id.ts
- web/src/locales/it.ts
- web/src/locales/ja.ts
- web/src/locales/pt-br.ts
- web/src/locales/ru.ts
- web/src/locales/vi.ts
- web/src/locales/zh-traditional.ts
- web/src/locales/zh.ts
- web/src/pages/dataset/dataset/reparse-dialog.tsx
- web/src/pages/dataset/process-log-modal.tsx
- web/src/pages/memories/add-or-edit-modal.tsx
- web/src/pages/memories/constants/index.tsx
- web/src/pages/memories/hooks.ts
- web/src/pages/memories/interface.ts
- web/src/pages/memory/memory-message/hook.ts
- web/src/pages/memory/memory-message/index.tsx
- web/src/pages/memory/memory-message/interface.ts
- web/src/pages/memory/memory-message/message-table.tsx
- web/src/pages/memory/memory-setting/advanced-settings-form.tsx
- web/src/pages/memory/memory-setting/hook.ts
- web/src/pages/memory/memory-setting/index.tsx
- web/src/pages/memory/memory-setting/memory-model-form.tsx
- web/src/services/memory-service.ts

The Memory System in RAGFlow provides persistent, long-term storage for agent interactions, moving beyond simple session-based history. This page documents the REST API endpoints, the frontend management interface, and the configuration mechanisms used to maintain and retrieve different types of memories (Raw, Semantic, Episodic, and Procedural).

## 1. Memory Management API

The backend provides a comprehensive REST API for managing memory entities and their contained messages. These APIs allow programmatic access to the multi-tiered memory system through the `memory_api` module.

### 1.1 Memory Entity Endpoints

These endpoints manage the metadata and configuration of memory "buckets".

| Method | Endpoint | Description |
| --- | --- | --- |
| `POST` | `/memories` | Create a new memory entity with specific LLM and embedding models. |
| `GET` | `/memories` | List all memories belonging to the current tenant with pagination and keyword filters. |
| `PUT` | `/memories/<memory_id>` | Update memory configuration including prompts, forgetting policy, and temperature. |
| `DELETE` | `/memories/<memory_id>` | Delete a memory and all its associated messages. |
| `GET` | `/memories/<memory_id>/config` | Retrieve the specific configuration for a memory ID. |

### 1.2 Message Operations

These endpoints allow for granular control over the data stored within a specific memory.

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/memories/<memory_id>` | Retrieve messages from a memory, filterable by `agent_id` and `keywords`. |
| `PUT` | `/memories/message/<message_id>` | Update the state (enabled/disabled) of a specific message. |
| `DELETE` | `/memories/message/<message_id>` | "Forget" a specific message by ID. |

### Data Flow: API to Service

The following diagram illustrates the transition from the REST layer to the service layer.

**Memory API Sequence**

```
"MySQL (memory table)"
"MemoryService [api/db/services/memory_service.py]"
"MemoryApiService [api/apps/services/memory_api_service.py]"
"MemoryAPI [api/apps/restful_apis/memory_api.py]"
"Client (Frontend/SDK)"
"MySQL (memory table)"
"MemoryService [api/db/services/memory_service.py]"
"MemoryApiService [api/apps/services/memory_api_service.py]"
"MemoryAPI [api/apps/restful_apis/memory_api.py]"
"Client (Frontend/SDK)"
"POST /memories (config)"
"create_memory(memory_info)"
"create_memory(tenant_id, ...)"
"Insert into memory table"
"Success"
"Memory Object"
"Formatted Data"
"200 OK (JSON Result)"
```

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/memory_api.py#L29-L63" min=29 max=63 file-path="api/apps/restful_apis/memory_api.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/memory_api_service.py#L76-L114" min=76 max=114 file-path="api/apps/services/memory_api_service.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/memory_service.py#L115-L151" min=115 max=151 file-path="api/db/services/memory_service.py">Hii</FileRef>`

## 2. Frontend Memory UI

The frontend interface for memory management provides a centralized hub for configuring memory behavior and auditing stored data.

### 2.1 Memory Configuration UI

The configuration page allows users to define how an agent learns and forgets. Key components include:

- Model Selection: Users specify an embedding_model and llm_id for processing and retrieving memories <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/memory-model-form.tsx#L14-L25" min=14 max=25 file-path="web/src/pages/memory/memory-setting/memory-model-form.tsx">Hii</FileRef>.
- Memory Types: Supports multiple concurrent types: raw, semantic, episodic, and procedural <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L123-L126" min=123 max=126 file-path="web/src/locales/en.ts">Hii</FileRef>.
- Advanced Settings:
Permissions: Controls access levels, such as "Only Me" (me) or "Team" (team)<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/advanced-settings-form.tsx#L67-L72" min=67 max=72 file-path="web/src/pages/memory/memory-setting/advanced-settings-form.tsx">Hii</FileRef>.Storage Type: Defines the underlying structure, currently supportingtable<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/advanced-settings-form.tsx#L85-L85" min=85  file-path="web/src/pages/memory/memory-setting/advanced-settings-form.tsx">Hii</FileRef>.Forgetting Policy: Logic for purging old memories, supportingFIFO(First-In-First-Out)<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/advanced-settings-form.tsx#L100-L100" min=100  file-path="web/src/pages/memory/memory-setting/advanced-settings-form.tsx">Hii</FileRef>.Prompts: Customizablesystem_promptanduser_promptused by the LLM during memory extraction<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/advanced-settings-form.tsx#L137-L158" min=137 max=158 file-path="web/src/pages/memory/memory-setting/advanced-settings-form.tsx">Hii</FileRef>.Temperature: Controls the randomness of the LLM when processing memories<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/advanced-settings-form.tsx#L119-L122" min=119 max=122 file-path="web/src/pages/memory/memory-setting/advanced-settings-form.tsx">Hii</FileRef>.

### 2.2 Message Table (Memory Browser)

The `MessageTable` component provides a detailed view of stored messages `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L78-L78" min=78 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>`.

| Column | Code Reference | Description |
| --- | --- | --- |
| **Session ID** | `accessorKey: 'session_id'` | Groups messages by chat session `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L127-L127" min=127 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>`. |
| **Agent** | `accessorKey: 'agent_name'` | Identifies which agent generated the memory `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L173-L173" min=173 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>`. |
| **Type** | `accessorKey: 'message_type'` | Displays if the memory is raw, semantic, etc. `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L182-L182" min=182 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>`. |
| **Enable/Disable** | `accessorKey: 'status'` | A switch to manually toggle whether a memory is active for retrieval `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L217-L224" min=217 max=224 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>`. |
| **Forget At** | `accessorKey: 'forget_at'` | Indicates when a memory will be purged based on policy `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L203-L203" min=203 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>`. |

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L124-L235" min=124 max=235 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/advanced-settings-form.tsx#L12-L163" min=12 max=163 file-path="web/src/pages/memory/memory-setting/advanced-settings-form.tsx">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/memory-service.ts#L1-L40" min=1 max=40 file-path="web/src/services/memory-service.ts">Hii</FileRef>`

## 3. Implementation and Data Flow

### 3.1 Code Entity Mapping

This diagram maps UI components to their corresponding backend service classes and database interactions.

**UI to Code Entity Mapping**

```
Core Logic & Storage

Backend API (Python)

Frontend (React)

AdvancedSettingsForm [web/src/pages/memory/memory-setting/advanced-settings-form.tsx]

MessageTable [web/src/pages/memory/memory-message/message-table.tsx]

memoryService [web/src/services/memory-service.ts]

MemoryApi [api/apps/restful_apis/memory_api.py]

MemoryApiService [api/apps/services/memory_api_service.py]

MemoryService [api/db/services/memory_service.py]

MessageService [api/db/services/message_service.py]

MySQL (memory table)

DocStore (memory indices)
```

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/services/memory-service.ts#L5-L39" min=5 max=39 file-path="web/src/services/memory-service.ts">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/restful_apis/memory_api.py#L25-L25" min=25 file-path="api/apps/restful_apis/memory_api.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/memory_api_service.py#L18-L27" min=18 max=27 file-path="api/apps/services/memory_api_service.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/memory_service.py#L28-L30" min=28 max=30 file-path="api/db/services/memory_service.py">Hii</FileRef>`

### 3.2 Constraints and Validation

The system enforces several technical constraints during memory management:

- Naming: Memory names are limited and cannot be empty or whitespace <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/memory_api_service.py#L88-L93" min=88 max=93 file-path="api/apps/services/memory_api_service.py">Hii</FileRef>.
- Size: Maximum storage size is capped by MEMORY_SIZE_LIMIT <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/memory_api_service.py#L165-L166" min=165 max=166 file-path="api/apps/services/memory_api_service.py">Hii</FileRef>.
- Localization: The UI supports extensive internationalization for memory-related terms across 13+ languages including English <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L123-L183" min=123 max=183 file-path="web/src/locales/en.ts">Hii</FileRef>, Chinese <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/zh.ts#L106-L180" min=106 max=180 file-path="web/src/locales/zh.ts">Hii</FileRef>, and German <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/de.ts#L112-L183" min=112 max=183 file-path="web/src/locales/de.ts">Hii</FileRef>.

### 3.3 Task Execution and Extraction

Saving to memory involves background tasks for indexing and extraction using LLMs.

- Extraction Logic: Semantic and procedural information is extracted from raw chat logs using LLMs based on user-defined prompts <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-setting/advanced-settings-form.tsx#L137-L158" min=137 max=158 file-path="web/src/pages/memory/memory-setting/advanced-settings-form.tsx">Hii</FileRef>.
- Status Management: The UI displays processing logs and progress for extraction tasks, mapping progress values to status labels like running, success, or failed <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/interface.ts#L1-L10" min=1 max=10 file-path="web/src/pages/memory/memory-message/interface.ts">Hii</FileRef> in components like ProcessLogModal <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/dataset/process-log-modal.tsx#L15-L25" min=15 max=25 file-path="web/src/pages/dataset/process-log-modal.tsx">Hii</FileRef>.
- Manual Control: Users can manually trigger "forgetting" (deletion) of messages via the UI action in MessageTable <FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L238-L255" min=238 max=255 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>.

Sources: `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/message-table.tsx#L217-L235" min=217 max=235 file-path="web/src/pages/memory/memory-message/message-table.tsx">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/api/apps/services/memory_api_service.py#L76-L114" min=76 max=114 file-path="api/apps/services/memory_api_service.py">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/de.ts#L148-L155" min=148 max=155 file-path="web/src/locales/de.ts">Hii</FileRef>`, `<FileRef file-url="https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/pages/memory/memory-message/interface.ts#L1-L10" min=1 max=10 file-path="web/src/pages/memory/memory-message/interface.ts">Hii</FileRef>`


---



<!-- ===== 11-administration-and-operations ===== -->

# Administration and Operations

Relevant source files
- admin/build_cli_release.sh
- admin/client/http_client.py
- admin/client/parser.py
- admin/client/ragflow_cli.py
- admin/client/ragflow_client.py
- api/utils/__init__.py
- api/utils/crypt.py
- cmd/ragflow_cli.go
- internal/cli/admin_parser.go
- internal/cli/cli.go
- internal/cli/cli_http.go
- internal/cli/common_command.go
- internal/cli/http_client.go
- internal/cli/lexer.go
- internal/cli/parser.go
- internal/cli/response.go
- internal/cli/types.go
- internal/cli/user_command.go
- internal/cli/user_parser.go
- internal/engine/engine.go
- internal/utility/convert.go

This page provides a technical overview of the administrative and operational infrastructure of RAGFlow. The system employs a dual-server architecture for administration (Python-based and Go-based), a secure sandbox for code execution, and a comprehensive suite of CLI and migration tools to manage multi-tenant environments and system health.

## Admin Service and CLI

RAGFlow features a dedicated administrative layer separate from the core RAG services. This layer is implemented as both a Python-based server and a Go-based service, with a powerful CLI for remote management.

### Admin Server Architecture

The Admin Server manages system-wide entities such as users, tenants, and global settings.

- Go Admin Backend: A high-performance administrative engine. The CLI in internal/cli/ interacts with the server via the AdminServerClient internal/cli/cli.go250-256 It provides sophisticated command parsing for administrative tasks like user management and system status internal/cli/admin_parser.go25-58
- Python Admin Backend: A Flask-based implementation providing management services for users, system settings, and environment configurations. It utilizes specialized managers like UserMgr and ServiceMgr to handle backend operations.
- Authentication: Both backends enforce superuser-only access. The system supports login via email and password, which are encrypted using scrypt internal/cli/common_command.go161-166
- User and Service Management: Implements logic for listing all services admin/client/ragflow_client.py120-130 showing service details admin/client/ragflow_client.py132-157 and managing user lifecycles internal/cli/user_command.go27-117

### RAGFlow CLI

The `ragflow-cli` is a command-line interface that translates SQL-like commands into API requests. The Go implementation [internal/cli/cli.go](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/cli.go) supports multiple modes: `api`, `admin`, `ingestor`, and `collector` [internal/cli/cli.go69-75](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/cli.go#L69-L75)

| Component | Responsibility | Source |
| --- | --- | --- |
| `Lexer` | Tokenizes SQL-like CLI input into discrete symbols | [internal/cli/lexer.go79-130](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/lexer.go#L79-L130) |
| `Parser` | Recursive descent parser for CLI commands | [internal/cli/parser.go49-61](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/parser.go#L49-L61) |
| `RAGFlowClient` | Python CLI HTTP client for API interaction | [admin/client/ragflow_client.py52-56](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/client/ragflow_client.py#L52-L56) |
| `ResponseIf` | Interface for formatting and printing CLI output | [internal/cli/response.go24-29](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/response.go#L24-L29) |

For details, see [Admin Service and CLI](/infiniflow/ragflow/11.1-admin-service-and-cli).

## Sandbox Code Executor

To support the **Code** component within agent workflows, RAGFlow utilizes a secure sandbox environment. This ensures that user-provided scripts are executed in isolation.

- Isolation Strategy: The system manages code execution through a specialized administrative interface.
- Language Support: The sandbox is designed to handle multiple languages (Python, Node.js) safely within agentic workflows, preventing malicious code from impacting the host system.
- Integration: The ragflow-cli provides commands to interact with system components that may trigger sandboxed execution, such as parsing or pipeline triggers internal/cli/types.go95-101

For details, see [Sandbox Code Executor](/infiniflow/ragflow/11.2-sandbox-code-executor).

## Testing Infrastructure

RAGFlow maintains a multi-layered testing suite to ensure system stability across different document engines and model providers.

- CLI Testing: The Go CLI includes benchmarking capabilities to test server performance over multiple iterations internal/cli/user_command.go42-52
- System Probes: Administrative commands like ping allow for health monitoring of both the admin and API servers internal/cli/common_command.go106-158
- Configuration Validation: The CLI can list and verify system configurations, including Redis, Database, and Document Engine connectivity internal/cli/user_command.go116-207

For details, see [Testing Infrastructure](/infiniflow/ragflow/11.3-testing-infrastructure).

## Migration and Utility Tools

The system includes utilities for operational maintenance, data integrity, and cross-engine migration.

- Database Utilities: The CLI supports commands for managing datasets and metadata internal/cli/user_parser.go222-268
- Configuration Management: ragflow-cli can list system environments and active configurations internal/cli/user_parser.go130-144 allowing administrators to verify the operational state of the storage and document engines internal/cli/user_command.go128-148
- Security: Includes utilities for password encryption and RSA-based public key encryption for secure transmissions admin/client/ragflow_client.py38-43

For details, see [Migration and Utility Tools](/infiniflow/ragflow/11.4-migration-and-utility-tools).

## System Interaction Diagram

### Administrative Control Flow (Go CLI)

This diagram bridges the Go CLI interface to the backend system entities.

```
Remote Servers

Command Execution

Interface Layer [internal/cli/]

AdminMode

APIMode

HttpClient

HttpClient

Login/Ping

CLI (cli.go)

Lexer (lexer.go)

Parser (parser.go)

UserCommand (user_command.go)

AdminCommand (admin_command.go)

CommonCommand (common_command.go)

Admin Server (:admin/ping)

API Server (:system/ping)
```

**Sources:** [internal/cli/cli.go108-117](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/cli.go#L108-L117) [internal/cli/parser.go249-262](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/parser.go#L249-L262) [internal/cli/common_command.go106-125](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/common_command.go#L106-L125) [internal/cli/user_command.go40-72](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_command.go#L40-L72)

### Configuration Discovery

This diagram maps how administrative tools resolve system configuration from the backend.

```
API Endpoint

Config Structure

CLI Tool

GET

JSON

GetConfigs

GetConfigs

GetConfigs

GetConfigs

ListConfigs (internal/cli/user_command.go)

Redis Config

Database Config

DocEngine (ES/Infinity)

Storage (MinIO)

/system/configs
```

**Sources:** [internal/cli/user_command.go74-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_command.go#L74-L114) [internal/cli/user_command.go116-207](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_command.go#L116-L207)

**Sources:**

- internal/cli/cli.go 39-117
- internal/cli/parser.go 49-262
- internal/cli/lexer.go 79-130
- internal/cli/user_command.go 40-207
- internal/cli/common_command.go 32-202
- internal/cli/admin_parser.go 25-284
- internal/cli/user_parser.go 119-268
- admin/client/ragflow_client.py 38-157
- admin/client/parser.py 19-228
- internal/cli/response.go 24-258


---



<!-- ===== 11.1-admin-service-and-cli ===== -->

# Admin Service and CLI

Relevant source files
- admin/build_cli_release.sh
- admin/client/http_client.py
- admin/client/parser.py
- admin/client/ragflow_cli.py
- admin/client/ragflow_client.py
- api/utils/__init__.py
- api/utils/crypt.py
- cmd/admin_server.go
- cmd/ragflow_cli.go
- internal/admin/handler.go
- internal/admin/password.go
- internal/admin/router.go
- internal/admin/service.go
- internal/cli/admin_command.go
- internal/cli/admin_parser.go
- internal/cli/cli.go
- internal/cli/cli_http.go
- internal/cli/common_command.go
- internal/cli/http_client.go
- internal/cli/lexer.go
- internal/cli/parser.go
- internal/cli/response.go
- internal/cli/types.go
- internal/cli/user_command.go
- internal/cli/user_parser.go
- internal/common/error_code.go
- internal/common/smtp_config.go
- internal/common/status_message.go
- internal/dao/user.go
- internal/engine/elasticsearch/client.go
- internal/engine/engine.go
- internal/handler/user.go
- internal/server/config.go
- internal/service/user.go
- internal/utility/captcha_png.go
- internal/utility/convert.go
- internal/utility/otp.go
- internal/utility/smtp.go
- internal/utility/token.go

The RAGFlow Admin Service and CLI provide a dedicated management layer for system administrators to oversee users, monitor service health, manage ingestion tasks, and perform maintenance. This system is implemented as a dual-layer architecture: a Go-based admin server for core operations and a multi-language CLI (Python and Go) for developer and operator interactions.

## Admin Service Architecture

The Admin Service operates as a privileged management interface. It provides endpoints for superuser authentication, user lifecycle management, and system-wide task monitoring.

### Server Implementation

The administrative logic is primarily implemented in the Go backend, which serves as the central point for privileged operations.

- Go Admin Server: Initialized via the AdminConfig which defines the host and port internal/server/config.go58-62 The server uses the Gin framework and is managed by internal/admin/handler.go and internal/admin/service.go internal/admin/handler.go39-50
- Authentication: Admin login requires the IsSuperuser flag to be true. The Login handler in Go verifies this flag after a successful email/password check internal/admin/handler.go145-153
- Service Layer: The admin.Service struct aggregates various DAOs (Data Access Objects) to perform cross-functional administrative tasks, including user, tenant, and ingestion task management internal/admin/service.go45-91

### Data Flow: Admin Authentication

The following diagram illustrates the authentication flow for administrative access via the Go-based admin handler.

**Admin Login Sequence**

```
"MySQL/PostgreSQL"
"internal/dao.UserDAO (Go)"
"internal/service.UserService (Go)"
"internal/admin.Handler (Go)"
"ragflow-cli (Go)"
"MySQL/PostgreSQL"
"internal/dao.UserDAO (Go)"
"internal/service.UserService (Go)"
"internal/admin.Handler (Go)"
"ragflow-cli (Go)"
"POST /admin/login {email, password}"
"LoginByEmail(req)"
"GetByEmail(email)"
"SELECT * FROM user WHERE email=..."
"User Record"
"HashPassword & Compare"
"User Object"
"Check *user.IsSuperuser == true"
"utility.DumpAccessToken(token, secret)"
"200 OK + Authorization Header + User Data"
```

Sources: [internal/admin/handler.go124-185](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L124-L185) [internal/service/user.go108-171](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/service/user.go#L108-L171) [internal/admin/handler.go145-152](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L145-L152)

## ragflow-cli

The `ragflow-cli` is a command-line interface that allows administrators and users to interact with RAGFlow services. It features a robust Go-based implementation with a custom parser and a Python client for scriptability.

### Implementation Details

- Command Parsing: The Go CLI uses a custom recursive descent Parser internal/cli/parser.go27-42 and a Lexer internal/cli/lexer.go25-30 to handle SQL-like syntax. It supports different modes: AdminMode and APIMode internal/cli/parser.go254-262
- Lexical Analysis: The lexer recognizes keywords like LOGIN, DATASETS, INGESTION, and METADATA internal/cli/lexer.go187-250 internal/cli/types.go26-185
- Configuration: The CLI reads from rf.yml to manage multiple API server profiles, including host, tokens, and credentials internal/cli/cli.go49-56

### Supported Command Categories

| Category | Example Commands | File Reference |
| --- | --- | --- |
| **System** | `PING`, `SHOW SERVER VERSION`, `LIST CONFIGS` | [internal/cli/user_command.go40-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_command.go#L40-L114) [internal/cli/user_parser.go62-70](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_parser.go#L62-L70) |
| **User Mgmt** | `REGISTER USER`, `LOGIN USER`, `LIST USERS` | [internal/cli/user_parser.go72-117](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_parser.go#L72-L117) [internal/cli/user_parser.go27-60](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_parser.go#L27-L60) |
| **Data Mgmt** | `LIST DATASETS`, `LIST DOCUMENTS FROM 'id'`, `GET METADATA OF DATASET` | [internal/cli/user_parser.go186-220](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_parser.go#L186-L220) [internal/cli/user_parser.go222-268](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_parser.go#L222-L268) |
| **Ingestion** | `LIST INGESTION`, `START INGESTION`, `STOP INGESTION` | [internal/cli/user_parser.go162-163](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_parser.go#L162-L163) [internal/cli/user_parser.go221-224](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_parser.go#L221-L224) |
| **Admin** | `LIST SERVICES`, `SHOW SERVICE <num>`, `SHUTDOWN SERVICE` | [admin/client/ragflow_client.py120-170](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/client/ragflow_client.py#L120-L170) [internal/cli/parser.go80-144](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/parser.go#L80-L144) |

Sources: [internal/cli/user_parser.go17-270](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_parser.go#L17-L270) [internal/cli/types.go26-203](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/types.go#L26-L203) [internal/cli/user_command.go40-114](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_command.go#L40-L114)

## Admin Service Logic

The `admin.Service` in Go encapsulates the business logic for privileged operations.

### Key Management Functions

- Ingestion Task Monitoring: ListIngestionTasks retrieves all tasks and joins them with user information and the latest logs to show the current processing step internal/admin/service.go104-148
- Task Control: StopIngestionTasks sets a task status to STOPPING and publishes the stop signal to the message queue (e.g., Redis Streams or Nats) internal/admin/service.go167-186
- User Management: ListUsers provides a summary of all registered accounts, including their superuser status and creation dates internal/admin/service.go214-231
- Session Management: Logout invalidates a user's session by prefixing their access token with INVALID_ in the database internal/admin/service.go94-101

### Health Monitoring and Diagnostics

The admin handler provides diagnostic endpoints for system maintenance.

- Health Check: GET /admin/health returns basic service status internal/admin/handler.go106-108
- Configuration Inspection: GetConfigs allows administrators to verify the connectivity and host addresses for Redis, the Database, and the Document Engine (Elasticsearch or Infinity) internal/cli/user_command.go116-207

**Code Entity Mapping: Admin Service Layer**

```
Code Entity Space

Natural Language Space

calls

queries

signals stop

pings

Admin manages ingestion tasks

Monitor system health

Review system configuration

internal/admin.Handler

internal/admin.Service

internal/dao.IngestionTaskDAO

internal/engine.MQEngine

internal/admin.Ping
```

Sources: [internal/admin/handler.go39-50](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L39-L50) [internal/admin/service.go45-91](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L45-L91) [internal/admin/service.go167-186](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/service.go#L167-L186) [internal/admin/handler.go111-113](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/admin/handler.go#L111-L113)

## Health Monitoring Implementation

The system tracks service availability and configuration integrity through several mechanisms:

1. Ping/Pong Mechanism: Both Admin and API modes support a PING command. Admin mode hits /admin/ping while API mode hits /system/ping internal/cli/common_command.go106-125
2. Benchmark Mode: Commands like ShowServerVersion and Ping support an iterations parameter to perform multiple requests and measure average duration/latency internal/cli/user_command.go40-52 internal/cli/common_command.go98-104
3. Engine Diagnostics: The GetConfigs utility specifically extracts host and type information for the DocEngine (Elasticsearch/Infinity) and StorageEngine (Minio), allowing operators to identify configuration mismatches internal/cli/user_command.go128-205
4. Client-Side Validation: The Python RAGFlowClient performs a ping_server check before login to ensure the target instance is reachable admin/client/ragflow_client.py57-68

Sources: [internal/cli/user_command.go40-207](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/user_command.go#L40-L207) [internal/cli/common_command.go98-158](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/cli/common_command.go#L98-L158) [admin/client/ragflow_client.py86-98](https://github.com/infiniflow/ragflow/blob/d32e05d5/admin/client/ragflow_client.py#L86-L98)


---



<!-- ===== 11.2-sandbox-code-executor ===== -->

# Sandbox Code Executor

Relevant source files
- README.md
- README_ar.md
- README_fr.md
- README_id.md
- README_ja.md
- README_ko.md
- README_pt_br.md
- README_tr.md
- README_tzh.md
- README_zh.md
- agent/tools/code_exec.py
- agent/tools/exesql.py
- docker/.env
- docker/README.md
- docs/guides/manage_files.md
- docs/quickstart.mdx
- web/src/pages/agent/form/exesql-form/use-submit-form.ts
- web/src/pages/agent/form/tool-form/exesql-form/index.tsx
- web/src/pages/agent/options.ts
- web/src/pages/next-chats/hooks/use-build-form-refs.ts

The Sandbox Code Executor is a secure execution environment designed to run arbitrary Python or JavaScript code within RAGFlow agent workflows. It provides a pluggable architecture that ensures user-provided scripts are isolated from the host environment while maintaining high-performance data exchange with the workflow engine. For self-hosted deployments, it leverages **gVisor** to provide a secure sandbox for the code executor [README_zh.md155](https://github.com/infiniflow/ragflow/blob/d32e05d5/README_zh.md?plain=1#L155-L155)

## System Architecture

The sandbox system is integrated into the Agent workflow via the `Code` component, which allows developers to add custom logic to their RAG pipelines [README.md100](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L100-L100)

### Natural Language to Code Entity Mapping

The following diagrams illustrate how user-defined logic in the RAGFlow Canvas translates into backend execution entities and how data flows through the system.

**Workflow to Execution Mapping**

```
ExecutionProviders

CodeEntitySpace

NaturalLanguageSpace

UserScript

InputVariables

ParsedBy

calls

delegates

invokes

Option1

Option2

Option3

CanvasCodeComponent

JSON_DSL

ArgumentsMap

agent.tools.code_exec.CodeExec

agent.sandbox.client.execute_code

agent.sandbox.providers.manager.ProviderManager

SandboxProvider.execute_code

AliyunCodeInterpreterProvider

SelfManagedProvider_gVisor

LocalProvider_Subprocess
```

Sources: [agent/tools/code_exec.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/code_exec.py#L29-L29) [README.md100](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L100-L100)

**Data and Component Integration Flow**

```
RAGFlowBackend

ParsesStdout

ValidatedBy

Resolution

extract_structured_result

StructuredResult

agent.tools.code_exec.build_code_exec_contract

Canvas_Variable_Resolution

SandboxEnvironment

Processes

Returns

main_function

arguments

RawResult
```

Sources: [agent/tools/code_exec.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/code_exec.py#L29-L29) [agent/tools/code_exec.py192-207](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/code_exec.py#L192-L207)

## Deployment and Requirements

To use the sandbox features in a self-hosted environment, specific host-level requirements must be met:

- gVisor: Required for the code executor (sandbox) feature to ensure secure isolation README_zh.md155 docs/quickstart.mdx35-36
- Operating System: Instructions are provided for Linux, macOS (Docker Desktop), and Windows (WSL 2) docs/quickstart.mdx43-182
- Memory Management: vm.max_map_count must be set to at least 262144 to ensure components like Elasticsearch function correctly alongside the sandbox README_zh.md162-175 docs/quickstart.mdx45-52

## Provider Implementations

RAGFlow supports multiple sandbox backends to accommodate different security and infrastructure needs.

| Provider | Description |
| --- | --- |
| **Self-Managed** | Uses gVisor-protected containers. Recommended for production self-hosting [README_zh.md155](https://github.com/infiniflow/ragflow/blob/d32e05d5/README_zh.md?plain=1#L155-L155) |
| **Aliyun** | Integration with Aliyun Code Interpreter for serverless execution. |
| **Local** | Executes code as a local process. Intended for development only. |
| **Node.js / Python** | Supports both Python and JavaScript/Node.js languages [README.md100](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L100-L100) |

Sources: [README.md100](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L100-L100) [README_zh.md155](https://github.com/infiniflow/ragflow/blob/d32e05d5/README_zh.md?plain=1#L155-L155)

## Integration with Agent Workflow

The Sandbox is the backbone of the **Code component**, implemented by the `CodeExec` class [agent/tools/code_exec.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/code_exec.py#L29-L29)

### Variable Resolution

The sandbox receives input variables resolved from the workflow context. For example, the `ExeSQL` tool uses a similar pattern where parameters like `sql` can be resolved from system variables like `{sys.query}` [agent/tools/exesql.py29-46](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/exesql.py#L29-L46)

### Execution Lifecycle

1. Code Wrapping: Raw user code is wrapped to handle argument passing and result serialization.
2. Execution: The code is executed within the chosen provider with defined timeouts agent/tools/exesql.py83-84
3. Result Extraction: The backend parses the output to extract structured JSON results and any generated artifacts (like images or CSVs).

## Security and Constraints

### Type Validation

The `CodeExec` tool enforces a strict contract between the sandbox and the engine to prevent unauthorized system access.

- Reserved Keys: Scripts are restricted from returning keys that conflict with internal system outputs agent/tools/code_exec.py29
- Database Security: For security reasons, tools like ExeSQL explicitly block execution against the internal rag_flow database using default credentials to prevent accidental data corruption or leakage agent/tools/exesql.py65-69

### Resource Limits

- Timeout: Component execution is governed by a timeout, defaulting to COMPONENT_EXEC_TIMEOUT (60s) if not specified agent/tools/exesql.py83
- Isolation: gVisor provides a second layer of defense by intercepting syscalls from the sandbox container README_zh.md155

Sources: [agent/tools/code_exec.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/code_exec.py#L29-L29) [agent/tools/exesql.py65-69](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/exesql.py#L65-L69) [agent/tools/exesql.py83](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/tools/exesql.py#L83-L83) [README_zh.md155](https://github.com/infiniflow/ragflow/blob/d32e05d5/README_zh.md?plain=1#L155-L155)


---



<!-- ===== 11.3-testing-infrastructure ===== -->

# Testing Infrastructure

Relevant source files
- .github/workflows/release.yml
- .github/workflows/tests.yml
- Dockerfile
- Dockerfile.deps
- api/apps/restful_apis/dataset_api.py
- docker/docker-compose-base.yml
- docker/infinity_conf.toml
- download_deps.py
- helm/values.yaml
- pyproject.toml
- sdk/python/pyproject.toml
- sdk/python/test/conftest.py
- sdk/python/test/test_frontend_api/common.py
- sdk/python/test/test_frontend_api/test_chunk.py
- sdk/python/test/test_frontend_api/test_dataset.py
- sdk/python/uv.lock
- test/benchmark/README.md
- test/benchmark/auth.py
- test/benchmark/chat.py
- test/benchmark/cli.py
- test/testcases/configs.py
- test/testcases/conftest.py
- test/testcases/test_http_api/common.py
- test/testcases/test_http_api/test_dataset_management/test_create_dataset.py
- test/testcases/test_http_api/test_dataset_management/test_delete_datasets.py
- test/testcases/test_http_api/test_dataset_management/test_knowledge_graph.py
- test/testcases/test_http_api/test_dataset_management/test_list_datasets.py
- test/testcases/test_http_api/test_dataset_management/test_update_dataset.py
- test/testcases/test_http_api/test_file_management_within_dataset/test_update_document.py
- test/testcases/test_http_api/test_session_management/test_chat_completions_openai.py
- test/testcases/test_http_api/test_session_management/test_related_questions.py
- test/testcases/test_sdk_api/test_chunk_management_within_dataset/test_add_chunk.py
- test/testcases/test_sdk_api/test_dataset_mangement/test_create_dataset.py
- test/testcases/test_sdk_api/test_dataset_mangement/test_delete_datasets.py
- test/testcases/test_sdk_api/test_dataset_mangement/test_list_datasets.py
- test/testcases/test_sdk_api/test_dataset_mangement/test_update_dataset.py
- test/testcases/test_sdk_api/test_file_management_within_dataset/test_update_document.py
- test/testcases/test_web_api/test_dataset_management/test_dataset_sdk_routes_unit.py
- uv.lock
- web/src/pages/dataset/dataset/generate-button/hook.ts
- web/src/utils/llm-util.ts

The RAGFlow testing infrastructure provides a comprehensive validation suite covering the entire stack, from core mathematical utilities and document parsers to end-to-end browser interactions. The system utilizes `pytest` as the primary test runner and integrates with GitHub Actions for continuous integration.

## Test Suite Organization

The test suite is categorized into several distinct layers based on the interface they target and their position in the request lifecycle. The project uses `uv` for dependency management and isolated test environments [pyproject.toml182-199](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L182-L199)

| Category | Location | Purpose | Key Tools |
| --- | --- | --- | --- |
| **SDK Tests** | `test/testcases/test_sdk_api/` | Validates the Python SDK client logic and its interaction with the server. | `ragflow-sdk`, `pytest` |
| **HTTP API Tests** | `test/testcases/test_http_api/` | Tests the RESTful API endpoints directly via `requests`. | `requests`, `pytest`, `hypothesis` |
| **Web API Tests** | `test/testcases/test_web_api/` | Tests the internal APIs used by the React frontend. | `requests`, `pytest` |
| **SDK Unit Tests** | `sdk/python/test/` | Validates specific SDK components and client-side logic. | `pytest`, `requests` |
| **Playwright E2E** | `test/playwright/` | Browser-based end-to-end testing of the application. | `pytest-playwright` |
| **Unit Tests** | `test/unit_test/` | Tests isolated business logic and utility functions using mocks. | `pytest`, `unittest.mock` |

### System Data Flow in Testing

The following diagram illustrates how different test types interact with the RAGFlow ecosystem, bridging natural language test definitions to code entities.

**Testing Interface Architecture**

```
Backend_Services

Code_Entities

Test_Runners

SDK Tests (test/testcases/test_sdk_api/)

HTTP API Tests (test/testcases/test_http_api/)

Web API Tests (test/testcases/test_web_api/)

Unit Tests (test/unit_test/)

Playwright E2E (test/playwright/)

ragflow_sdk.DataSet (sdk/python/ragflow_sdk/modules/dataset.py)

ragflow_server.py (api/ragflow_server.py)

RAGFlowWebApiAuth (test/testcases/test_web_api/common.py)

RAGFlowHttpApiAuth (test/testcases/test_http_api/common.py)

MySQL (docker/docker-compose-base.yml)

DocStore: Infinity/ES/OpenSearch (docker/docker-compose-base.yml)

MinIO Object Store (docker/docker-compose-base.yml)

sandbox-executor-manager (docker/docker-compose-base.yml)
```

**Sources:** [test/testcases/test_http_api/common.py18-34](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_http_api/common.py#L18-L34) [docker/docker-compose-base.yml158-184](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L158-L184) [pyproject.toml183-199](https://github.com/infiniflow/ragflow/blob/d32e05d5/pyproject.toml#L183-L199) [.github/workflows/tests.yml32-37](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L32-L37)

## Test Configuration and CI Integration

RAGFlow leverages `pytest` fixtures, markers, and GitHub Actions to manage state and prioritize execution across different environments.

### Pytest Markers and Priority

The test suite utilizes priority markers to categorize tests, defined in `pytest.ini_options` [sdk/python/pyproject.toml26-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/pyproject.toml#L26-L31):

- @pytest.mark.p1: Critical/High priority functional tests, such as basic dataset creation or document upload.
- @pytest.mark.p2: Medium priority tests, covering specific functional variations and configuration changes.
- @pytest.mark.p3: Low priority or edge cases, including malformed payloads and stress testing.

### CI Workflow (GitHub Actions)

The CI pipeline in `.github/workflows/tests.yml` automates the testing process:

1. Static Analysis: Runs ruff for linting github/workflows/tests.yml94-99
2. Environment Setup:
Builds the Go server componentsgithub/workflows/tests.yml132-143Prepares test resources and Docker imagesgithub/workflows/tests.yml144-1503. Service Orchestration: Uses docker-compose to spin up the full stack, including MySQL, Redis, MinIO, and the configured DocStore (Infinity/ES/OpenSearch) docker/docker-compose-base.yml1-230
4. Test Execution: Runs the test suite against the live containers. The system handles workspace ownership and duplication checks to optimize runner usage github/workflows/tests.yml39-91

**Sources:** [sdk/python/pyproject.toml26-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/pyproject.toml#L26-L31) [.github/workflows/tests.yml94-156](https://github.com/infiniflow/ragflow/blob/d32e05d5/.github/workflows/tests.yml#L94-L156) [docker/docker-compose-base.yml1-230](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L230)

## Specialized Testing Scenarios

### SDK and HTTP API Validation

The testing infrastructure uses a common set of utilities for API interaction. For example, `test/testcases/test_http_api/common.py` defines standard wrappers for dataset management, file management, and chunk operations [test/testcases/test_http_api/common.py37-189](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_http_api/common.py#L37-L189) These wrappers ensure consistency across test cases and provide a clean interface for `requests`-based testing.

### Test Environment Fixtures

In the Python SDK test suite, `sdk/python/test/conftest.py` manages the lifecycle of the test environment:

- Authentication: Handles user registration and login to obtain Authorization headers sdk/python/test/conftest.py42-60
- Model Provisioning: Automatically adds model providers (e.g., ZHIPU-AI) and configures default chat and embedding models for the tenant sdk/python/test/conftest.py119-210
- Token Management: Provides fixtures for API key generation used in downstream tests sdk/python/test/conftest.py64-77

**Mocked Entity Architecture**

```
Mocked_Entities

Unit_Test_Context

Test Function

Monkeypatch (pytest)

DialogService Mock (test/unit_test/api/db/services/test_dialog_service.py)

UserService Mock (api/apps/restful_apis/user_api.py)

common.configs Stub (test/testcases/configs.py)
```

**Sources:** [sdk/python/test/conftest.py42-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/test/conftest.py#L42-L210) [test/testcases/test_http_api/common.py18-34](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_http_api/common.py#L18-L34)

## Test Capabilities and Stress Testing

### Document Processing Validation

The test suite includes utilities to programmatically generate test files (TXT, PDF, DOCX) to verify the document processing pipeline [test/testcases/test_http_api/common.py75-101](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_http_api/common.py#L75-L101) Tests cover:

- Upload & Download: Ensuring binary integrity of files in MinIO test/testcases/test_http_api/common.py103-116
- Parsing Control: Testing the ability to start and stop document parsing asynchronously test/testcases/test_http_api/common.py140-149
- Chunk Management: Validating the manual addition, listing, and deletion of document chunks test/testcases/test_http_api/common.py165-190

### Integration with External Backends

Tests are designed to run against different backends configured in `docker/docker-compose-base.yml`. This includes:

- Elasticsearch: Standard document store docker/docker-compose-base.yml2-38
- Infinity: High-performance vector database docker/docker-compose-base.yml76-101
- OpenSearch: Alternative document store docker/docker-compose-base.yml40-74
- OceanBase/SeekDB: Distributed relational databases docker/docker-compose-base.yml103-156

**Sources:** [test/testcases/test_http_api/common.py75-190](https://github.com/infiniflow/ragflow/blob/d32e05d5/test/testcases/test_http_api/common.py#L75-L190) [docker/docker-compose-base.yml1-156](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/docker-compose-base.yml#L1-L156) [sdk/python/pyproject.toml13-23](https://github.com/infiniflow/ragflow/blob/d32e05d5/sdk/python/pyproject.toml#L13-L23)


---



<!-- ===== 11.4-migration-and-utility-tools ===== -->

# Migration and Utility Tools

Relevant source files
- README.md
- README_ar.md
- README_fr.md
- README_id.md
- README_ja.md
- README_ko.md
- README_pt_br.md
- README_tr.md
- README_tzh.md
- README_zh.md
- docker/.env
- docker/README.md
- docker/docker-compose.yml
- docker/entrypoint.sh
- docker/launch_backend_service.sh
- docs/guides/manage_files.md
- docs/quickstart.mdx
- internal/entity/tenant_model.go
- internal/entity/tenant_model_instance.go
- mcp/client/client.py
- mcp/client/streamable_http_client.py
- mcp/server/server.py
- tools/scripts/README.md
- tools/scripts/db_schema_sync.py
- tools/scripts/mysql_migration.py

This section documents the operational utilities, data synchronization tools, and database migration frameworks provided in the RAGFlow codebase. These tools facilitate system maintenance, data migration between storage engines, and administrative management.

## Database Migration Framework

RAGFlow uses a specialized migration script to handle schema changes and data transformations in MySQL, ensuring consistency across version upgrades.

### MySQL Migration Script

The `mysql_migration.py` tool provides a flexible framework for managing database schema evolution. It supports multi-stage migrations, version tracking, and execution logging.

| Code Entity | Role | File Reference |
| --- | --- | --- |
| `MigrationDatabase` | Wrapper for `peewee.MySQLDatabase` and `playhouse.migrate.MySQLMigrator`. | [tools/scripts/mysql_migration.py140-153](https://github.com/infiniflow/ragflow/blob/d32e05d5/tools/scripts/mysql_migration.py#L140-L153) |
| `MigrationStats` | Tracks duration, tables operated, and rows processed during migration. | [tools/scripts/mysql_migration.py98-123](https://github.com/infiniflow/ragflow/blob/d32e05d5/tools/scripts/mysql_migration.py#L98-L123) |
| `MIGRATION_DB_VERSION_MARKER` | System setting key used to track the current database version. | [tools/scripts/mysql_migration.py59](https://github.com/infiniflow/ragflow/blob/d32e05d5/tools/scripts/mysql_migration.py#L59-L59) |
| `run_mysql_migrations` | Function in the migration tool that executes the migration logic. | [tools/scripts/mysql_migration.py125-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/tools/scripts/mysql_migration.py#L125-L137) |

**Database Migration Data Flow**

```
Schema Entities (internal/entity/)

Migration Tool (tools/scripts/mysql_migration.py)

Entrypoint (docker/entrypoint.sh)

--init-model-provider-tables

python3 tools/scripts/mysql_migration.py

MigrationDatabase

Migration Stages (--stages)

upsert_system_setting

tenant_model_provider.go

tenant_model_instance.go

tenant_model.go
```

Sources: [tools/scripts/mysql_migration.py125-137](https://github.com/infiniflow/ragflow/blob/d32e05d5/tools/scripts/mysql_migration.py#L125-L137) [tools/scripts/mysql_migration.py194-210](https://github.com/infiniflow/ragflow/blob/d32e05d5/tools/scripts/mysql_migration.py#L194-L210) [docker/entrypoint.sh94-97](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L94-L97) [internal/entity/tenant_model.go1-25](https://github.com/infiniflow/ragflow/blob/d32e05d5/internal/entity/tenant_model.go#L1-L25)

## Document Store Integration and Configuration

RAGFlow implements a polyglot persistence strategy, abstracting connectivity to multiple document engines (Elasticsearch, Infinity, OpenSearch, and OceanBase) through environment-driven configuration.

### Document Engine Selection

The `DOC_ENGINE` environment variable determines which vector database backend is utilized at startup.

| Engine Option | Description | File Reference |
| --- | --- | --- |
| `elasticsearch` | Default search engine for RAG tasks. | [docker/.env20](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L20-L20) |
| `infinity` | High-performance AI-native database. | [docker/.env16](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L16-L16) |
| `oceanbase` | Distributed relational database for high-concurrency RAG. | [docker/.env17](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L17-L17) |
| `opensearch` | Open-source fork of Elasticsearch. | [docker/.env18](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L18-L18) |

### OceanBase Integration

OceanBase configuration is managed through specific environment variables in the Docker environment, including memory limits and cluster names.

| Variable | Role | File Reference |
| --- | --- | --- |
| `OCEANBASE_HOST` | Hostname for the OceanBase service. | [docker/.env75](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L75-L75) |
| `OB_MEMORY_LIMIT` | Maximum memory allocated to the OceanBase container (Default: 10G). | [docker/.env91](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L91-L91) |
| `OB_DATAFILE_SIZE` | Size of the data file for OceanBase storage. | [docker/.env93](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L93-L93) |

**Document Store Connectivity Architecture**

```
System Backend

Service Template (conf/service_conf.yaml.template)

Environment Configuration (docker/.env)

DOC_ENGINE

ES_HOST / ES_PORT

OCEANBASE_HOST / OCEANBASE_PORT

service_conf.yaml

Elasticsearch Cluster

OceanBase Cluster

Infinity Engine
```

Sources: [docker/.env20-38](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L20-L38) [docker/.env75-95](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L75-L95) [docker/entrypoint.sh163-182](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L163-L182)

## Web Scraping and External Data Sync

RAGFlow supports data synchronization from diverse external sources including Confluence, Notion, and Discord.

### Data Sync Workers

The `entrypoint.sh` script manages the lifecycle of data synchronization workers, which can be disabled via the `--disable-datasync` flag.

| Component | Logic | File Reference |
| --- | --- | --- |
| `ENABLE_DATASYNC` | Boolean flag to toggle external data source synchronization. | [docker/entrypoint.sh38](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L38-L38) |
| `datasync_exe` | Shell function that launches the data synchronization process. | [docker/entrypoint.sh236-241](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L236-L241) |

Sources: [docker/entrypoint.sh82-85](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L82-L85) [docker/entrypoint.sh236-241](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L236-L241)

## Administrative and Operational Utilities

### Admin Server and CLI

RAGFlow includes an Admin Server for system-level management, enabled via the `--enable-adminserver` flag.

| Service | Port | File Reference |
| --- | --- | --- |
| Admin Server (Python) | `ADMIN_SVR_HTTP_PORT` (9381) | [docker/.env156](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L156-L156) |
| Admin Server (Go) | `GO_ADMIN_PORT` (9383) | [docker/.env159](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/.env#L159-L159) |

### System Bootstrapping and Configuration

The `entrypoint.sh` script coordinates the initialization of the environment. It dynamically generates the `service_conf.yaml` file from a template by replacing environment variable placeholders.

**Configuration Generation Flow**

```
Output

Processing (docker/entrypoint.sh)

Input Files

service_conf.yaml.template

.env Variables

DEF_ENV_VALUE_PATTERN

eval echo

/ragflow/conf/service_conf.yaml
```

Sources: [docker/entrypoint.sh163-182](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L163-L182) [docker/entrypoint.sh90-93](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L90-L93)

### MCP Server Integration

The Model Context Protocol (MCP) server can be initialized through the entrypoint, allowing external tools to interact with RAGFlow's context layer.

| Parameter | Default Value | File Reference |
| --- | --- | --- |
| `MCP_PORT` | 9382 | [docker/entrypoint.sh48](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L48-L48) |
| `MCP_MODE` | self-host | [docker/entrypoint.sh51](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L51-L51) |
| `MCP_SCRIPT_PATH` | /ragflow/mcp/server/server.py | [docker/entrypoint.sh50](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L50-L50) |

Sources: [docker/entrypoint.sh47-55](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L47-L55) [docker/entrypoint.sh86-89](https://github.com/infiniflow/ragflow/blob/d32e05d5/docker/entrypoint.sh#L86-L89)


---



<!-- ===== 12-glossary ===== -->

# Glossary

Relevant source files
- README.md
- README_ar.md
- README_fr.md
- README_id.md
- README_ja.md
- README_ko.md
- README_pt_br.md
- README_tr.md
- README_tzh.md
- README_zh.md
- agent/canvas.py
- agent/component/agent_with_tools.py
- agent/component/base.py
- agent/component/categorize.py
- agent/component/llm.py
- agent/component/message.py
- agent/tools/base.py
- agent/tools/retrieval.py
- api/apps/llm_app.py
- api/db/__init__.py
- api/db/db_models.py
- api/db/init_data.py
- api/db/services/dialog_service.py
- api/db/services/document_service.py
- api/db/services/file_service.py
- api/db/services/knowledgebase_service.py
- api/db/services/llm_service.py
- api/db/services/task_service.py
- api/db/services/user_service.py
- api/utils/file_utils.py
- common/mcp_tool_call_conn.py
- conf/llm_factories.json
- deepdoc/parser/ppt_parser.py
- deepdoc/vision/__init__.py
- deepdoc/vision/operators.py
- deepdoc/vision/postprocess.py
- docker/.env
- docker/README.md
- docs/guides/manage_files.md
- docs/quickstart.mdx
- rag/llm/__init__.py
- rag/llm/chat_model.py
- rag/llm/cv_model.py
- rag/llm/embedding_model.py
- rag/llm/rerank_model.py
- rag/llm/sequence2txt_model.py
- rag/llm/tts_model.py
- rag/nlp/query.py
- rag/nlp/rag_tokenizer.py
- rag/nlp/search.py
- rag/nlp/term_weight.py
- rag/prompts/generator.py
- rag/raptor.py
- rag/svr/task_executor.py
- web/src/locales/de.ts
- web/src/locales/en.ts
- web/src/locales/es.ts
- web/src/locales/fr.ts
- web/src/locales/id.ts
- web/src/locales/it.ts
- web/src/locales/ja.ts
- web/src/locales/pt-br.ts
- web/src/locales/ru.ts
- web/src/locales/vi.ts
- web/src/locales/zh-traditional.ts
- web/src/locales/zh.ts
- web/src/pages/user-setting/setting-model/constant.ts

This page provides definitions and technical pointers for codebase-specific terms, domain concepts, and architectural jargon used within RAGFlow.

## Purpose and Scope

The RAGFlow glossary serves as a reference for onboarding engineers to understand the specific nomenclature used in the Python backend, the Go server, and the React frontend. It bridges high-level RAG (Retrieval-Augmented Generation) concepts with their specific implementations in the `rag/`, `api/`, and `agent/` directories.

## Core Domain Terms

### RAG (Retrieval-Augmented Generation)

The core architectural pattern of the system. It involves retrieving relevant document chunks from a data store to provide context for a Large Language Model (LLM) to generate grounded responses [README.md75-78](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L75-L78)

### Dataset (Knowledge Base)

A logical collection of documents belonging to a tenant. In the code, this is often referred to as `Knowledgebase` or `KB`.

- Code Entity: KnowledgebaseService api/db/services/knowledgebase_service.py34
- Database Table: knowledgebase api/db/db_models.py31
- Localization: Referred to as "Dataset" in English UI web/src/locales/en.ts110 and "知识库" in Chinese UI web/src/locales/zh.ts94

### Chunking (Parsing)

The process of breaking down a document into smaller, searchable units. RAGFlow uses "Template-based chunking" where different parsers are applied based on document structure [README.md120-123](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L120-L123)

- Code Entity: ParserType common/constants.py76
- Implementation: The FACTORY in task_executor.py maps parser types (e.g., naive, paper, book, qa) to specific implementation modules rag/svr/task_executor.py114-131

### Embedding

The process of converting text chunks into numerical vectors. These vectors are stored in a document store for similarity search.

- Code Entity: Base embedding class rag/llm/embedding_model.py138-146
- Standard Ceiling: Inputs are truncated to DEFAULT_MAX_TOKENS (8192) to avoid provider rejection rag/llm/embedding_model.py40-43

### Reranking

A second-stage retrieval process where a specialized model (Reranker) scores the relevance of retrieved chunks against the query to improve precision.

- Code Entity: Base rerank class rag/llm/rerank_model.py28

## Codebase-Specific Jargon

### Dialog (Assistant)

A configuration object that defines how a chat behaves, including which datasets it searches, which LLM it uses, and its system prompt.

- Code Entity: DialogService api/db/services/dialog_service.py138-140
- Database Table: Dialog api/db/db_models.py31

### Session (Conversation)

A specific instance of an interaction between a user and a Dialog or Agent. It maintains the message history.

- Code Entity: ConversationService api/db/services/conversation_service.py34

### Tenant

The top-level entity for multi-tenancy. All datasets, documents, and LLM configurations are scoped to a `tenant_id`.

- Model Wrapper: LLMBundle api/db/services/llm_service.py79
- Model Config: get_model_config_from_provider_instance retrieves tenant-specific LLM settings api/db/joint_services/tenant_model_service.py82

### Task Executor

The background worker process that handles heavy lifting like document parsing, embedding, and GraphRAG indexing. It consumes tasks from Redis Streams.

- Key Function: set_progress() updates task status and handles cancellation rag/svr/task_executor.py165-197
- Task Types: Maps identifiers like dataflow, raptor, memory to PipelineTaskType rag/svr/task_executor.py133-139

### Memory

A feature for AI agents to persist information across sessions, categorized into types like raw, semantic, episodic, and procedural.

- Code Entity: handle_save_to_memory_task api/db/joint_services/memory_message_service.py44
- Task Type: PipelineTaskType.MEMORY rag/svr/task_executor.py138

## System Space Mapping

### Natural Language to Code Entity Mapping

This diagram shows how user-facing concepts in the UI/Docs map to specific classes and database models in the backend.

**Concept Mapping Diagram**

```
CodeEntitySpace(Backend)

NaturalLanguageSpace(UI/Docs)

'Dataset/KnowledgeBase'

'Chat/Assistant/Dialog'

'Agent/Workflow/Canvas'

'Document/File'

KnowledgebaseService

DialogService

Canvas

DocumentService

Table:knowledgebase

Table:dialog

Table:canvas

Table:document
```

Sources: [api/db/db_models.py25-31](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/db_models.py#L25-L31) [api/db/services/knowledgebase_service.py34](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/knowledgebase_service.py#L34-L34) [api/db/services/dialog_service.py138-140](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/dialog_service.py#L138-L140) [agent/canvas.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L29-L29) [api/db/services/document_service.py77](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L77-L77)

### Task Execution Data Flow

Mapping the lifecycle of a document upload from the "Natural Language Space" (User Action) to the "Code Entity Space" (Background Processing).

**Task Lifecycle Diagram**

```
"rag/app/*.py(Parsers)"
"rag/svr/task_executor.py"
"RedisStream(SVR_CONSUMER_GROUP_NAME)"
"api/apps/llm_app.py"
"User(Web/SDK)"
"rag/app/*.py(Parsers)"
"rag/svr/task_executor.py"
"RedisStream(SVR_CONSUMER_GROUP_NAME)"
"api/apps/llm_app.py"
"User(Web/SDK)"
"task_executor.py:TaskManager"
"POST /api/v1/datasets/{kb_id}/documents"
"DocumentService.save()"
"TaskService.create_task()"
"FetchRedisMsg"
"set_progress(task_id,msg='Processing...')"
"FACTORY[parser_type].chunk()"
"ListOfChunks"
"Embedding & Indexing"
"TaskService.update_progress(prog=100)"
```

Sources: [rag/svr/task_executor.py114-131](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L114-L131) [rag/svr/task_executor.py165-197](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L165-L197) [api/db/services/document_service.py77](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/document_service.py#L77-L77) [api/db/services/task_service.py79-81](https://github.com/infiniflow/ragflow/blob/d32e05d5/api/db/services/task_service.py#L79-L81) [common/constants.py103](https://github.com/infiniflow/ragflow/blob/d32e05d5/common/constants.py#L103-L103)

## Technical Abbreviations

| Abbreviation | Full Name | Description | Code Pointer |
| --- | --- | --- | --- |
| **DSL** | Domain Specific Language | The JSON representation of an agent workflow graph. | [agent/canvas.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L29-L29) |
| **MCP** | Model Context Protocol | Protocol for connecting LLMs to external data and tools. | [web/src/locales/en.ts73-79](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L73-L79) |
| **LITELLM** | LiteLLM | Library used to provide a unified interface for multiple LLM providers. | [rag/llm/chat_model.py29-36](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L29-L36) |
| **RAPTOR** | Recursive Abstractive Processing... | A tree-organized retrieval method for long documents. | [rag/svr/task_executor.py48-56](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L48-L56) |
| **OCR** | Optical Character Recognition | Used in document parsing to extract text from images or PDFs. | [README.md132](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L132-L132) |
| **KG** | Knowledge Graph | Graph-based parsing and retrieval strategy. | [rag/svr/task_executor.py129](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L129-L129) |
| **SSE** | Server-Sent Events | Protocol used for streaming chat completions. | [rag/svr/task_executor.py155-162](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L155-L162) |

Sources: [web/src/locales/en.ts73-79](https://github.com/infiniflow/ragflow/blob/d32e05d5/web/src/locales/en.ts#L73-L79) [rag/llm/chat_model.py29-36](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/llm/chat_model.py#L29-L36) [rag/svr/task_executor.py48-139](https://github.com/infiniflow/ragflow/blob/d32e05d5/rag/svr/task_executor.py#L48-L139) [agent/canvas.py29](https://github.com/infiniflow/ragflow/blob/d32e05d5/agent/canvas.py#L29-L29) [README.md132](https://github.com/infiniflow/ragflow/blob/d32e05d5/README.md?plain=1#L132-L132)


---
