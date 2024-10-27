---
title: Reconsidering UUIDs in The Database
description: 
aliases: 
tags:
  - Post
draft: false
date: 2024-06-01
---
## The elegance and dilemmas of UUIDs
In the realm of database identifiers, [[UUID]]s are like the cool kids everyone wants to hang out with. Why? Because they're ridiculously easy to generate and guarantee that no two IDs are ever the same. It's like having a magical name generator that ensures each entry is unique, so you never have to worry about any awkward name mix-ups. So, next time when you're setting up IDs, just remember: UUIDs are your new best friend or are they your best friend lets talk about this shall we.

We have a tendency use UUIDs brazenly all the time to fulfil the requirement of unique identifiers. The use of UUIDs specifically UUIDv4 has become the norm in the world of development though UUIDs have downsides which no one talks about like UUIDs being Unsortable and Incorrectly storing UUIDs can decrease database performance drastically.

## Unpacking the challenges of using UUIDs
- Storage
	- Problem: Storing UUIDs as text fields can lead to inefficiencies. Text fields require more storage space and can lead to slower query performance compared to binary representations. 
	- Solution: Use the native `UUID` type if available, or store UUIDs in a binary format (`BINARY(16)`) to improve storage efficiency and query performance. This reduces the storage footprint and enhances index performance.
- Sortability
	- Problem: UUIDs are not naturally sortable in a meaningful way. This can cause issues when trying to order records by the primary key, especially if the order of insertion or any temporal sequence is important for the application's logic.
	- Solution: Use UUID versions that include timestamps (e.g., ULID, ) to achieve partial ordering based on creation time. Alternatively, add an additional sequential column (e.g., `created_at`) for ordering purposes. An alternative solution is to use ULIDs, which are both sortable and unique, providing the best of both worlds.
- Pagination
	- Problem: Implementing efficient cursor-based pagination with UUIDs can be challenging. UUIDs do not have a natural order, making it difficult to create stable pagination cursors. This can result in poor performance and complexity in query implementation, especially in large datasets.
	- Solution: Use surrogate keys or additional ordered columns (e.g., `created_at` or `sequential_id`) for pagination. Combining UUIDs with timestamps or sequence numbers can help achieve efficient and predictable pagination. An alternative solution is to use ULIDs or TypeIDs, which are both sortable and unique, which make them unique while achieving efficient and predictable pagination.
- Differentiation
	- Problem: UUIDs used as primary keys in different tables (e.g., Users and Posts) do not provide any indication of their source. This lack of differentiation can lead to confusion and potential errors, as a UUID from one table could mistakenly be used in another context.
	- Solution: Implement name spaced UUIDs or use prefix conventions to differentiate UUIDs between entities like `user_0b882a25-0319-4f4c-93b0-e5431fe8cba3` or `post_33874e0b-0317-4296-9995-aa8def3873ab`. This ensures clarity and reduces the risk of misusing UUIDs across different contexts. An alternative solution is TypeIDs like `user_01HZEWFS1TSMXR5NR5VDH8GKB7` or `post_01HZEWG8NE2JDDFYF70E85B74A` which are both sortable, unique and has types built-in which make them easy to differentiate between entities.
- Readability
	- Problem: UUIDs are complex and not human-readable, making them difficult to work with when manually inspecting database records.
	- Solution: Use UUIDs primarily for internal processing and provide additional human-readable identifiers (e.g., usernames or user IDs) or prefix the UUID with some human-readable information.
- Ergonomics
	- Problem: UUIDs contain hyphens, which can make them harder to copy and paste accurately. These hyphens split the UUID into five parts, each treated as separate segments. This increases the likelihood of errors when manually handling UUIDs, especially in command-line interfaces or scripts.
	- Solution: Use compact UUID representations without hyphens where appropriate. This makes the UUID easy to copy and paste accurately.

## Beyond UUIDs for unique identifiers
- ULID (Universally Unique Lexicographically Sortable Identifier)
	- ULID was developed to address some limitations of UUIDs, specifically their lack of sortability and readability. It is composed of a 48-bit timestamp and an 80-bit random component. This structure makes ULIDs sortable by creation time, as the first part is a millisecond-precision timestamp. Additionally, ULIDs are more human-readable due to their base32 encoding, which uses Crockford's alphabet. With a low probability of collisions thanks to the 80-bit randomness, ULIDs are well-suited for databases where insertion order is important and for systems requiring human-readable IDs.
- KSUID (K-Sortable Unique Identifier)
	- KSUID, created by Segment, provides a highly sortable and globally unique identifier. It consists of a 32-bit timestamp and a 128-bit random component, which allows for chronological sorting due to the timestamp's position at the start. KSUIDs are also more compact and readable than UUIDs, thanks to their base62 encoding. The large random component ensures an extremely low chance of collisions. KSUIDs are ideal for distributed systems where the order of events is crucial and for applications needing unique and sortable IDs.
- SnowflakeID
	- Developed by Twitter, SnowflakeID generates unique IDs efficiently in distributed systems. Each SnowflakeID is made up of a 41-bit timestamp, a 10-bit machine ID, and a 12-bit sequence number. This structure supports scalability by allowing IDs to be generated independently on multiple nodes without synchronization. The timestamp component ensures chronological sorting, while the overall design allows for quick and efficient ID generation. SnowflakeID is particularly suited for social networks and large-scale web services that require distributed unique ID generation and systems needing high throughput ID generation.
- CUID (Collision-resistant Unique Identifier)
	- CUID is designed for environments where collision resistance and usability are key concerns. It combines a timestamp, a counter, a fingerprint of the generating machine, and random characters to create each identifier. This composition minimizes the likelihood of collisions, even when IDs are generated in parallel. CUIDs are more human-readable than UUIDs and are simple to implement and understand. They are particularly useful in web applications where unique identifiers are generated frequently and in systems requiring a balance of readability and uniqueness. (CUIDv2 is not sortable)
- NanoID
	- NanoID is a tiny, secure, URL-friendly unique string ID generator, typically producing IDs that are 21 characters long. This makes NanoID more compact than UUIDs. It uses a cryptographically strong random number generator, ensuring security. NanoID is also highly customizable, allowing the length and alphabet to be adjusted to meet specific needs. This makes NanoID well-suited for front-end applications where URL length matters and for systems requiring secure and compact unique identifiers.
- TypeID
	- TypeID is a modern, type-safe identifier format inspired by Stripe IDs. It is designed to be type-safe, K-sortable, and globally unique, based on ULID. TypeID consists of two parts: a type prefix and a 128-bit ULID encoded as a modified base32 string. The type prefix is a lowercase snake case ASCII string (up to 63 characters) that identifies the type of entity the ID represents, while the ULID part is a 26-character base32-encoded string, providing uniqueness and chronological ordering. The format ensures type safety, as TypeIDs prevent the accidental use of an ID for one type of entity where another type is expected, reducing errors and easing debugging. TypeIDs are compatible with ULIDs, ensuring global uniqueness and chronological ordering of identifiers. They are also K-sortable, allowing efficient sorting and use as primary keys in databases, improving locality compared to entirely random IDs like UUIDv4. The thoughtful encoding of TypeIDs, which is URL-safe, case-insensitive, and avoids ambiguous characters, makes them easier to work with in various scenarios.

While UUIDs are widely used for their simplicity and uniqueness, alternative ID generation methods like ULID, KSUID, SnowflakeID, CUID, NanoID, and TypeID offer additional benefits such as sortability, readability, collision resistance, and compactness. TypeID, in particular, represents a harmonious blend of modern identifier formats, combining type safety, readability, and the sorting capabilities of ULIDs. Choosing the right one depends on the specific needs of your application, such as the importance of time-based sorting, readability, and the environment in which the IDs will be generated and used.



