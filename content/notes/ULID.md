---
title: ULID
description: 
aliases: 
tags:
  - Note
draft: false
date: 2024-06-03
---
ULID, or Universally Unique Lexicographically Sortable Identifier, is a type of identifier that combines the uniqueness of [[UUID]]s with the ability to be easily sortable. ULIDs are 128-bit identifiers designed for high performance in distributed systems and are particularly useful in scenarios where the identifiers need to be both unique and ordered.

## Structure of ULID
A ULID is composed of two parts: a 48-bit timestamp and a 80-bit random component. This structure ensures that ULIDs generated at different times are inherently ordered based on their creation time, making them ideal for use cases where chronological sorting is important.

### Format of ULID
```
tttttttt-tttt-rrrr-yyyy-yyyyyyyyyyyy
```
- `t` represents the timestamp, encoded as 10 base32 characters (0-9 and a-v).
- `r` represents the random component, encoded as 16 base32 characters.
- `y` represents the fixed bits, used for ULID identification and encoded as 10 base32 characters.

## Benefits of using ULIDs
- **Uniqueness**: Like UUIDs, ULIDs are designed to be globally unique, reducing the risk of identifier collisions in distributed systems.
- **Chronological Sorting**: The timestamp component of ULIDs ensures that they can be sorted chronologically without additional processing, making them ideal for time-series data.
- **Compactness**: ULIDs are more compact than UUIDs, which can be beneficial in terms of storage space efficiency, especially in large datasets.
- **Compatibility**: ULIDs can be easily converted to and from UUIDs, allowing for seamless integration with existing systems.

## Downsides of using ULIDs
- **Less Widely Adopted**: ULIDs are not as widely adopted as UUIDs, which may limit their compatibility with certain systems and libraries.
- **Complexity**: The use of a timestamp and random component in ULIDs adds complexity compared to simple sequential identifiers, which may increase the implementation overhead.

## ULID vs. UUID
- **Uniqueness**: Both ULIDs and UUIDs are designed to be globally unique, but ULIDs achieve this by combining a timestamp with randomness, while UUIDs rely solely on randomness.
- **Sortability**: ULIDs are lexicographically sortable based on their timestamp, which UUIDs are not.
- **Compactness**: ULIDs are more compact than UUIDs, making them more efficient in terms of storage space.

In summary, ULIDs offer a unique combination of global uniqueness and chronological sorting, making them well-suited for use in distributed systems where ordered identifiers are required.