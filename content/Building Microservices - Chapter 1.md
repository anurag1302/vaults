I have been reading the book Building Microservices by Sam Newman, and this article is an attempt to compose what all I have learnt in the first chapter.

## Introduction

After reading the first chapter of [Building Microservices](https://www.oreilly.com/library/view/building-microservices-2nd/9781492034018/), it becomes clear that this chapter is meant to set the mindset rather than teach implementation details. It focuses on explaining what microservices really are, why they emerged, and what trade-offs they introduce.

Instead of pushing microservices as the default architecture, the chapter encourages us to think carefully about context, scale, teams, and operational maturity.

## What Are Microservices?

At a high level, microservices are -

> Small, autonomous services that work together to form a system.

The key idea here is autonomy, not just size.

Core characteristics -

- Each service is independently deployable
- Each service focuses on a single business capability
- Services communicate via well-defined APIs
- Internal implementation details are hidden from other services

The real goal of microservices is to make systems easier to change and evolve over time, especially as they grow.

## Monoliths - Understanding the Different Types

One of the most useful parts of this chapter is how it reframes the idea of a monolith. Not all monoliths are bad, and treating them all the same leads to poor architectural decisions.

### The Single-Process monolith

This is the traditional monolith -

- A single application
- A single deployment unit
- One codebase

This approach -

- Is simple to understand
- Is easy to test and deploy early on
- Works very well for small teams and early-stage products

For many systems, this is a perfectly valid choice.

![Single Process monolith](/images/single-process-monolith.png)

### The Modular monolith

A modular monolith is still deployed as one unit, but internally it has -

- Well-defined modules
- Clear boundaries between responsibilities
- Controlled dependencies between modules

This is an important takeaway from the chapter -

> A well-designed modular monolith is often a better starting point than jumping straight into microservices.

It gives us many of the benefits of good separation without the complexity of distribution.

![Modular monolith](/images/modular-monolith.png)

### The Distributed monolith

This is the architecture we should try to avoid.

- The system is split into multiple services
- Services are tightly coupled
- Changes require coordinated deployments
- Failures easily cascade across services

In this case -

- We pay the cost of a distributed system
- But do not get the benefits of independence

The chapter makes it clear that this outcome is worse than a simple monolith.

![Distributed monolith](/images/distributed-monolith.png)

## Why Microservices Emerged

Microservices did not appear because monoliths were “wrong”. They emerged because systems and organizations changed.

Some of the pressures highlighted are -

- Growing system complexity
- Increasing number of teams
- Need for faster and safer releases
- Different parts of the system needing different scaling characteristics

As these pressures increased, tightly coupled systems became harder to evolve.

## Enabling Technologies

Microservices became practical largely because the surrounding ecosystem matured. The chapter highlights several enabling technologies that made this architectural style feasible.

### Continuous Delivery

- Automated builds, tests, and deployments
- Frequent and reliable releases
- Reduced fear of making changes

Without strong CI/CD practices, microservices become extremely difficult to manage.

### Containers

Containers play a major role in enabling microservices -

- Consistent runtime environments
- Easy packaging and deployment
- Faster startup times
- Better isolation between services

They make running many small services operationally realistic.

### Log Aggregation and Observability

In a monolith -

- Logs are usually in one place
- Debugging is relatively straightforward

In a microservices system -

- Logs are spread across many services
- Centralized log aggregation becomes essential
- Metrics, tracing, and monitoring are mandatory

A clear message from this chapter is -

> Observability is not optional in a distributed system.

### Cloud and Infrastructure Automation

Cloud platforms and automation tools enable -

- On-demand infrastructure
- Auto-scaling
- Managed databases and messaging systems

These reduce the operational burden that would otherwise make microservices impractical for most teams.

## Advantages of Microservices

The chapter presents the benefits of microservices in a grounded, realistic way.

Key advantages

- Independent deployments Teams can release changes without coordinating across the entire system.
- Focused ownership Teams own services end to end, including code and operations.
- Selective scaling Only the parts of the system that need more resources are scaled.
- Technology flexibility Different services can use different technologies when there is a clear benefit.
- Improved resilience Failures can be isolated if the system is designed carefully.

## The Cost of Microservices

An equally important part of the chapter is its honesty about trade-offs.

Microservices introduce -

- Network latency and partial failures
- Distributed data and consistency challenges
- Increased operational complexity
- Harder debugging and testing
- Higher infrastructure and tooling costs

The complexity does not disappear. It moves from -

> Code to Operations & infrastructure

![Microservices Advs and Disadvs](/images/microservices-adv-disadv.png)

## When Microservices Make Sense

Based on the discussion in this chapter, microservices tend to work best when -

- The system is large or rapidly growing
- Multiple teams need autonomy
- Independent scaling is required
- The organization has strong DevOps and operational maturity

They are often a poor fit when -

- The system is small
- The team is small
- Operational experience is limited

## Key Takeaways from Chapter 1

- Microservices are about autonomy, not size
- Monoliths are not inherently bad
- Modular monoliths are a strong architectural option
- Distributed monoliths should be avoided
- Modern tooling made microservices viable
- Operational readiness is critical
- Architecture should evolve with the organization

## Conclusion & Closing Thoughts

The first chapter sets a practical tone for the rest of the book. Rather than promoting microservices as the “right” answer, it encourages us to understand the problem we are solving first.

The biggest takeaway -

> Microservices are a powerful architectural style, but only when used in the right context.
