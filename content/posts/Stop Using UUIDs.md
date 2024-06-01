---
title: Stop Using UUIDs
description: 
aliases: 
tags:
  - post
  - uuid
draft: true
date: 2024-06-01
---
In the realm of IDs, UUIDs are like the cool kids everyone wants to hang out with. Why? Because they're ridiculously easy to generate and guarantee that no two IDs are ever the same. It's like having a magical name generator that ensures each entry is unique, so you never have to worry about any awkward name mix-ups. So, next time when you're setting up IDs, just remember: UUIDs are your new best friend or are they your best friend lets talk about this shall we.

We have a tendency use UUIDs brazenly all the time to fulfil the requirement of unique identifiers. The use of UUIDs specifically UUID V4 has become the norm in the world of development though UUIDs have downsides which no one talks about like UUIDs being Unsortable and Incorrectly storing UUIDs can decrease database performance drastically.  

Lets buildup a scenario of using UUIDs as Primary Key identifiers in a database