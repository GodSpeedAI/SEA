

# **Data Schemas for Sentient Enterprise Architecture (SEA) - Version 4.0**

This document provides detailed data schemas for the core entities within the Sentient Enterprise Architecture (SEA), with a particular focus on the Cognitive Extension Layer and its Cognitive Artifact DSL (CADSL). These schemas are designed to be machine-readable and support the generative capabilities of the SEA, ensuring consistency across the Semantic Core, Knowledge Layer, and Computational Layer.

## **1. Core SEA Entities (Semantic Core & Knowledge Layer)**

These schemas define the fundamental building blocks of the SEA, derived from the MUDM and formalized by SBVR. They are intended to be represented in a Knowledge Graph (e.g., using RDF/OWL) and also inform the structure of relational databases or other data stores in the Computational Layer.

### **1.1. Entity Schema**

Represents any addressable subject with unique identity in the business domain. This is the foundational concept for all other domain objects.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Entity",
  "description": "A foundational concept representing any addressable subject with unique identity in the business domain.",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier for the entity (e.g., UUID, business key)."
    },
    "name": {
      "type": "string",
      "description": "Human-readable name of the entity."
    },
    "type": {
      "type": "string",
      "description": "Categorization of the entity (e.g., 'Customer', 'Product', 'Organization')."
    },
    "description": {
      "type": "string",
      "description": "Detailed description of the entity."
    },
    "attributes": {
      "type": "object",
      "description": "Key-value pairs for additional entity-specific properties.",
      "additionalProperties": true
    },
    "createdAt": {
      "type": "string",
      "format": "date-time",
      "description": "Timestamp when the entity was created."
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Timestamp when the entity was last updated."
    }
  },
  "required": ["id", "name", "type"]
}
```

### **1.2. Resource Schema**

Represents anything consumed, produced, exchanged, or utilized within the business processes.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Resource",
  "description": "Anything consumed, produced, exchanged, or utilized within business processes.",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier for the resource."
    },
    "name": {
      "type": "string",
      "description": "Human-readable name of the resource."
    },
    "type": {
      "type": "string",
      "description": "Categorization of the resource (e.g., 'RawMaterial', 'FinishedProduct', 'Currency', 'Skill')."
    },
    "unitOfMeasure": {
      "type": "string",
      "description": "The standard unit of measure for this resource (e.g., 'kg', 'piece', 'USD', 'hour')."
    },
    "description": {
      "type": "string",
      "description": "Detailed description of the resource."
    },
    "attributes": {
      "type": "object",
      "description": "Key-value pairs for additional resource-specific properties.",
      "additionalProperties": true
    }
  },
  "required": ["id", "name", "type", "unitOfMeasure"]
}
```

### **1.3. Flow Schema**

Represents the dynamic transfer or transformation of resources between entities or states.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Flow",
  "description": "The dynamic transfer or transformation of resources between entities or states.",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier for the flow."
    },
    "type": {
      "type": "string",
      "description": "Categorization of the flow (e.g., 'Production', 'Sales', 'Transfer', 'InformationExchange')."
    },
    "resourceId": {
      "type": "string",
      "description": "ID of the resource involved in the flow."
    },
    "quantity": {
      "type": "number",
      "description": "The amount of resource involved in the flow."
    },
    "sourceEntityId": {
      "type": "string",
      "description": "ID of the originating entity."
    },
    "destinationEntityId": {
      "type": "string",
      "description": "ID of the destination entity."
    },
    "initiatedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Timestamp when the flow was initiated."
    },
    "completedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Timestamp when the flow was completed."
    },
    "status": {
      "type": "string",
      "enum": ["initiated", "in-progress", "completed", "cancelled"],
      "description": "Current status of the flow."
    },
    "attributes": {
      "type": "object",
      "description": "Key-value pairs for additional flow-specific properties.",
      "additionalProperties": true
    }
  },
  "required": ["id", "type", "resourceId", "quantity", "sourceEntityId", "destinationEntityId", "initiatedAt"]
}
```

### **1.4. Instance Schema**

Represents a physical or logical instance of a Resource, enabling traceability.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Instance",
  "description": "A physical or logical instance of a Resource, enabling traceability.",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier for the instance (e.g., serial number, batch ID)."
    },
    "resourceId": {
      "type": "string",
      "description": "ID of the resource this is an instance of."
    },
    "currentLocationEntityId": {
      "type": "string",
      "description": "ID of the entity where this instance is currently located."
    },
    "status": {
      "type": "string",
      "description": "Current status of the instance (e.g., 'in-stock', 'in-transit', 'consumed')."
    },
    "attributes": {
      "type": "object",
      "description": "Key-value pairs for additional instance-specific properties.",
      "additionalProperties": true
    }
  },
  "required": ["id", "resourceId", "currentLocationEntityId"]
}
```

### **1.5. Policy Schema**

Defines rules, conditions, and access mechanisms governing entities, resources, and flows.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Policy",
  "description": "Defines rules, conditions, and access mechanisms governing entities, resources, and flows.",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier for the policy."
    },
    "name": {
      "type": "string",
      "description": "Human-readable name of the policy."
    },
    "description": {
      "type": "string",
      "description": "Detailed description of the policy, often expressed in SBVR."
    },
    "type": {
      "type": "string",
      "description": "Categorization of the policy (e.g., 'PricingRule', 'AccessControl', 'ComplianceRule')."
    },
    "appliesTo": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "entityId": {"type": "string"},
          "resourceId": {"type": "string"},
          "flowId": {"type": "string"}
        },
        "minProperties": 1
      },
      "description": "List of entities, resources, or flows this policy applies to."
    },
    "ruleDefinition": {
      "type": "string",
      "description": "The formal definition of the rule, ideally in SBVR or a similar declarative language."
    },
    "isActive": {
      "type": "boolean",
      "description": "Indicates if the policy is currently active."
    }
  },
  "required": ["id", "name", "type", "ruleDefinition", "isActive"]
}
```

## **2. Cognitive Artifact DSL (CADSL) Schemas**

These schemas define the structure of cognitive artifacts and their constituent elements, enabling their dynamic generation and manipulation by the Cognitive Extension Layer.

### **2.1. CadslElement Base Schema**

All CADSL elements inherit from this base schema.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CadslElement",
  "description": "Base schema for all Cognitive Artifact DSL elements.",
  "type": "object",
  "properties": {
    "type": {
      "type": "string",
      "description": "The specific type of CADSL element (e.g., 'TextField', 'Button', 'Section')."
    },
    "id": {
      "type": "string",
      "description": "Optional unique identifier for the element within an artifact."
    },
    "metadata": {
      "type": "object",
      "description": "Optional key-value pairs for element-specific metadata (e.g., 'color', 'size', 'font').",
      "additionalProperties": true
    }
  },
  "required": ["type"]
}
```

### **2.2. Specific CADSL Element Schemas**

Each element type defined in the CADSL specification (`docs/cognitive-artifact-dsl-spec.md`) will have its own schema, extending `CadslElement`.

#### **2.2.1. TextField Schema**

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "TextField",
  "description": "A single-line text input field.",
  "type": "object",
  "allOf": [{"ref": "#/definitions/CadslElement"}],
  "properties": {
    "type": {"const": "TextField"},
    "label": {"type": "string"},
    "placeholder": {"type": "string"},
    "value": {"type": "string"},
    "isEditable": {"type": "boolean", "default": true}
  },
  "required": ["label"]
}
```

#### **2.2.2. Checkbox Schema**

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Checkbox",
  "description": "A binary option checkbox.",
  "type": "object",
  "allOf": [{"ref": "#/definitions/CadslElement"}],
  "properties": {
    "type": {"const": "Checkbox"},
    "label": {"type": "string"},
    "checked": {"type": "boolean", "default": false},
    "isEditable": {"type": "boolean", "default": true}
  },
  "required": ["label"]
}
```

#### **2.2.3. Section Schema**

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Section",
  "description": "A container for grouping other CADSL elements.",
  "type": "object",
  "allOf": [{"ref": "#/definitions/CadslElement"}],
  "properties": {
    "type": {"const": "Section"},
    "title": {"type": "string"},
    "children": {
      "type": "array",
      "items": {"ref": "#/definitions/CadslElement"}
    }
  },
  "required": ["children"]
}
```

*(Note: Full schemas for all CADSL elements would be provided here, following the structure above. The `#/definitions/CadslElement` reference would point to the base schema defined earlier, or a separate file for definitions.)*

### **2.3. Cognitive Artifact Schema**

Represents a complete cognitive artifact, composed of a canvas of CADSL elements.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CognitiveArtifact",
  "description": "A complete cognitive artifact, composed of a canvas of CADSL elements.",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "description": "Unique identifier for the cognitive artifact."
    },
    "name": {
      "type": "string",
      "description": "Human-readable name of the artifact."
    },
    "type": {
      "type": "string",
      "description": "Categorization of the artifact (e.g., 'ProjectPlanner', 'MindMap', 'Checklist')."
    },
    "canvas": {
      "type": "array",
      "items": {"ref": "#/definitions/CadslElement"},
      "description": "The root-level elements that constitute the artifact's content."
    },
    "createdAt": {
      "type": "string",
      "format": "date-time"
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time"
    },
    "ownerId": {
      "type": "string",
      "description": "ID of the user who owns/created the artifact."
    },
    "isReadOnly": {
      "type": "boolean",
      "default": false,
      "description": "If true, the artifact cannot be modified by the user."
    }
  },
  "required": ["id", "name", "type", "canvas", "ownerId"]
}
```

## **3. CALM Architecture Model Schema**

This schema is based on the FINOS CALM specification, extended with SEA-specific metadata where appropriate. It defines the structure for `my-system.arch.json` files.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CALM Architecture Model (SEA Extension)",
  "description": "A CALM-compliant architecture definition for SEA systems.",
  "type": "object",
  "properties": {
    "nodes": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "id": {"type": "string"},
          "name": {"type": "string"},
          "type": {"type": "string", "enum": ["service", "database", "queue", "external-api", "ui-component", "ai-agent", "cognitive-engine"]},
          "description": {"type": "string"},
          "metadata": {
            "type": "object",
            "properties": {
              "domain": {"type": "string", "description": "SEA Domain (e.g., Sales, Finance, CognitiveSupport)"},
              "team": {"type": "string"},
              "language": {"type": "string"},
              "deployment": {"type": "string"},
              "semanticCoreLink": {"type": "string", "description": "Link to relevant DSL/SBVR definition in Semantic Core"}
            },
            "additionalProperties": true
          },
          "controls": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "id": {"type": "string"},
                "description": {"type": "string"},
                "status": {"type": "string", "enum": ["compliant", "non-compliant", "waived"]}
              },
              "required": ["id", "description"]
            }
          }
        },
        "required": ["id", "name", "type"]
      }
    },
    "relationships": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "id": {"type": "string"},
          "source": {"type": "string"},
          "target": {"type": "string"},
          "type": {"type": "string", "enum": ["api-call", "message-queue", "reads", "writes", "reads-writes", "stream", "governs", "informs", "generates"]},
          "description": {"type": "string"},
          "metadata": {
            "type": "object",
            "properties": {
              "protocol": {"type": "string"},
              "encryption": {"type": "string"},
              "dataFlow": {"type": "string", "description": "Data classification of data flowing through this relationship (e.g., PII, Confidential)"}
            },
            "additionalProperties": true
          },
          "controls": {
            "type": "array",
            "items": {
              "type": "object",
              "properties": {
                "id": {"type": "string"},
                "description": {"type": "string"},
                "status": {"type": "string", "enum": ["compliant", "non-compliant", "waived"]}
              },
              "required": ["id", "description"]
            }
          }
        },
        "required": ["id", "source", "target", "type"]
      }
    }
  },
  "required": ["nodes", "relationships"]
}
```

## **4. AI Agent Configuration Schema (Prompt Management DSL)**

This schema defines the structure for configuring AI agents, particularly their prompt templates, context injection, and skill composition, analogous to LoRA/Adapter patterns.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "AIAgentConfiguration",
  "description": "Configuration schema for AI agents within the SEA Intelligence Layer.",
  "type": "object",
  "properties": {
    "agentId": {
      "type": "string",
      "description": "Unique identifier for the AI agent."
    },
    "name": {
      "type": "string",
      "description": "Human-readable name of the AI agent."
    },
    "role": {
      "type": "string",
      "description": "The primary role or function of the AI agent (e.g., 'CustomerSupport', 'CodeGenerator', 'ArtifactRecommender')."
    },
    "baseModel": {
      "type": "string",
      "description": "Identifier for the underlying large language model (e.g., 'GPT-4', 'Gemini-Pro')."
    },
    "promptTemplate": {
      "type": "string",
      "description": "The base prompt template for the agent, potentially with placeholders for context."
    },
    "contextSources": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "sourceType": {"type": "string", "enum": ["knowledgeGraph", "domainService", "userConversation", "cognitiveArtifact"]},
          "query": {"type": "string", "description": "Query to extract context from the source (e.g., Cypher query, API endpoint, semantic search)."},
          "injectionPoint": {"type": "string", "description": "Placeholder in promptTemplate where this context is injected."}
        },
        "required": ["sourceType", "query", "injectionPoint"]
      },
      "description": "Defines where and how context is retrieved and injected into the prompt."
    },
    "skills": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "skillId": {"type": "string"},
          "description": {"type": "string"},
          "toolCallDefinition": {
            "type": "object",
            "description": "Definition of the tool/function call associated with this skill (e.g., OpenAPI spec, function signature).",
            "additionalProperties": true
          }
        },
        "required": ["skillId", "toolCallDefinition"]
      },
      "description": "List of tools or functions the AI agent can invoke."
    },
    "behaviorAdapters": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "adapterType": {"type": "string", "enum": ["LoRA", "PrefixTuning", "AdapterLayer", "CustomLogic"]},
          "configuration": {
            "type": "object",
            "description": "Specific configuration for the adapter (e.g., LoRA rank, adapter weights).",
            "additionalProperties": true
          }
        },
        "required": ["adapterType", "configuration"]
      },
      "description": "Defines modular adjustments to the agent's base behavior or model weights."
    }
  },
  "required": ["agentId", "name", "role", "baseModel", "promptTemplate"]
}
```

## **5. User Interaction & Feedback Schema**

This schema captures user interactions with cognitive artifacts and feedback on AI recommendations, crucial for the continuous learning and refinement of the Cognitive Extension Layer.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "UserInteractionFeedback",
  "description": "Records user interactions with cognitive artifacts and feedback on AI recommendations.",
  "type": "object",
  "properties": {
    "interactionId": {
      "type": "string",
      "description": "Unique identifier for the interaction event."
    },
    "userId": {
      "type": "string",
      "description": "ID of the user performing the interaction."
    },
    "timestamp": {
      "type": "string",
      "format": "date-time",
      "description": "Time of the interaction."
    },
    "eventType": {
      "type": "string",
      "enum": ["artifact_generated", "artifact_recommended", "artifact_accepted", "artifact_rejected", "artifact_modified", "element_interacted"],
      "description": "Type of event recorded."
    },
    "artifactId": {
      "type": "string",
      "description": "ID of the cognitive artifact involved (if any)."
    },
    "elementId": {
      "type": "string",
      "description": "ID of the specific CADSL element interacted with (if any)."
    },
    "contextSnapshot": {
      "type": "object",
      "description": "Snapshot of the conversational context at the time of interaction.",
      "additionalProperties": true
    },
    "feedback": {
      "type": "object",
      "properties": {
        "rating": {"type": "integer", "minimum": 1, "maximum": 5},
        "comment": {"type": "string"},
        "reason": {"type": "string"}
      },
      "description": "Optional user feedback on the artifact or recommendation."
    }
  },
  "required": ["interactionId", "userId", "timestamp", "eventType"]
}
```

## **6. Automated Documentation Generation (ADG) Framework Schemas**

These schemas support the ADG Framework, enabling automated, context-aware generation of comprehensive project documentation.

### **6.1. DocumentationRequest Schema**

Represents a request to generate documentation for a specific project.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "DocumentationRequest",
  "description": "A request to generate documentation for a specific project.",
  "type": "object",
  "properties": {
    "requestId": {
      "type": "string",
      "description": "Unique identifier for the documentation request."
    },
    "projectId": {
      "type": "string",
      "description": "Unique identifier for the project to be documented."
    },
    "projectLocation": {
      "type": "string",
      "description": "File path or repository URL of the project."
    },
    "documentationType": {
      "type": "string",
      "enum": ["README", "API_DOCS", "ARCHITECTURE_OVERVIEW", "USER_GUIDE", "COMPREHENSIVE"],
      "description": "Type of documentation to generate."
    },
    "configuration": {
      "type": "object",
      "properties": {
        "includeCodeExamples": {"type": "boolean"},
        "includeDiagrams": {"type": "boolean"},
        "includeAPIReference": {"type": "boolean"},
        "targetAudience": {"type": "string", "enum": ["developer", "architect", "end-user", "mixed"]},
        "templateId": {"type": "string", "description": "ID of documentation template to use."}
      },
      "description": "Configuration parameters for documentation generation."
    },
    "requestedBy": {
      "type": "string",
      "description": "User ID of the requester."
    },
    "requestedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Timestamp when the request was made."
    }
  },
  "required": ["requestId", "projectId", "projectLocation", "documentationType", "requestedBy", "requestedAt"]
}
```

### **6.2. DocumentationArtifact Schema**

Represents the generated documentation artifact with its content and metadata.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "DocumentationArtifact",
  "description": "Generated documentation artifact with content and metadata.",
  "type": "object",
  "properties": {
    "artifactId": {
      "type": "string",
      "description": "Unique identifier for the documentation artifact."
    },
    "requestId": {
      "type": "string",
      "description": "ID of the originating documentation request."
    },
    "projectId": {
      "type": "string",
      "description": "ID of the documented project."
    },
    "documentationType": {
      "type": "string",
      "enum": ["README", "API_DOCS", "ARCHITECTURE_OVERVIEW", "USER_GUIDE", "COMPREHENSIVE"],
      "description": "Type of documentation generated."
    },
    "content": {
      "type": "object",
      "properties": {
        "cadslDefinition": {
          "type": "object",
          "description": "CADSL representation of the documentation (if applicable)."
        },
        "markdown": {
          "type": "string",
          "description": "Rendered Markdown content of the documentation."
        },
        "html": {
          "type": "string",
          "description": "Rendered HTML content of the documentation (if generated)."
        }
      },
      "description": "The actual documentation content in various formats."
    },
    "metadata": {
      "type": "object",
      "properties": {
        "title": {"type": "string"},
        "version": {"type": "string"},
        "generatedAt": {"type": "string", "format": "date-time"},
        "generatedBy": {"type": "string", "description": "Service or user that generated the documentation."},
        "projectVersion": {"type": "string"},
        "semanticCoverage": {"type": "number", "minimum": 0, "maximum": 100, "description": "Percentage of semantic concepts covered."},
        "architecturalCompliance": {"type": "boolean", "description": "Whether the documented architecture is CALM-compliant."}
      },
      "description": "Metadata about the generated documentation."
    },
    "validationStatus": {
      "type": "string",
      "enum": ["valid", "warnings", "errors"],
      "description": "Validation status of the generated documentation."
    },
    "validationMessages": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "level": {"type": "string", "enum": ["info", "warning", "error"]},
          "message": {"type": "string"},
          "location": {"type": "string", "description": "Location in documentation where issue occurs."}
        }
      },
      "description": "Validation messages, warnings, or errors."
    }
  },
  "required": ["artifactId", "requestId", "projectId", "documentationType", "content", "metadata"]
}
```

### **6.3. ProjectContext Schema**

Represents comprehensive contextual information extracted from a project for documentation generation.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ProjectContext",
  "description": "Comprehensive contextual information extracted from a project.",
  "type": "object",
  "properties": {
    "projectId": {
      "type": "string",
      "description": "Unique identifier for the project."
    },
    "projectName": {
      "type": "string",
      "description": "Name of the project."
    },
    "projectDescription": {
      "type": "string",
      "description": "High-level description of the project."
    },
    "technicalStack": {
      "type": "object",
      "properties": {
        "languages": {"type": "array", "items": {"type": "string"}},
        "frameworks": {"type": "array", "items": {"type": "string"}},
        "databases": {"type": "array", "items": {"type": "string"}},
        "infrastructure": {"type": "array", "items": {"type": "string"}}
      },
      "description": "Technical stack information."
    },
    "semanticContext": {
      "type": "object",
      "properties": {
        "dslDefinitions": {"type": "array", "items": {"type": "object"}},
        "sbvrRules": {"type": "array", "items": {"type": "object"}},
        "businessConcepts": {"type": "array", "items": {"type": "object"}},
        "knowledgeGraphEntities": {"type": "array", "items": {"type": "object"}}
      },
      "description": "Semantic information from Semantic Core and Knowledge Graph."
    },
    "architecturalContext": {
      "type": "object",
      "properties": {
        "calmDefinitions": {"type": "array", "items": {"type": "object"}},
        "components": {"type": "array", "items": {"type": "object"}},
        "relationships": {"type": "array", "items": {"type": "object"}},
        "controls": {"type": "array", "items": {"type": "object"}},
        "complianceStatus": {"type": "string", "enum": ["compliant", "non-compliant", "partially-compliant"]}
      },
      "description": "Architectural information from CALM definitions."
    },
    "codebaseMetrics": {
      "type": "object",
      "properties": {
        "linesOfCode": {"type": "integer"},
        "numberOfFiles": {"type": "integer"},
        "numberOfClasses": {"type": "integer"},
        "numberOfFunctions": {"type": "integer"},
        "testCoverage": {"type": "number", "minimum": 0, "maximum": 100}
      },
      "description": "Metrics about the codebase."
    },
    "dependencies": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "name": {"type": "string"},
          "version": {"type": "string"},
          "type": {"type": "string", "enum": ["direct", "transitive"]}
        }
      },
      "description": "Project dependencies."
    },
    "apiEndpoints": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "path": {"type": "string"},
          "method": {"type": "string", "enum": ["GET", "POST", "PUT", "DELETE", "PATCH"]},
          "description": {"type": "string"},
          "requestSchema": {"type": "object"},
          "responseSchema": {"type": "object"}
        }
      },
      "description": "API endpoints exposed by the project."
    },
    "extractedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Timestamp when context was extracted."
    }
  },
  "required": ["projectId", "projectName", "technicalStack", "extractedAt"]
}
```

