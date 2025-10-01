# **CALM-based DSL Specifications for Sentient Enterprise Architecture (SEA)**

Within the Sentient Enterprise Architecture (SEA), the **Architectural Layer** is formally defined and governed by a DSL built upon the **Common Architecture Language Model (CALM)**. This CALM-based DSL serves as the machine-readable blueprint for the enterprise's systems, ensuring consistency, enabling automated validation, and facilitating the generation of architectural artifacts. This specification outlines the structure and key elements of this DSL, leveraging the FINOS CALM standard.

## **1. Core Principles of the CALM-based DSL**

The CALM-based DSL adheres to the following principles:

*   **Architecture-as-Code:** Architectural definitions are treated as code, stored in version control, and subject to the same development lifecycle as software (versioning, diffing, code review).
*   **Declarative:** Architects declare the desired state of the architecture (what it should be), rather than imperative steps (how to build it).
*   **Machine-Readable:** The DSL is expressed in JSON (or YAML, as a superset of JSON), allowing for programmatic parsing, validation, and transformation by automated tools.
*   **Extensible:** The core CALM schema can be extended with custom metadata and controls to meet specific organizational or regulatory requirements.
*   **Generative:** The DSL serves as an input for generating various architectural outputs, including diagrams, documentation, and even infrastructure-as-code stubs.

## **2. Structure of the CALM-based DSL**

The CALM-based DSL document (e.g., `my-system.arch.json` or `my-system.arch.yaml`) is a JSON object conforming to the FINOS CALM meta-schema. Its primary components are `nodes`, `relationships`, `metadata`, and `controls`.

### **2.1. Nodes (Components)**

`Nodes` represent the fundamental building blocks of the architecture. In SEA, these align with logical components, services, data stores, external systems, or even organizational units that participate in the architecture.

**Example Node Definition:**

```json
{
  "nodes": [
    {
      "id": "order-service",
      "name": "Order Processing Service",
      "type": "service",
      "description": "Microservice responsible for handling customer orders.",
      "metadata": {
        "domain": "Sales",
        "team": "Alpha Squad",
        "language": "Python",
        "deployment": "Kubernetes"
      },
      "controls": [
        {
          "id": "data-encryption-at-rest",
          "description": "All persistent data must be encrypted at rest.",
          "status": "compliant"
        }
      ]
    },
    {
      "id": "customer-db",
      "name": "Customer Database",
      "type": "database",
      "description": "PostgreSQL database storing customer profiles.",
      "metadata": {
        "dataClassification": "PII",
        "owner": "Data Governance Team"
      }
    }
  ]
}
```

**Key Attributes for Nodes:**

*   `id` (string, required): A unique identifier for the node.
*   `name` (string, required): A human-readable name.
*   `type` (string, required): Categorization of the node (e.g., `service`, `database`, `queue`, `external-api`, `ui-component`). This can be extended with custom types relevant to the enterprise.
*   `description` (string, optional): Detailed explanation of the node's purpose.
*   `metadata` (object, optional): A flexible key-value store for attaching additional, custom attributes (e.g., `domain`, `team`, `technologyStack`, `sla`). This is crucial for embedding organizational context and for compliance checks.
*   `controls` (array of objects, optional): Specifies security, policy, or regulatory requirements applicable to this node. Each control can have an `id`, `description`, and `status`.

### **2.2. Relationships (Interactions)**

`Relationships` define how `nodes` interact with each other. These represent data flows, API calls, message exchanges, or any form of communication between architectural components.

**Example Relationship Definition:**

```json
{
  "relationships": [
    {
      "id": "order-service-to-customer-db",
      "source": "order-service",
      "target": "customer-db",
      "type": "reads-writes",
      "description": "Order service reads and writes customer data.",
      "metadata": {
        "protocol": "JDBC",
        "encryption": "TLS 1.2"
      },
      "controls": [
        {
          "id": "access-control-least-privilege",
          "description": "Database access must adhere to least privilege principle.",
          "status": "compliant"
        }
      ]
    },
    {
      "id": "order-service-to-payment-gateway",
      "source": "order-service",
      "target": "external-payment-gateway",
      "type": "api-call",
      "description": "Order service invokes external payment processing API.",
      "metadata": {
        "authentication": "OAuth2",
        "dataFlow": "PII"
      }
    }
  ]
}
```

**Key Attributes for Relationships:**

*   `id` (string, required): A unique identifier for the relationship.
*   `source` (string, required): The `id` of the originating node.
*   `target` (string, required): The `id` of the destination node.
*   `type` (string, required): Categorization of the interaction (e.g., `api-call`, `message-queue`, `reads`, `writes`, `reads-writes`, `stream`). Custom types can be defined.
*   `description` (string, optional): Detailed explanation of the interaction.
*   `metadata` (object, optional): Custom attributes relevant to the interaction (e.g., `protocol`, `encryption`, `dataFlow`, `latencySLA`).
*   `controls` (array of objects, optional): Specifies security or policy requirements for this interaction (e.g., `data-in-transit-encryption`).

### **2.3. Metadata and Controls**

CALM emphasizes **extensibility** through `metadata` and `controls`. These allow organizations to embed their specific governance, compliance, and operational requirements directly into the architectural model.

*   **Metadata:** Arbitrary key-value pairs that can be attached to any node or relationship. This is critical for tagging sensitive data classes (`dataClassification: PII`), assigning ownership (`owner: TeamX`), or indicating technology choices (`technologyStack: [Spring Boot, Kafka]`).
*   **Controls:** Formal statements of security or policy requirements. These can be tied to nodes or relationships and are used by the CALM CLI for automated validation. For example, a control could require all network connections to use TLS, and the `calm validate` command would flag any non-compliant relationships.

## **3. Integration with SEA Framework**

This CALM-based DSL integrates synergistically with the other layers of the SEA:

*   **Semantic Core (DSL/SBVR):** The CALM DSL provides the architectural context for the business concepts defined in the Semantic Core. For instance, a `service` node in CALM might implement `Policies` and handle `Flows` defined in the core DSL. Metadata in CALM can link architectural components directly to business capabilities or domain contexts defined in SBVR.
*   **Computational Layer:** The CALM model serves as a blueprint for AI-driven code generation. Generators can use the CALM definition to scaffold service boundaries, API interfaces, and integration points, ensuring that the generated code adheres to the defined architecture. CALM can also generate Infrastructure-as-Code (IaC) stubs (e.g., Terraform, Kubernetes manifests) that align with the architectural intent [IncorporatingCALMintoYourArchitectureStack.pdf].
*   **Knowledge Layer:** The CALM model can be transformed into a graph representation within the Knowledge Layer, allowing for semantic queries about the architecture itself. This enables reasoning about architectural dependencies, impact analysis of changes, and identification of architectural patterns.
*   **Intelligence Layer:** AI agents can leverage the CALM model to understand the system's structure and constraints. For example, an AI agent tasked with optimizing a service could consult the CALM model to understand its dependencies and the controls it must adhere to. The CALM model can also govern the deployment and interaction patterns of AI agents themselves.

## **4. Tooling Ecosystem (FINOS CALM CLI)**

The FINOS CALM project provides a Command Line Interface (CLI) that is central to working with the CALM-based DSL:

*   `calm validate`: Validates an architecture JSON/YAML file against the CALM schema and against predefined architectural patterns and controls. This is crucial for integrating architectural governance into CI/CD pipelines, preventing architectural drift [IncorporatingCALMintoYourArchitectureStack.pdf].
*   `calm docify`: Generates comprehensive documentation (e.g., a Docusaurus site) from the CALM model, ensuring that documentation is always in sync with the actual architecture [IncorporatingCALMintoYourArchitectureStack.pdf].
*   `calm generate`: Scaffolds new architecture files from predefined patterns, promoting reuse and adherence to organizational standards [IncorporatingCALMintoYourArchitectureStack.pdf].
*   `CALM Hub`: A UI tool for visualizing CALM models into interactive diagrams, providing an always-accurate representation of the architecture [IncorporatingCALMintoYourArchitectureStack.pdf].

By adopting a CALM-based DSL, the Sentient Enterprise Architecture gains a powerful, formal mechanism for defining, governing, and evolving its technical landscape, ensuring that architectural intent is consistently translated into operational reality.
