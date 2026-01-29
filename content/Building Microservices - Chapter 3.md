
## Introduction

When we talk about microservices, the first big question is almost always:

> “We already have a monolith - how do we split it safely?”

Chapter 3 of _Building Microservices_  focuses exactly on this problem. The key idea of the chapter is **not rushing**, **not rewriting everything**, and **not treating the monolith as bad**. Instead, we evolve the system carefully.

Let’s walk through the ideas step by step.

---

## **Have a Goal**

Before we touch any code, we must ask ourselves:

**Why are we splitting the monolith?**

Common goals are:

- We want faster deployments
- We want teams to work independently
- We want better scalability in specific areas
- We want safer changes in frequently modified code

If we don’t have a clear goal, we’ll end up splitting things _randomly_. That usually leads to more services, more complexity, and **no real benefit**.

So first, we agree on:

- What problem we are solving
- How we will measure success

Microservices are a **means**, not the goal.

---

## **Incremental Migration**

There is strong advice against a **big-bang rewrite**.

Instead of rewriting the entire monolith:

- We move **one small piece at a time**
- We keep the system running throughout the migration
- We learn from each step and adjust our approach

The monolith continues serving users while we slowly carve pieces out of it. This reduces risk and keeps the business moving.

Think of this as **renovating a house while living in it**, not demolishing and rebuilding.

---

## **The Monolith Is Rarely the Enemy**

It’s easy to blame the monolith for everything:

- Slow development
- Fear of deployments
- Tight coupling

But the truth is:

- The monolith probably delivered a lot of value
- It may still work reliably in production
- It can help us during migration

The monolith is often **a victim of success**, not bad design. Treating it as an enemy leads to rushed decisions and unnecessary rewrites.

We should **use the monolith as a starting point**, not something to destroy.

---

## **The Dangers of Premature Decomposition**

If we split too early or without understanding the domain:

- We create services with poor boundaries
- We move complexity from code into the network
- We introduce distributed systems problems too early

Instead of clean boundaries, we get:

- Chatty services
- Shared databases
- Tight runtime coupling

This is worse than a monolith.

We should first:

- Understand how the system works
- Identify stable business boundaries
- Improve modularity inside the monolith if needed

Only then does splitting make sense.

---

## **What to Split First?**

We should not ask:

> “What is easiest to split?”

We should ask:

> “What gives us the most value if split?”

Good candidates are:

- Areas that change frequently
- Functionality owned by a specific team
- Parts that cause deployment risk
- Features that need to scale differently

Often, we start with **non-core or edge functionality** rather than the heart of the system.

---

## **Decomposition by Layer**

When splitting, we often face a choice.

### **Code First Approach**

Here, we:

- Extract business logic into a new service
- Still use the existing database initially

Why this works:

- We get an independently deployable service quickly
- We understand real dependencies early
- We reduce risk in the first step

Downside:

- Shared database coupling may remain for a while

This approach is very common in real systems.

---

### **Data First Approach**

Here, we:

- Split the database first
- Move data ownership into the new service

Why this is harder:

- Data dependencies are usually deeply tangled
- Transactions and joins become complex immediately

But:

- It forces us to design proper boundaries
- It avoids long-term shared database problems

Many teams start with **code first**, then follow up with **data separation**.

---

## **Useful Decompositional Patterns**

The book introduces several patterns that help us migrate safely.

---

## **Strangler Fig Pattern**

This is one of the most important patterns.

How it works:

- We build new functionality as a microservice
- Requests go through a routing layer
- Gradually, traffic moves from monolith to the new service
- Eventually, the old code can be removed

The old system and new system **coexist**.

Benefits:

- Very low risk
- Easy rollback
- No big rewrite

This pattern is perfect when we expose functionality via APIs or HTTP endpoints.

---

## **Parallel Run**

For critical functionality, we sometimes:

- Run old and new implementations at the same time
- Feed both with the same inputs
- Compare the outputs

Only when we are confident:

- We switch fully to the new service

This approach builds **real production confidence**, but it increases complexity and cost.

---

## **Feature Toggle**

Feature toggles allow us to:

- Deploy new code safely
- Turn it on or off without redeploying

During migration:

- New service logic can be hidden behind a toggle
- We enable it gradually (internal users → partial traffic → full traffic)

This separates **deployment from release**, which is extremely valuable in distributed systems.

---

## **Data Decomposition Concerns**

Once we split services, data becomes the hardest part.

---

### **Performance**

In a monolith:

- We make in-process calls
- Database joins are cheap

In microservices:

- Network calls add latency
- Too many calls slow the system

We often need:

- Caching
- Asynchronous communication
- Aggregation services

We trade simplicity for independence.

---

### **Data Integrity**

We can no longer rely on:

- Cross-table foreign keys
- Database-level constraints across services

Instead:

- Each service owns its data
- Integrity is enforced via APIs and domain rules

This is more work, but it gives us autonomy.

---

### **Transactions**

Distributed transactions are hard and fragile.

Instead of two-phase commits, we use:

- **Sagas**
- Local transactions
- Compensating actions

This gives us **eventual consistency**, which is usually good enough for business systems.

---

### **Tooling**

Splitting systems means:

- More deployments
- More logs
- More failures to observe

We must invest in:

- Centralized logging
- Distributed tracing
- Monitoring and alerting

Without tooling, microservices become unmanageable.

---

### **Reporting Database**

Even with separated databases, the business still wants reports.

A common solution:

- Each service publishes events
- Data is copied into a reporting database
- Reporting queries run on this read-optimized store

This avoids coupling operational databases while still enabling analytics.

---

## **Conclusion**

Chapter 3 teaches us that:

- Splitting the monolith is a **journey**, not a rewrite
- We must start with **clear goals**
- Incremental change is safer than big rewrites
- The monolith is often an **ally**, not a villain
- Premature decomposition creates more problems than it solves
- Patterns like **Strangler Fig, Parallel Run, and Feature Toggles** reduce risk
- Data is the hardest part and must be handled carefully
- Tooling and observability are non-negotiable