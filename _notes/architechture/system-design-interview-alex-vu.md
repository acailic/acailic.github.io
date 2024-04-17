---
title: System Design Interview with Alex Vu
layout: post
tags: [architecture, system-design, interview, alex-vu]
date: 2020-04-17
---

## Chapter 2: Envelope estimation

Clarifying Questions: It’s crucial to ask the interviewer questions to clarify product features and requirements, such as the importance of making posts, the sorting order of the news feed, and the maximum number of friends a user can have.

High-Level Design: Develop a blueprint for the system’s design, collaborate with the interviewer, and use diagrams to illustrate key components like APIs, servers, and databases.

Deep Dive: Focus on critical components of the architecture, manage time effectively, and avoid unnecessary details that don’t demonstrate your design capabilities.

Wrap Up: Discuss potential improvements, recap the design, consider error cases and operational issues, and think about scaling for future growth.

## Chapter 3: Framework for System Design

Key-Value Store Design: Discusses designing a key-value store with operations like put(key, value) and get(key).

Design Tradeoffs: Highlights the balance between read, write, memory usage, consistency, and availability.

System Characteristics: Describes a system with small key-value pairs, big data storage, high availability, scalability, automatic scaling, tunable consistency, and low latency.

Distributed System: Explains the need for a distributed key-value store to support big data, referencing the CAP theorem and its implications on system design.

## Chapter 4: Rate Limiter

Web Crawler Purpose: It is used for search engine indexing, web archiving, web mining, and monitoring copyright infringements.

Design Scope: The design includes handling 1 billion pages per month, considering only HTML content, and storing pages for up to 5 years.

Key Characteristics: A good web crawler must be scalable, robust, polite, and extensible.

High-Level Design: The design involves components like Seed URLs, URL Frontier, HTML Downloader, and Content Parser, among others.

## Chapter 5: Design consistent hashing

Message Queue: Sends friends list and new post ID to the message queue1.

News Feed Cache: Workers fetch data from the queue and store it in the news feed cache as a <post_id, user_id> mapping table.

Memory Management: Only IDs are stored to keep memory consumption low, with a configurable limit due to the low likelihood of users scrolling through thousands of posts.

Cache Retrieval: Details the process for retrieving news feed, including fetching post IDs from the cache and constructing the fully hydrated news feed from user and post objects in caches. The final news feed is returned in JSON format to the client.

## Chapter 6: Design a key-value store

Historical Data Analysis: It involves examining historical query distributions to inform smarter sharding logic.

Shard Map Manager: This component maintains a lookup database to determine where data rows should be stored1.

Sharding Example: For instance, if queries for ‘s’ and for ‘u’ to ‘z’ are similar in number, two shards may be created: one for ‘s’ and another for ‘u’ to ‘z’.

Interviewer Questions: The section also includes potential follow-up questions an interviewer might ask about supporting multiple languages, handling country-specific top search queries, and supporting trending real-time search queries2.

## Chapter 7:

Notification Service: Informs clients of file changes to maintain consistency. Uses long polling as it’s unidirectional and infrequent, unlike WebSocket which is better for real-time, bi-directional communication like chat apps.