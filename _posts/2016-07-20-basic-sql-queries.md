---
layout: post
title: "Basic SQL Queries: Three Questions"
tags:
  - "tech"
  - "sql"
---

I've found it helpful when building simple SQL queries to organize my thinking around three questions:

* What data am I interested in?
  * Raw data, or calculated?
  * Individual rows, or aggregate data?
  * All results, or filtered results?

* Where is the data coming from?

* How do I want the data presented?

This diagram summarizes the different parts of a SQL query in relation to those three questions:

![Diagram showing basic SQL query parts and how they relate to decisions][diagram]

There are a few things left out, like subqueries and the fact that you can alias tables, not just selected data. But hopefully it'll get you started with organizing your SQL thinking more effectively!

[diagram]: {{ site.url }}/images/basic-sql-queries.png
