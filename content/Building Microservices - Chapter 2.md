## Introduction

The second chapter of _Building Microservices (2nd Edition)_ focuses on one of the **hardest problems in microservices architecture**:

> **How do we decide where to draw service boundaries?**

Poor boundaries lead to tightly coupled systems, distributed monoliths, and painful change. Good boundaries enable autonomy, independent evolution, and resilience.

This chapter introduces a concrete example, explains core design principles like **information hiding, cohesion, and coupling**, and then uses **Domain-Driven Design (DDD)** concepts to guide boundary decisions. It also discusses alternative ways to split services and when those alternatives may (or may not) make sense.

## Introducing MusicCorp: The Monolith Problem

MusicCorp is a fictional company that sells physical and digital music. They currently have a **"Big Ball of Mud"** monolith.

- **The Problem:** When the developers want to change how shipping labels are printed, they accidentally break the "Recommendations" engine because the code is all tangled together.
- **The Goal:** Use MusicCorp's business structure (Warehouse, Finance, Catalog) to define service boundaries.

## What Makes a Good Microservice Boundary?

Newman argues that a boundary should act as a "cell membrane," deciding what gets in and what stays hidden.

### Information Hiding

Based on David Parnas’s 1972 paper, this is the idea that a module should hide its **internal implementation details**.

- **MusicCorp Example:** The _Finance Service_ should hide how it calculates taxes. The _Web Store_ doesn't need to know the tax formulas; it just needs to send an "Order Total" and get back a "Tax Amount." If Finance changes its database from MySQL to Mongo, no other service should care.

### Cohesion

>**Things that change together should stay together.**

- **Technical Detail:** If you have to jump between three different services to add a single "Gift Wrap" feature, your cohesion is low. You want **High Cohesion**, where one business change equals one service update.

### Coupling

Coupling measures how much a change in one service forces a change in another.

- **Loose Coupling:** You can change and deploy Service A without touching Service B.

## The Interplay of Coupling and Cohesion

Newman explains that these two are linked. If you have **low cohesion** (related logic is spread out), you will naturally have **tight coupling** (you have to change many things at once). The "sweet spot" is finding boundaries that keep related logic in one place.


## Detailed Types of Coupling

Newman breaks down exactly how services get "stuck" to each other:

|**Type of Coupling**|**Description**|**MusicCorp Example**|
|---|---|---|
|**Domain Coupling**|One service needs another to do its job.|The **Web Store** calls the **Payment Service** to charge a card. (Generally Acceptable).|
|**Pass-Through**|A service passes data it doesn't use to another service.|**Warehouse** receives a `CustomerProfile` just to pass the `HomeAddress` to the **Shipping Service**. (Bad: Warehouse shouldn't know about profiles).|
|**Common Coupling**|Multiple services share the same data source (Shared DB).|**Finance** and **Warehouse** both read/write to the same `Orders` table. If Finance changes the "Status" column to a "StatusID," Warehouse crashes. (Very Dangerous).|
|**Content Coupling**|An external service reaches into another's internals.|The **Catalog Service** modifies the **Inventory Service’s** database directly via SQL. (Worst: No Information Hiding).|


## Just Enough Domain-Driven Design (DDD)

Newman simplifies DDD into three tools for modeling:

### Ubiquitous Language

Every word should mean the same thing to developers and the business.

- **Example:** In MusicCorp, don't call it a `User` in code if the business calls it a `Subscriber`. Use the business term.

### Bounded Context

This is a boundary where a specific model applies.

- **Example:** In the **Warehouse Context**, a "Return" is a physical box that needs to be inspected. In the **Customer Service Context**, a "Return" is a financial refund. By keeping these in separate Bounded Contexts, we avoid one giant, confusing `Return` object that tries to do everything.

### Aggregate

An Aggregate is a collection of objects that are bound together by a root entity.

- **Example:** An `Order` (Root) and its `LineItems`. You cannot change a `LineItem` without going through the `Order`. This ensures data stays consistent.

## Mapping to Microservices

Newman suggests that a **Bounded Context** is the ideal size for a microservice.

- If a Bounded Context is too big, you might split it by **Aggregates**.
- **The Rule:** Never let an Aggregate be split across two microservices. An Aggregate must live entirely within one service to maintain data integrity.

##  Event Storming

This is a collaborative way to find boundaries.

- Put business experts and devs in a room.
- Map out "Events" (e.g., _Order Placed_, _Payment Declined_).
- Group these events into "Contexts."

This helps find boundaries that are based on behavior, not just data tables.

## Alternatives to Business Domain Boundaries

Sometimes, business logic isn't the only reason to split a service:

- **Volatility:** If the "Tax Rules" change every month but the "Product Catalog" changes once a year, split them so you don't have to redeploy the Catalog constantly.
- **Data:** If MusicCorp starts storing "Credit Card Info," that data needs high security. Put it in its own tiny, locked-down service to reduce the scope of security audits (PCI compliance).
- **Technology:** If the "Recommendation Engine" needs a heavy Machine Learning library in Python, but the rest of the site is in Go, make the Engine a separate service.
- **Organizational:** If a new team is hired in a different time zone, give them their own service boundary so they don't have to wait for approvals from the main team.

## Mixing Models and Exceptions

Newman warns against being a "purist." You might start with business boundaries, but then move a piece of code because of a technology need. This is okay, as long as you are aware of the trade-offs in coupling and cohesion.

### Summary

The goal of Chapter 2 is to move away from technical layers (like a "Database Service" or "UI Service") and toward **Business Domains**. By using Bounded Contexts and keeping coupling low, you create a system that is flexible and easy to change.