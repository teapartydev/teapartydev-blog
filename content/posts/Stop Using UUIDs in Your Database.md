---
title: Stop Using UUIDs in Your Database
description: 
aliases: 
tags:
  - Post
draft: false
date: 2024-06-01
---
## The Elegance and Dilemmas of UUIDs

In the realm of database identifiers, [[UUID]]s are like the cool kids everyone wants to hang out with. Why? Because they're ridiculously easy to generate and guarantee that no two IDs are ever the same. It's like having a magical name generator that ensures each entry is unique, so you never have to worry about any awkward name mix-ups. So, next time when you're setting up IDs, just remember: UUIDs are your new best friend or are they your best friend lets talk about this shall we.

We have a tendency use UUIDs brazenly all the time to fulfil the requirement of unique identifiers. The use of UUIDs specifically UUIDv4 has become the norm in the world of development though UUIDs have downsides which no one talks about like UUIDs being Unsortable and Incorrectly storing UUIDs can decrease database performance drastically.  

## Unpacking the Challenges of Using UUIDs

- **Storage**
	- Storing UUIDs as text fields can lead to inefficiencies. Text fields require more storage space and can lead to slower query performance compared to binary representations. 
	- Use the native `UUID` type if available, or store UUIDs in a binary format (`BINARY(16)`) to improve storage efficiency and query performance. This reduces the storage footprint and enhances index performance.
- **Sortability**
	- UUIDs are not naturally sortable in a meaningful way. This can cause issues when trying to order records by the primary key, especially if the order of insertion or any temporal sequence is important for the application's logic.
	- Use UUID versions that include timestamps (e.g., ULID, ) to achieve partial ordering based on creation time. Alternatively, add an additional sequential column (e.g., `created_at`) for ordering purposes. An alternative solution is to use ULIDs, which are both sortable and unique, providing the best of both worlds.
- **Pagination**
	- Implementing efficient cursor-based pagination with UUIDs can be challenging. UUIDs do not have a natural order, making it difficult to create stable pagination cursors. This can result in poor performance and complexity in query implementation, especially in large datasets.
	- Use surrogate keys or additional ordered columns (e.g., `created_at` or `sequential_id`) for pagination. Combining UUIDs with timestamps or sequence numbers can help achieve efficient and predictable pagination. An alternative solution is to use ULIDs or TypeIDs, which are both sortable and unique, which make them unique while achieving efficient and predictable pagination.
- **Differentiation**
	- UUIDs used as primary keys in different tables (e.g., Users and Posts) do not provide any indication of their source. This lack of differentiation can lead to confusion and potential errors, as a UUID from one table could mistakenly be used in another context.
	- Implement name spaced UUIDs or use prefix conventions to differentiate UUIDs between entities like `user_0b882a25-0319-4f4c-93b0-e5431fe8cba3` or `post_33874e0b-0317-4296-9995-aa8def3873ab`. This ensures clarity and reduces the risk of misusing UUIDs across different contexts. An alternative solution is TypeIDs like `user_01HZEWFS1TSMXR5NR5VDH8GKB7` or `post_01HZEWG8NE2JDDFYF70E85B74A` which are both sortable, unique and has types built-in which make them easy to differentiate between entities.
- **Readability**
	- UUIDs are complex and not human-readable, making them difficult to work with when manually inspecting database records. This can complicate debugging and data analysis processes.
	- Use UUIDs primarily for internal processing and provide additional human-readable identifiers (e.g., usernames or user IDs) or prefix the UUID with some information .
- **Ergonomics**
	- UUIDs contain hyphens, which can make them harder to copy and paste accurately. These hyphens split the UUID into five parts, each treated as separate segments. This increases the likelihood of errors when manually handling UUIDs, especially in command-line interfaces or scripts.
	- Use compact UUID representations without hyphens where appropriate. This makes the UUID easy to copy and paste accurately.


