---
title: Stop Using UUIDs
description: 
aliases: 
tags:
  - post
  - uuid
draft: false
date: 2024-06-01
---
## The Hidden Pitfalls of UUIDs

In the realm of IDs, UUIDs are like the cool kids everyone wants to hang out with. Why? Because they're ridiculously easy to generate and guarantee that no two IDs are ever the same. It's like having a magical name generator that ensures each entry is unique, so you never have to worry about any awkward name mix-ups. So, next time when you're setting up IDs, just remember: UUIDs are your new best friend or are they your best friend lets talk about this shall we.

We have a tendency use UUIDs brazenly all the time to fulfil the requirement of unique identifiers. The use of UUIDs specifically UUID V4 has become the norm in the world of development though UUIDs have downsides which no one talks about like UUIDs being Unsortable and Incorrectly storing UUIDs can decrease database performance drastically.  

## Issues with UUIDs as Primary Keys and How to Mitigate Them

Let's build a scenario where we use UUIDs as primary key identifiers in a database table and identify the potential problems we might encounter.

- Users Table

| name       | type                      |
| ---------- | ------------------------- |
| id         | text (UUID - Primary Key) |
| name       | text                      |
| email      | text                      |
| age        | int                       |
| created_at | timestamp                 |
| updated_at | timestamp                 |

- Posts Table

| name         | type                               |
| ------------ | ---------------------------------- |
| id           | text (UUID - Primary Key)          |
| creator_id   | text (UUID - Foreign Key to Users) |
| name         | text                               |
| content      | text                               |
| published_at | timestamp                          |
| created_at   | timestamp                          |
| updated_at   | timestamp                          |

Let's break down the problems caused by using UUID as Primary Key in the above tables.

1. **Using Text Field for UUIDs is Insufficient**
    - Storing UUIDs as text fields can lead to inefficiencies. Text fields require more storage space and can lead to slower query performance compared to binary representations. Using `UUID` type directly or converting them to `BINARY(16)` can be more efficient.
2. **Unsortable**
    - UUIDs are not naturally sortable in a meaningful way. This can cause issues when trying to order records by the primary key, especially if the order of insertion or any temporal sequence is important for the application's logic.
3. **Not Human Readable**
    - UUIDs are complex and not human-readable, making them difficult to work with when manually inspecting database records. This can complicate debugging and data analysis processes.
4. **Hard to Copy and Paste due to Hyphens**
    - UUIDs contain hyphens, which can make them harder to copy and paste accurately. These hyphens split the UUID into five parts, each treated as separate segments. This increases the likelihood of errors when manually handling UUIDs, especially in command-line interfaces or scripts.
5. **No Clear Indication of Entity Type**
    - UUIDs used as primary keys in different tables (e.g., Users and Posts) do not provide any indication of their source. This lack of differentiation can lead to confusion and potential errors, as a UUID from one table could mistakenly be used in another context.
6. **Uses More Storage Space**
    - UUIDs are 128-bit values, which consume more storage space compared to other ID types like ULID's or . This can increase the overall size of the database, leading to higher storage costs and potentially affecting performance.
7. **Hard to Implement Efficient Cursor-Based Pagination**
    - Implementing efficient cursor-based pagination with UUIDs can be challenging. UUIDs do not have a natural order, making it difficult to create stable pagination cursors. This can result in poor performance and complexity in query implementation, especially in large datasets.

Let's fix down the problems caused by using UUID as Primary Key in the above tables.

1. **Optimizing Storage for UUIDs**
    - Use the native `UUID` type if available, or store UUIDs in a binary format (`BINARY(16)`) to improve storage efficiency and query performance. This reduces the storage footprint and enhances index performance.
2. **Handling Sortability**
    - Use UUID versions that include timestamps (e.g., UUIDv1 or UUIDv6) to achieve partial ordering based on creation time. Alternatively, add an additional sequential column (e.g., `created_at`) for ordering purposes.
    - An alternative solution is to use ULIDs, which are both sortable and unique, providing the best of both worlds.
3. **Improving Readability**
    - Use UUIDs primarily for internal processing and provide additional human-readable identifiers (e.g., usernames or user IDs) for easier reference during manual inspection and debugging.
4. **Simplifying Copy and Paste**
    - Design interfaces and tools to handle UUIDs seamlessly, reducing the need for manual copying and pasting. Additionally, use compact UUID representations without hyphens where appropriate.
5. **Differentiating Entity Types**
    - Implement namespaced UUIDs or use prefix conventions to differentiate UUIDs between entities like `user_0b882a25-0319-4f4c-93b0-e5431fe8cba3` or `post_33874e0b-0317-4296-9995-aa8def3873ab`. This ensures clarity and reduces the risk of misusing UUIDs across different contexts.
    -  An alternative solution is TypeIDs like `user_01HZEWFS1TSMXR5NR5VDH8GKB7` or `post_01HZEWG8NE2JDDFYF70E85B74A` which are both sortable, unique and has types built-in which make them easy to differentiate between entities.
6. **Managing Storage Space**
    - Consider the trade-offs between UUIDs and other key types based on application needs. Use optimized storage formats to mitigate the impact on space and performance. Evaluate the necessity of UUIDs for your specific use case.
7. **Enhancing Cursor-Based Pagination**
    - Use surrogate keys or additional ordered columns (e.g., `created_at` or `sequential_id`) for pagination. Combining UUIDs with timestamps or sequence numbers can help achieve efficient and predictable pagination.
    - An alternative solution is to use ULIDs or TypeIDs, which are both sortable and unique, which make them unique while achieving efficient and predictable pagination.


