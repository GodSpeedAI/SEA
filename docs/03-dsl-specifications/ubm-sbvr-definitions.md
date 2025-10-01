    `As a <role>, I want <capability> so that <benefit>.`
5.  **Link to DSL Concepts.** Document which nodes, resources, movements, items, and paths the story addresses. This ensures traceability between user stories and domain model elements.

##### 2.4 Writing EARS Acceptance Criteria

1.  **Select Appropriate EARS Pattern.** Determine the type of requirement:
    *   **Ubiquitous**: Always-active behaviour (“The `<system>` shall `<response>`.”)
    *   **State‑Driven**: Behaviour active under certain conditions (“While `<precondition>`, the `<system>` shall `<response>`.”)
    *   **Event‑Driven**: Reaction to an event (“When `<trigger>`, the `<system>` shall `<response>`.”)
    *   **Optional Feature**: Conditional feature (“Where `<feature>` is included, the `<system>` shall `<response>`.”)
    *   **Unwanted Behavior**: Handling faults (“If `<trigger>` occurs, then the `<system>` shall `<response>`.”)
    *   **Complex**: Combination of state and event (“While `<preconditions>`, when `<trigger>`, the `<system>` shall `<response>`.”)[edu.qracorp.com](https://edu.qracorp.com/hubfs/EARSConfigurationPart3.pdf#:~:text=Ubiquitous%3A%20The%20,System%20shall%20enter%20Standby%20Mode).
2.  **Describe Preconditions, Triggers, and Responses.**
    *   **Preconditions** represent states of nodes or resources (e.g., “inventory is low”).
    *   **Triggers** are domain events (movements).
    *   **Responses** are system actions (usually other movements or state changes).
3.  **Write Testable Statements.** Each acceptance criterion should be verifiable. Avoid combining multiple behaviours; instead, write separate criteria for each scenario.
4.  **Link to User Story.** Each user story may have multiple acceptance criteria, each covering a different scenario or pattern.

#### 3\. Example Use Case: Online Bookstore

##### 3.1 Domain Modeling with UBM DSL

**Entities (Nodes).**

*   `Customer`: Node representing a shopper.
*   `Order`: Node representing the aggregate root for a purchase.
*   `Inventory`: Node representing the warehouse or stock.
*   `PaymentAccount`: Node representing a payment service.

**Resources.**

*   `Book`: Resource (value object) with attributes (ISBN, title, price).
*   `Money`: Resource representing currency units.

**Movements.**

*   `AddItemToCartMovement`: Customer adds a book to an order.
*   `PlaceOrderMovement`: Customer finalizes the order.
*   `ReserveInventoryMovement`: Inventory reserves copies of books.
*   `ProcessPaymentMovement`: PaymentAccount charges the customer.
*   `ShipOrderMovement`: Order is shipped.

**Items.**

*   `BookItem`: Individual physical copy of a book.

**Paths.**

*   `StandardShippingPath`: Defines default shipping cost and conditions.

##### 3.2 Mapping to DDD Components

*   **Entities and Aggregates:** `Order` aggregate root contains order line items (items). `Customer`, `Inventory`, `PaymentAccount` are entities.
*   **Value Objects:** `Book`, `Money`.
*   **Domain Events/Commands:** `PlaceOrderMovement` corresponds to `PlaceOrderCommand` and `OrderPlacedEvent`. `ReserveInventoryMovement` is a command executed within the `Inventory` context.
*   **Services:** `PaymentService` processes `ProcessPaymentMovement`.

##### 3.3 User Story Derivation

**Movement:** `PlaceOrderMovement` (Customer finalizes purchase).

**User Story:**

```
As a customer,
I want to place an order containing selected books,
so that I can purchase them for delivery.
```

**Traceability:**

*   _Actor:_ Customer (source node).
*   _Capability:_ Placing an order (movement).
*   _Benefit:_ Obtain books (resource) via shipment (movement).
*   _Affected Nodes:_ Customer, Order, Inventory, PaymentAccount.
*   _Affected Resources:_ Book, Money.
*   _Paths:_ StandardShippingPath.

##### 3.4 Acceptance Criteria Using EARS

1.  **Event‑Driven Requirement.**
    *   _Scenario:_ Customer places order.
    *   _EARS Template:_ When `<trigger>`, the `<system>` shall `<response>`.
    *   _Trigger:_ Customer confirms cart (PlaceOrderMovement command).
    *   _Response:_ System shall create an order record, reserve inventory, and request payment.
    *   _Statement:_
        ```
        When the customer confirms the cart,
        the Order Management System shall create an Order and reserve the required quantity in Inventory.
        ```
        (Covers order creation and reservation. Additional criterion covers payment.)
2.  **Event‑Driven Requirement (Payment).**
    *   _Trigger:_ Order created and reservation succeeded.
    *   _Response:_ System shall process payment.
    *   _Statement:_
        ```
        When an Order is created,
        the Payment Service shall process the customer’s payment for the total order amount.
        ```
3.  **State‑Driven Requirement (Inventory).**
    *   _Precondition:_ Inventory has sufficient stock.
    *   _Response:_ Reserve the quantity.
    *   _Statement:_
        ```
        While sufficient inventory exists for each Book in the Order,
        the Inventory System shall reserve the quantity of each BookItem for that Order.
        ```
        If inventory is insufficient, the order should fail (an unwanted behavior requirement can cover this).
4.  **Unwanted Behavior Requirement (Out of Stock).**
    *   _Trigger:_ Inventory has insufficient stock for any Book.
    *   _Response:_ System shall notify customer and prevent order.
    *   _Statement:_
        ```
        If the Inventory System detects insufficient stock for any Book in the Order,
        then the Order Management System shall notify the customer that the book is unavailable and halt the ordering process.
        ```
5.  **Ubiquitous Requirement (Persistent Logging).**
    *   _Always active behaviour._
    *   _Statement:_
        ```
        The Order Management System shall log the date, time, and details of every Order placed.
        ```
6.  **Optional Feature Requirement (Gift Wrapping).**
    *   _Feature:_ Gift wrapping available.
    *   _Statement:_
        ```
        Where the gift wrapping feature is selected,
        the Order Management System shall add gift wrapping charges and label the Order as requiring gift packaging.
        ```

Each of these acceptance criteria is testable and linked back to the domain model. Together they comprehensively define the behaviour required to satisfy the user story.

##### 3.5 Implementation Notes

*   **Traceability Matrix:** Maintain a mapping between DSL elements, DDD classes, user stories, and acceptance criteria. This ensures each requirement is covered by tests and code.
*   **Tool Support:** Requirements and domain models can be stored in a repository. Automated scripts can parse SBVR statements to generate boilerplate code (entities, repositories, events) and test cases for EARS acceptance criteria.
*   **Team Collaboration:** Encourage domain experts to author or review SBVR statements to ensure the ubiquitous language aligns with real business terminology.

#### 4\. Rationale and Recommendations

*   **Why SBVR/UBM DSL?** SBVR provides formal semantics for business vocabularies and rules[en.wikipedia.org](https://en.wikipedia.org/wiki/Semantics_of_Business_Vocabulary_and_Business_Rules#:~:text=The%20Semantics%20of%20Business%20Vocabulary,driven%20architecture%20%28MDA); the UBM DSL encapsulates core abstractions (Node, Resource, Movement, Item, Path) applicable across domains. This supports consistent domain modeling and easy knowledge-graph integration.
*   **Why DDD?** DDD focuses on modeling software to match the domain, fosters collaboration between developers and domain experts, and uses a ubiquitous language. Combining DDD with the DSL ensures that software structure reflects real business processes.
*   **Why EARS?** EARS introduces just enough structure to natural language requirements to eliminate ambiguity and improve testability[edu.qracorp.com](https://edu.qracorp.com/hubfs/EARSConfigurationPart3.pdf#:~:text=Ubiquitous%3A%20The%20,System%20shall%20enter%20Standby%20Mode). The pattern-based approach simplifies writing acceptance criteria and aligns well with user stories.
*   **Alternative Approaches:**
    *   _Direct UML or BPMN Modeling._ Traditional UML or BPMN diagrams can model domains but lack the controlled natural language of SBVR/EARS. They may be harder for non-technical stakeholders to interpret and do not directly connect to EARS templates.
    *   _Pure Code‑First Modeling._ Starting from code (entities and services) risks misalignment with business terminology and can diverge from the domain experts’ mental model.
    *   _Recommendation:_ Use the SBVR/UBM + DDD + EARS approach as it provides a balanced mix of formal semantics, clear communication, and traceable requirements.

By following this specification and example, teams can represent complex domains clearly, derive implementation details systematically, and write unambiguous, testable requirements that align with both business and technical needs.

## Prompt:
now lets translate the specification for that DSL into a self contained python DDD/Hexagon architecture domain model including: entities, value objects, aggregates, domain event, domain commands, domain exceptions, CQRS, event/message bus, services, UoW, ports, adapters, dependency injection, etc use typing.protocol instead of Abstract Base class except where there is no other options. The DSL should now kind of work like an API for that library. using best practice without technical debt. I want to be able to make the DSL and actual data structures directly connected kind of holographically. refine and improve this prompt considering my intentions and approach; then respond to the improved/refined prompt

## Response:
Thought

Absolutely—great direction. I tightened your ask so we can build a clean, extensible foundation without hidden tech debt, then I deliver a **single, self-contained Python implementation** (DDD + Hexagon) where the **UBM DSL acts like an API** and “holographically” projects into the runtime domain model (definitions ↔ instances ↔ events ↔ storage) with CQRS, message bus, UoW, ports/adapters, DI, and type-safe protocols.

* * *

Improved / Refined Prompt
-------------------------

**Goal:**  
Produce a **self-contained Python library** that implements a **UBM-based DSL** (Resource, Node, Movement, Item, Path) and a **DDD/Hexagonal architecture** stack: value objects, entities, aggregates, domain events/commands, CQRS handlers, message bus, unit of work, ports/adapters (in-memory reference adapters), and minimal dependency injection. The **DSL must be the primary API**: UBM definitions declared at runtime are **validated**, **stored in a registry**, and **holographically** bound to concrete domain types/instances, constraints, and event flows.

**Key Requirements:**

1.  **Pythonic & Type-Safe:** Use `@dataclass(frozen=True)` for value objects; `typing.Protocol` (not ABC) for ports/repositories/UoW/bus; structural typing where possible.
2.  **DDD Core:** Entity/ValueObject base types, AggregateRoot event recording, DomainEvent/Command bases, domain exceptions.
3.  **UBM DSL API:** Builders for `ResourceDef`, `NodeDef`, `MovementDef`, `ItemDef`, `PathDef` + **Registry** (idempotent, versionable), **Rules** (cardinality + consistency), and **Factories** that produce instances wired to definitions.
4.  **CQRS:** Commands/Queries + handlers, synchronous message bus, handler registration, middleware hooks.
5.  **UoW & Repos:** Protocols + in-memory reference implementations; transactional boundary (commit/rollback), event outbox (in-memory).
6.  **Hexagonal Ports/Adapters:** Minimal `clock`, `id_provider`, `logger`, `persistence` ports with default in-memory adapters; pluggable.
7.  **DI Container:** Tiny resolver to compose the app without global state.
8.  **Example:** End-to-end “Bookstore” scenario: define DSL (Book, Money, Customer Node, Order Node, Movements), run commands (place order), observe domain events, show querying state.
9.  **Quality:** No external deps; clear separation of concerns; extensible; zero tech debt starters; docstrings on public parts.

* * *

Self-Contained Python Implementation
------------------------------------

> Copy this whole block into `ubm_ddd.py` and run with Python 3.11+.

```python
# ubm_ddd.py
from __future__ import annotations
from dataclasses import dataclass, field, replace
from typing import (
    Protocol, runtime_checkable, TypeVar, Generic, Callable, Iterable, Optional,
    Dict, List, Any, Tuple, Union, Mapping
)
from uuid import uuid4, UUID
from datetime import datetime, timezone

# =========================
# Shared primitives
# =========================

def utcnow() -> datetime:
    return datetime.now(timezone.utc)

# ================
# Domain Base Types
# ================

@dataclass(frozen=True)
class ValueObject:
    """Base class for value objects (immutable)."""
    # Add common helpers if needed

@dataclass
class Entity:
    """Base class for entities (have identity)."""
    id: UUID

@dataclass
class DomainEvent(ValueObject):
    occurred_at: datetime = field(default_factory=utcnow)

@dataclass(frozen=True)
class Command(ValueObject):
    """Marker for commands."""

@dataclass
class AggregateRoot(Entity):
    _pending_events: List[DomainEvent] = field(default_factory=list, init=False, repr=False)

    def _record(self, event: DomainEvent) -> None:
        self._pending_events.append(event)

    def pull_events(self) -> List[DomainEvent]:
        evts = list(self._pending_events)
        self._pending_events.clear()
        return evts

# =========================
# Domain Exceptions
# =========================

class DomainError(Exception): ...
class RuleViolation(DomainError): ...
class NotFound(DomainError): ...
class Conflict(DomainError): ...
class ValidationError(DomainError): ...

# =========================
# UBM DSL - Definitions
# =========================

@dataclass(frozen=True)
class ConceptId(ValueObject):
    value: str

def _cid(prefix: str, name: str) -> ConceptId:
    return ConceptId(f"{prefix}:{name}")

@dataclass(frozen=True)
class ResourceDef(ValueObject):
    id: ConceptId
    name: str
    unit_of_measure: str  # e.g., 'piece', 'USD', 'hour'
    meta: Mapping[str, Any] = field(default_factory=dict)

@dataclass(frozen=True)
class NodeDef(ValueObject):
    id: ConceptId
    name: str
    kind: str  # e.g., 'Organization', 'Account', 'Warehouse', 'Person'
    meta: Mapping[str, Any] = field(default_factory=dict)

@dataclass(frozen=True)
class PathDef(ValueObject):
    id: ConceptId
    name: str
    resource_id: ConceptId
    source_node_id: ConceptId
    destination_node_id: ConceptId
    price: Optional[float] = None
    currency: Optional[str] = None
    valid_from: Optional[datetime] = None
    valid_to: Optional[datetime] = None
    meta: Mapping[str, Any] = field(default_factory=dict)

@dataclass(frozen=True)
class ItemDef(ValueObject):
    id: ConceptId
    name: str
    resource_id: ConceptId
    meta: Mapping[str, Any] = field(default_factory=dict)

@dataclass(frozen=True)
class MovementDef(ValueObject):
    id: ConceptId
    name: str
    resource_id: ConceptId
    # movement schema can constrain fields, e.g., requires_path, requires_items, etc.
    requires_items: bool = False
    meta: Mapping[str, Any] = field(default_factory=dict)

# -------------------------
# DSL Registry (holographic link)
# -------------------------

@dataclass
class DslRegistry:
    resources: Dict[str, ResourceDef] = field(default_factory=dict)
    nodes: Dict[str, NodeDef] = field(default_factory=dict)
    paths: Dict[str, PathDef] = field(default_factory=dict)
    items: Dict[str, ItemDef] = field(default_factory=dict)
    movements: Dict[str, MovementDef] = field(default_factory=dict)

    def register_resource(self, name: str, unit: str, **meta) -> ResourceDef:
        rid = _cid("res", name)
        if rid.value in self.resources:
            return self.resources[rid.value]
        r = ResourceDef(id=rid, name=name, unit_of_measure=unit, meta=meta)
        self.resources[rid.value] = r
        return r

    def register_node(self, name: str, kind: str, **meta) -> NodeDef:
        nid = _cid("node", name)
        if nid.value in self.nodes:
            return self.nodes[nid.value]
        n = NodeDef(id=nid, name=name, kind=kind, meta=meta)
        self.n
(Content truncated due to size limit. Use page ranges or line ranges to read remaining content)