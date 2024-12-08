---
title: BitBend
description: 
aliases: 
tags:
  - Note
draft: false
date: 2024-12-01
---
# Introducing BitBend: The Software-Defined Database That Puts You in Control 🚀

Modern databases are impressive, no doubt about it. They ship with an arsenal of features, from `ROW LEVEL SECURITY` to intricate query optimizers, all wrapped up with ACID guarantees. But here's the thing: developers often bypass these features, opting to re-implement them in their application code. Why? Because the built-in tools aren't always scalable or transparent, and splitting logic between the database and the application can lead to confusion, performance bottlenecks, and unintended side effects.

**Enter BitBend.**  
BitBend isn’t just another database—it’s a **software-defined database** that reimagines how developers interact with their data. It gives you **complete control**, letting you write custom logic directly next to your data in **TypeScript/JavaScript**, all while ensuring correctness, scalability, and real-time reactivity.

BitBend leverages the power of **V8**—the same engine that powers Chrome and Node.js—to execute all `mutations` (data modifications) and `queries` (data retrieval) in a **transactional environment**, enabling the full capabilities of TypeScript/JavaScript. In the future, BitBend will also support **WASM (WebAssembly)**, opening up even more possibilities for custom logic.

But BitBend doesn’t stop there. For long-running tasks or operations requiring additional flexibility, developers can use **actions**, which execute in **Node.js** and allow TypeScript/JavaScript to run for hours if needed. While **actions** aren’t transactional or reactive like `mutations` or `queries`, they work seamlessly as serverless functions. With this dual execution model, **BitBend ensures that all your application code can live in one place, with no compromises.**

---

### 🚦 **No Bells and Whistles (and That’s a Good Thing)**

Most databases come with an array of out-of-the-box features designed to help developers enforce business logic. Constraints like `CHECK` statements or `FOREIGN KEYs`, and security features like `ROW LEVEL SECURITY`, seem helpful at first glance. But in practice, many teams find these tools either too limiting or too opaque, and they opt to implement their business logic directly in their application layer.

The problem? Now, your logic is scattered across two domains partly in the database and partly in your code. Want to understand how your system behaves? You’ll need to piece together the logic in both places, which can be a nightmare for debugging, scalability, and onboarding.

**BitBend flips the script:**

- **No built-in constraints, no hidden features.** You get a clean slate to implement exactly what you need.
- **Write your logic next to your data** in familiar languages like TypeScript and JavaScript.
- Anything the DB doesn’t have? **Build it yourself** or leverage a community package.

It’s like having a database where you’re in charge, and your logic is guaranteed to work seamlessly and transactionally. No compromises, no surprises.

---

### 🧠 **Declarative Queries, Full Control**

Traditional databases rely on **query planners** to determine how to execute your SQL statements. These planners are powerful and can optimize queries for complex workloads. PostgreSQL’s query planner, for instance, is a marvel of engineering. But query planners aren’t perfect—they can sometimes make the wrong decision, resulting in suboptimal performance and frustrating debugging sessions.

**BitBend takes a different approach:**

- Queries are **declarative**, written in TypeScript/JavaScript.
- Every query operates in a **range scan format**, giving you explicit control over data access patterns.
- You define **indexes** and explicitly refer to them in your queries, ensuring your code is efficient by design.

This approach eliminates the "magic" of query planners and puts you firmly in control of how your queries execute. If performance issues arise, it’s straightforward to debug because **everything is just code**—clear, transparent, and easy to reason about.

---

### ✅ **Serializable Consistency, Always**

Consistency in databases is a complicated topic. Most systems offer tunable consistency levels, like "eventual consistency" or "read-committed." While this flexibility can be useful, it’s often a hassle for developers who just want one thing: **correct data**.

With BitBend, there’s no guesswork.

- Transactions run with **Serializable consistency**, the gold standard for correctness.
- This ensures that all operations appear to run sequentially, even when they’re executed in parallel.

**How does BitBend achieve this?**  
Through a technique called **Deterministic Optimistic Concurrency Control (DOCC):**

- Transactions are **lock-free**, avoiding the bottlenecks of traditional locking mechanisms.
- If a conflict occurs, deterministic logic ensures transactions can be safely retried without unintended side effects.

To maintain this high level of consistency, BitBend enforces some constraints:

- Transactions must complete within **5 seconds**.
- They are **one-shot transactions**, meaning all data modifications and validations happen in a single step.

These constraints ensure BitBend stays fast, correct, and scalable, without compromising on developer experience.

---

### 🔄 **Real-Time Reactivity Built-In**

Real-time updates are a staple of modern applications, whether it’s collaborative document editing, live dashboards, or chat apps. But implementing real-time behavior is notoriously tricky. Traditional databases often require third-party tools, polling mechanisms, or complex streaming architectures to achieve real-time functionality.

**BitBend makes real-time reactivity effortless:**

- Every query you write is automatically a **subscription**.
- When data changes, all dependent queries are updated in real-time—no additional code or infrastructure required.

It’s real-time all the time, baked into the core of the database. No hacks, no workarounds. Just seamless, reactive applications.

---

### 💾 **Triple-Layer Storage for Ultimate Flexibility**

BitBend’s storage architecture is designed to handle diverse workloads, from OLTP to OLAP to full-text and vector searches. It’s a **hybrid triple-layer system**:

1. **Layer 1:** In-memory distributed cache for lightning-fast access.
2. **Layer 2:** NVMe/SSD-backed storage, powered by Raft for durability and consistency.
3. **Layer 3:** S3-compatible object storage for cheap, long-term persistence and infinite scalability.

This setup lets BitBend excel in:

- **Transactional workloads (OLTP):** Low-latency reads and writes.
- **Analytics (OLAP):** A column-store mirror of row-store data enables efficient analytical queries.
- **Search and AI:** Full-text search and vector search indexes are built-in, making it a search engine and AI-ready database out of the box.

Whether you’re building a high-traffic web app, running analytics, or embedding AI models, BitBend has you covered.

---

### 🛠 **V8 and Node.js: Power and Flexibility**

What makes BitBend truly special is how it executes your application code:

- **V8 for Mutations and Queries:** Every `mutation` and `query` runs inside the V8 engine, giving you the full power of **TypeScript/JavaScript** in a transactional environment.
- **Node.js for Actions:** For long-running or non-transactional tasks, you can use `actions`, which run on Node.js. These can execute for hours, making them perfect for tasks like complex data processing or external service integration.

With this model:

- **Mutations and Queries** are transactional, reactive, and high-performance.
- **Actions** offer the flexibility of serverless functions while integrating seamlessly with the rest of your application logic.

All of this is underpinned by BitBend’s **Rust runtime**, which guarantees blazing-fast performance and reliability.

---

### TL;DR: Why BitBend?

BitBend isn’t just a database; it’s a paradigm shift in how developers think about data. It combines the best aspects of databases, application servers, and real-time systems into a unified platform:

- **Control:** Write your logic directly next to your data, with full transparency and no hidden behavior.
- **Flexibility:** Choose between transactional V8 execution or long-running Node.js functions.
- **Scalability:** Declarative queries and lock-free transactions ensure your system stays fast and efficient.
- **Correctness:** Serializable consistency guarantees data integrity without compromise.
- **Real-time reactivity:** Effortless live updates make modern app development a breeze.

If you’re ready to leave behind the complexities of traditional databases and embrace a new way of working with data, **BitBend is here to make it happen.**