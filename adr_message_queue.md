# ADR-001: Message Queue Selection for Small Microservices Application

## Authors

- Kamil Pankowski

## Status
**Accepted** - 2025-07-06

## Context
We are developing a small microservices application with a maximum of 3 services that will be deployed using Docker Compose for local development and potentially small-scale production environments. The application requires asynchronous communication between services for tasks such as:

- Event-driven notifications between services
- Decoupling service dependencies
- Handling background job processing
- Ensuring message delivery reliability

### Requirements
- **Simplicity**: Easy to set up and maintain for a small team
- **Docker Compose compatibility**: Must work seamlessly in containerized environment
- **Local development friendly**: Quick startup, good debugging capabilities
- **Reliability**: Basic message persistence and delivery guarantees
- **Resource efficiency**: Minimal resource footprint for small-scale deployment
- **Operational overhead**: Low maintenance requirements

## Decision
**We will use RabbitMQ as our message queue solution.**

## Rationale

### Options Considered

#### 1. RabbitMQ
**Pros:**
- Excellent Docker Compose integration with official images
- Rich management UI for debugging and monitoring
- Mature, battle-tested solution with extensive documentation
- Low resource footprint suitable for small applications
- Simple setup with sensible defaults
- Strong community support and extensive examples
- Supports multiple messaging patterns (pub/sub, work queues, RPC)

**Cons:**
- Additional infrastructure component to manage
- Potential overkill for very simple use cases

#### 2. Azure Service Bus Emulator
**Pros:**
- Matches production Azure environment if deploying to Azure
- Familiar for teams already using Azure ecosystem
- Built-in dead letter queue functionality

**Cons:**
- Limited local development experience
- Emulator may not have full feature parity with actual Azure Service Bus
- Additional complexity for non-Azure deployments
- Less mature tooling for local debugging
- Requires Azure CLI and additional setup steps

#### 3. Redis (with Redis Streams)
**Pros:**
- Dual purpose: caching and message queuing
- Very fast and lightweight
- Excellent Docker support
- Simple data structures
- Good for high-throughput scenarios

**Cons:**
- Primarily in-memory storage (persistence requires configuration)
- Less mature message queue features compared to dedicated solutions
- Limited message routing capabilities
- No built-in management UI for message queues

### Why RabbitMQ?

1. **Developer Experience**: The RabbitMQ Management UI provides excellent visibility into queue states, message flows, and system health - crucial for debugging in development.

2. **Documentation and Examples**: Extensive documentation and community examples specifically for Docker Compose setups make implementation straightforward.

3. **Operational Simplicity**: Single container deployment with minimal configuration required for basic use cases.

4. **Scalability Path**: While we start small, RabbitMQ can easily scale with clustering if needed later.

5. **Message Patterns**: Supports all common messaging patterns we might need as the application grows.

## Consequences

### Positive
- **Fast Development Cycle**: Quick setup enables immediate development progress
- **Excellent Debugging**: Management UI provides real-time insights into message flows
- **Reliability**: Mature message durability and delivery guarantees
- **Team Productivity**: Well-documented patterns and extensive community resources
- **Future-Proof**: Easy to extend with additional messaging patterns as application grows

### Negative
- **Additional Infrastructure**: One more component to manage and monitor
- **Learning Curve**: Team needs to learn AMQP concepts and RabbitMQ specifics
- **Resource Usage**: Additional memory and CPU overhead (though minimal for small applications)

### Risks and Mitigations
- **Risk**: RabbitMQ container failure causes message loss
  - **Mitigation**: Use persistent volumes and configure durable queues
- **Risk**: Performance bottlenecks as application scales
  - **Mitigation**: Monitor queue depths and implement proper connection pooling

## Alternatives Considered but Rejected
- **Apache Kafka**: Too heavyweight for 3 microservices
- **AWS SQS**: Requires AWS infrastructure, not suitable for Docker Compose local development
- **In-memory solutions**: Too simplistic, lack persistence guarantees

## Review Date
This decision should be reviewed in 6 months or when the application scales beyond 5 microservices.
