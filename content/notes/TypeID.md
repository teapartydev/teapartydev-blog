---
title: TypeID
description: 
aliases: 
tags:
  - Note
draft: false
date: 2024-06-03
---
TypeID is a modern, type-safe identifier format inspired by Stripe IDs. It is based on [[ULID]] and designed to be type-safe, K-sortable, and globally unique, providing a versatile solution for identifying entities in distributed systems.

## Structure of TypeID
A TypeID consists of two parts: a type prefix and a 128-bit ULID encoded as a modified base32 string. The type prefix identifies the type of entity the ID represents, while the ULID part ensures uniqueness and chronological ordering.

### Format of TypeID
```
user_01D8C1CYBCA99S6V6T8S2S9NK1
└──┘ └───────────────────────┘
type    ULID suffix (base32)
```
- `type` is a lowercase snake case ASCII string (up to 63 characters) identifying the entity type.
- `ULID suffix` is a 26-character base32-encoded string providing uniqueness and chronological ordering.

## Benefits of using TypeIDs
- **Type Safety**: TypeIDs ensure that you cannot accidentally use an ID for one type of entity where another type is expected, reducing errors and simplifying debugging.
- **Compatibility with ULIDs**: TypeIDs are compatible with ULIDs, ensuring global uniqueness and chronological ordering of identifiers.
- **K-Sortable**: TypeIDs are designed to be K-sortable, meaning they can be efficiently sorted and used as primary keys in databases, improving locality compared to entirely random IDs like UUIDv4.
- **Thoughtful Encoding**: The base32 encoding used in TypeIDs is URL safe, case-insensitive, avoids ambiguous characters, and is more compact than traditional hex encoding, making it easier to work with in various scenarios.

## Downsides of using TypeIDs
- **Less Widely Adopted**: TypeIDs are not as widely adopted as UUIDs, which may limit their compatibility with certain systems and libraries.
- **Configuration Overhead**: The requirement to define type prefixes adds some configuration overhead compared to simpler identifiers.

## TypeID vs. UUID
- **Uniqueness**: Both TypeIDs and UUIDs are designed to be globally unique, but TypeIDs achieve this by combining a type prefix with a ULID, while UUIDs rely solely on randomness.
- **Sortability**: TypeIDs, like ULIDs, are K-sortable, which UUIDs are not.
- **Type Safety**: TypeIDs offer type safety by including a type prefix, reducing the risk of using IDs in the wrong context.
- **Compactness**: TypeIDs use a compact base32 encoding, making them easier to handle in various scenarios compared to the hexadecimal encoding of UUIDs.

In summary, TypeIDs represent a harmonious blend of the best characteristics found in modern identifier formats. They combine the type safety and readability of human-readable identifiers with the global uniqueness and sorting capabilities of ULIDs. TypeIDs offer a robust solution for identifying entities in distributed systems, providing developers with a versatile and efficient tool for managing and referencing data. However, their adoption is less widespread compared to UUIDs, which may affect compatibility with some systems.