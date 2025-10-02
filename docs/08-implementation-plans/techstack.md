# SEA 5.0 Technology Stack (Python + Kong)

## Core Runtime & Services

- **Python 3.11+** powers every backend service, including the Semantic Core, Knowledge Graph Service, Context Analyzer, Artifact Engine, Documentation Orchestrator, Project Context Analyzer, and all Domain Services.
- **FastAPI** provides the primary web framework for synchronous REST endpoints, async event handlers, and service-to-service communication. Shared dependencies include Pydantic models, Uvicorn/Gunicorn deployment targets, SQLAlchemy/asyncpg for relational access, and grpcio for optional gRPC interfaces.
- **CADSL Runtime** continues to run on **Node.js 20+** with React/TypeScript to render cognitive artifacts and support interactive UX workflows.
- **AI/ML Workloads** rely on PyTorch/TensorFlow for model authoring, backed by the Prompt Management DSL and adapter patterns (LoRA-style) described in the intelligence layer.

## API Gateway & Edge

- **Kong Gateway** (OSS or Enterprise) fronts all FastAPI services, enabling a polyglot edge layer. Plugins handle OAuth2/JWT validation, rate limiting, request transformation, logging, and blue/green routing. Declarative configuration is stored alongside CALM models to keep architecture-as-code in sync.
- Kong’s service/route definitions expose standardized REST and gRPC interfaces, while internal FastAPI services remain language-agnostic consumers behind the gateway.

## Data & Messaging

- **PostgreSQL 15+** persists operational data for the Semantic Core, DSL management, and AI agent configuration services.
- **Neo4j 5+** implements the knowledge graph with RDF/OWL/SHACL validation for semantic reasoning.
- **RabbitMQ** supports asynchronous event distribution, user feedback collection, and long-running workflow orchestration.

## Infrastructure & Operations

- **Docker** images wrap every FastAPI service; deployments target cloud-agnostic **Kubernetes** clusters (GKE, EKS, AKS) following CALM-governed topology definitions.
- **CI/CD** pipelines (GitHub Actions or GitLab CI) build container images, apply Kong configuration bundles, and execute blue/green or rolling releases.
- **Observability** combines Prometheus/Grafana metrics, ELK or Loki for structured JSON logs, and Jaeger for distributed tracing. Kong plugins emit edge telemetry to the same observability stack.

## Security & Configuration

- OAuth2/JWT, mTLS, and Kong plugin policies enforce authentication and authorization across FastAPI services.
- `.env` and Kubernetes secrets manage database credentials, Neo4j Bolt credentials, JWT signing keys, LLM provider keys, and Kong admin tokens.
- FastAPI services adopt shared security middleware (rate limiting adapters, CORS, request validation) to ensure consistent posture across the polyglot gateway boundary.

## Testing & Tooling

- **pytest**, **coverage.py**, and **mypy** underpin FastAPI service testing; **Jest** with **Istanbul/nyc** covers JavaScript/TypeScript components where applicable.
- **Docker Compose** spins up Kong, PostgreSQL, Neo4j, and RabbitMQ for integration tests; Pact-based contract tests verify Kong-exposed routes; Playwright/Cypress continue to validate the React frontend.
- Pre-commit hooks run Black, isort, flake8, ESLint, and markdownlint to keep the multi-language stack consistent.

This configuration keeps the SEA architecture polyglot-friendly while consolidating backend services on Python/FastAPI and delegating edge mediation to Kong.
