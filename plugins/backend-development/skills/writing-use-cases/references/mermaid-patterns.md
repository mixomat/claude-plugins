# Mermaid Sequence Diagram Patterns

## Basic Structure

```mermaid
sequenceDiagram
    actor User as User
    participant A as Service A
    participant B as Service B

    User->>+A: Request
    A->>B: Internal call
    B-->>A: Response
    A-->>-User: Final response
```

## Actors and Participants

```
actor CCC as Customer Support (CCC)     # Human actor
participant OMS as Order Management     # System/service
```

## Arrow Types

```
A->>B: Solid line, solid arrow (sync request)
A-->>B: Dashed line, solid arrow (sync response)
A->>+B: Activate B's lifeline
B-->>-A: Deactivate B's lifeline
```

## Notes

```mermaid
Note over A: Single participant note
Note over A,B: Note spanning participants
```

Use notes to group logical operations:
```mermaid
Note over OMS: Update Order
OMS->>OMS: Retrieve & validate order
OMS->>OMS: Create shipment
OMS->>OMS: Save changes
```

## Conditional Flows

```mermaid
alt condition is true
    A->>B: Do this
else condition is false
    Note over A: Skip operation
end
```

## Complete Example

```mermaid
sequenceDiagram
    actor User as User
    participant API as API Gateway
    participant SVC as Backend Service
    participant DB as Database
    participant MQ as Message Queue

    User->>+API: POST /api/resource<br/>(CreateRequest)

    Note over API: Validate Request
    API->>API: Validate input
    API->>API: Check permissions

    Note over SVC: Process Request
    API->>+SVC: Create resource
    SVC->>+DB: Insert record
    DB-->>-SVC: Record created
    SVC-->>-API: Resource created

    Note over API: Publish Event
    API->>MQ: Publish ResourceCreated event
    MQ-->>API: Event published

    API-->>-User: 201 Created

    MQ->>+SVC: ResourceCreated event

    alt notification enabled
        SVC->>SVC: Send notification
    else notification disabled
        Note over SVC: Skip notification
    end

    SVC-->>-MQ: Event processed
```

## Tips

- Use `<br/>` for line breaks in messages
- Keep participant aliases short but descriptive
- Group related operations with `Note over` blocks
- Activate/deactivate lifelines (`+`/`-`) for clarity
- Self-calls (`A->>A:`) show internal processing
