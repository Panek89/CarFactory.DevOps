# ADR-001: Docker Image Registry Selection

## Status

Accepted

## Authors

- Kamil Pankowski

## Date

2025-06-30

## Context

Our microservices project requires a reliable and cost-effective solution for storing and managing Docker images. The project consists of multiple microservices that will be containerized and deployed across different environments. We need to evaluate different registry options to determine the most suitable solution for our current needs and budget constraints.

The key requirements are:

- Support for private repositories to protect proprietary code
- Cost-effectiveness for a growing project
- Reliable availability and performance
- Easy integration with CI/CD pipelines
- Support for image tagging strategy for multiple microservices

## Decision

We will use **Docker Hub** as our primary Docker image registry solution.

## Options Considered

### 1. Docker Hub (Free Tier)

**Pros:**

- One free private repository included
- Excellent integration with Docker tooling
- High availability and global CDN
- Well-established platform with extensive documentation
- Easy setup and configuration
- Strong community support
- Automatic builds from Git repositories

**Cons:**

- Limited to one private repository on free tier
- Rate limiting on image pulls for free accounts
- Limited storage space on free tier
- No advanced security scanning on free tier

### 2. Harbor on VPS

**Pros:**

- Full control over the infrastructure
- No repository limits
- Advanced security features (vulnerability scanning, RBAC)
- On-premises deployment option
- Support for multiple registries replication
- Comprehensive audit logging

**Cons:**

- Requires VPS hosting costs (~30pln/month)
- Maintenance overhead (updates, backups, monitoring)
- Infrastructure management responsibility
- Potential single point of failure without proper setup
- Higher operational complexity

### 3. Azure Container Registry

**Pros:**

- Enterprise-grade security and compliance
- Integrated with Azure ecosystem
- Advanced features (geo-replication, webhook support)
- Automated patching and maintenance
- Strong SLA guarantees
- Advanced threat detection

**Cons:**

- Higher cost (~$5/month minimum for Basic tier)
- Vendor lock-in to Microsoft Azure
- More complex pricing model
- Overkill for small projects
- Requires Azure subscription

### 4. Amazon ECR (Elastic Container Registry)

**Pros:**

- Tight integration with AWS services
- Pay-per-use pricing model
- High availability and scalability
- Integrated security scanning
- Fine-grained access control with IAM
- Lifecycle policies for image management

**Cons:**

- AWS vendor lock-in
- Costs can escalate with usage (~$0.10 per GB/month)
- Complexity in setup and permissions
- Requires AWS knowledge and account setup
- No free tier for private repositories

## Rationale

Docker Hub was selected as the optimal solution for the following reasons:

1. **Cost-Effectiveness**: The free tier provides one private repository, which aligns perfectly with our project structure where multiple microservices can be stored as different tagged images within a single repository.

2. **Tagging Strategy**: We can implement a comprehensive tagging strategy using the format `<microservice-name>:<version>` (e.g., `carfactory-employees:v1.0.0`, `carfactory-factories:v2.1.0`) within the single private repository, effectively managing multiple microservices.

3. **Simplicity**: Docker Hub offers the lowest barrier to entry with minimal configuration required, allowing the team to focus on development rather than infrastructure management.

4. **Integration**: Native integration with Docker CLI and most CI/CD platforms reduces complexity in deployment pipelines.

5. **Reliability**: As the most widely used container registry, Docker Hub provides proven reliability and performance.

6. **Future Scalability**: When the project grows, we can easily upgrade to Docker Hub Pro ($5/month) for additional private repositories and enhanced features.

## Implementation Details

### Repository Structure

- Repository Name: `<project-name>/microservices`
- Tagging Convention: `<service-name>-<version>-<environment>`
  - Example: `carfactory-employees-1.0.0-prod`
  - Example: `carfactory-factories-2.1.0-staging`

### CI/CD Integration

- Configure automated builds triggered by Git commits
- Implement multi-stage builds for different environments
- Set up automated tagging based on Git branches/tags

### Security Considerations

- Use Docker Hub access tokens instead of passwords
- Implement image signing for production images
- Regular review of access permissions

## Consequences

### Positive

- Immediate cost savings compared to paid alternatives
- Reduced operational overhead
- Faster time to market
- Simplified development workflow
- Industry-standard solution

### Negative

- Limited to one private repository initially
- Potential rate limiting on high-usage scenarios
- Dependency on external service availability
- Limited advanced security features compared to enterprise solutions

### Risks and Mitigations

- **Risk**: Rate limiting during high CI/CD activity
  - **Mitigation**: Monitor usage and upgrade to paid tier if needed
- **Risk**: Single repository limitation as project grows
  - **Mitigation**: Plan for upgrade to Docker Hub Pro when multiple private repositories are needed

## Review Date

This decision should be reviewed in 6 months (December 2025) or when:

- The project requires more than one private repository
- Rate limiting becomes a significant issue
- Security requirements change significantly
- Team size grows substantially

## References

- [Docker Hub Pricing](https://www.docker.com/pricing)
- [Harbor Documentation](https://goharbor.io/docs/)
- [Azure Container Registry Pricing](https://azure.microsoft.com/en-us/pricing/details/container-registry/)
- [Amazon ECR Pricing](https://aws.amazon.com/ecr/pricing/)
