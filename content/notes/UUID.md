---
title: UUID
description: 
aliases: 
tags:
  - note
  - uuid
draft: false
date: 2024-05-31
---
UUID, or Universally Unique Identifier, are standardized identifiers defined by RFC 4122. They are used to uniquely identify information in computer systems. UUIDs are 128-bit numbers represented as a 32-character hexadecimal string, typically displayed in five groups separated by hyphens, like this: `550e8400-e29b-41d4-a716-446655440000`.

### Versions of UUIDs
1. **Version 1 (Time-based)**: This version uses the current timestamp and the MAC address (or a random node ID) to generate UUIDs. While this version guarantees uniqueness, it may reveal information about the system's network configuration and has a limited resolution of 100 nanoseconds.
2. **Version 2 (DCE Security)**: This version is similar to Version 1 but includes additional information such as the POSIX UID or GID of the user generating the UUID. This version is rarely used.
3. **Version 3 (Name-based, MD5)**: This version generates UUIDs based on a namespace identifier (typically a URL) and a name. The MD5 hash algorithm is used to combine the namespace and name to generate the UUID. While this version ensures uniqueness within a namespace, it does not guarantee global uniqueness.
4. **Version 4 (Random)**: This version uses random or pseudo-random numbers to generate UUIDs. It is the most commonly used version due to its simplicity and high likelihood of uniqueness. However, since it relies on randomness, there is a theoretical possibility of generating duplicate UUIDs.
5. **Version 5 (Name-based, SHA-1)**: Similar to Version 3, this version generates UUIDs based on a namespace identifier and a name, but it uses the SHA-1 hash algorithm instead of MD5. Like Version 3, it ensures uniqueness within a namespace but does not guarantee global uniqueness.

### UUID Version 4 (UUID V4)
UUID Version 4 (UUID V4) are generated using random or pseudo-random numbers. They are the most commonly used version of UUIDs due to their simplicity and high likelihood of uniqueness. Version 4 UUIDs consist of 122 random bits, with the remaining 6 bits fixed to specific values to indicate the version (4) and variant (RFC 4122).

#### Format of a UUID Version 4 
```
xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx
```

- `x` represents a random hexadecimal digit (0-9, a-f).
- `4` indicates the UUID version (in this case, Version 4).
- `y` is a random hexadecimal digit (8, 9, a, or b) representing the variant (RFC 4122).

 Example UUID Version 4 might look like this: `f47ac10b-58cc-4372-a567-0e02b2c3d479`. In this UUID
- The `4` in the third group indicates it is a Version 4 UUID.
- The `8` in the fourth group indicates it is a variant specified by RFC 4122.

Each UUID is generated using a random or pseudo-random number generator, ensuring that the probability of generating the same UUID twice is extremely low, even across different systems.

### Benefits of using UUIDs
- **Uniqueness**: UUIDs are designed to be globally unique, meaning that the probability of generating the same UUID twice is extremely low, even across different systems.
- **Ease of Generation**: UUIDs can be generated without coordination between multiple systems, making them ideal for distributed systems.
- **Flexibility**: UUIDs can be generated using different algorithms and sources of randomness, allowing developers to choose the most suitable method for their application.

### Downsides of using UUIDs
- **Length**: UUIDs are 128 bits long, which can be inefficient in terms of storage space compared to shorter identifiers.
- **Readability**: The hexadecimal representation of UUIDs can be less readable than other types of identifiers, especially in logs or user interfaces.
- **Collisions**: While the probability of generating duplicate UUIDs is low, it is not zero, especially in Version 4 (Random) UUIDs where collisions can occur if the random number generator is not properly seeded.
- **Selecting and Copying**: Selecting and copying UUIDs can be tricky. UUIDs contain hyphen (`-`) separators to divide the ID into parts. When trying to select the entire ID for copying, only part of it gets selected because the hyphens cause the characters to be treated as separate parts.
- **Unsortable**: UUIDs, while useful for generating unique identifiers, have some drawbacks. They are not sortable in the way that sequentially generated IDs are, which can impact performance in certain scenarios. Sorting UUIDs requires additional processing compared to incrementing integer IDs, which can lead to slower database operations, especially in large datasets.

Overall, UUIDs are a powerful tool for generating unique identifiers in distributed systems, providing a balance between uniqueness, ease of generation, and flexibility.