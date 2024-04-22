---
theme: dracula
canvasWidth: 700
layout: cover
text-align: center
title: Blazingly Fast Catalog Data Streaming and Processing
---

# 🔥 Blazingly Fast 🔥

## Catalog Data Streaming and Processing

---

# About me

<v-clicks>

🫶 Passionate about open source projects

🚀 Expert in optimizing web appplications

👴🏻 One of grand-parents of Magento

🧑‍🚒 Experienced in putting out fires on production systems

</v-clicks>

--- 

# Why 🔥 Blazingly Fast?

<v-clicks>

🏆 Competitive Advantage

📈 Scalability

⚙️ Operational Efficiency

💰 Lower Infrastructure Costs

</v-clicks>

---

# Where It Helps

<v-clicks>

📊 BI / analytics tools

🔍 Search & Personalization Engines 

📌 Much better Data Indexation

🛍️ Marketplace Feeds

</v-clicks>

---
layout: fact
---

## 🤔 Why NOT core tools? 

---
layout: fact
---

## 📈 Excessive Memory Usage 

<v-click>

### in App and Database

</v-click>

---
layout: fact
---

## 🚫 99.9% of redundant I/O and CPU cycles

--- 
layout: fact
---

## 💥 Breaking Changes in API

<v-click>

### Database schema is the most stable API

</v-click>

--- 

# 🪄 Creating efficient data stream

<v-clicks>

📜 Prepared statements 

    Remove time it takes for MySQL to parse your queries

🔪 Range database queries 

    Reduce scope of data your database has to process

🔀 Merging values in PHP code

    Do not use queries where application shines

</v-clicks>

---

# ⚠️ Mind the gap

### Identifiers can have huge gaps

<v-click>

```sql {all|2-3|5}
SELECT
  MIN(entity_id) as `min`,
  MAX(entity_id) as `max`
FROM catalog_product_entity
GROUP BY CEIL(entity_id / 5000)
```

</v-click>

---

# ⚠️ Avoid complex JOINs

### Type of JOIN execution strategy

<v-clicks>

👌 `Index Usage` uses index to find records in second table

😒 `Block Nested Loop` uses `join_buffer` if second table can fit completely in it to speedup look up of rows.

😱 `Nested Loop` full table scan of second table for each row of the first one

</v-clicks>

--- 

# ⚠️ Unbuffered Queries


<v-clicks>

🔗 Separated connections for each query type

🔄 Fetch each row as a generator

📊 Interleave reading by using a sorted query result 

</v-clicks>

--- 
layout: fact
---

## 🔥 Blazingly Fast Indexer 🔥

---

# 📂 Category Products Index

<v-clicks>

🔁 *Anchor* categories include products from child subtree

⛔ *In-active* categories disable whole subtree

⏭️ *Disabled* and *Not Visible Individually* products skipped

</v-clicks>

---

# Supporting Table Schema

<v-clicks>

- **category_id** and **store_id** as a primary key
- **anchor_ids** - JSON array of all parent categories that are `is_anchor`

</v-clicks>

---
layout: fact
---

## 💁 State Machine

--- 

# Category Tree State Machine

<v-clicks>

⬇️ Order categories by path in ascending order

⛔ Transition to inactive state by tracking level

⏭️ Ignore records on deeper level when marked in-active

⤵️ Push active parent category to anchor ids stack

⤴️ Pop from anchor ids stack when tree level changes

</v-clicks>


--- 

# Product Part of Index

<v-clicks>

- Selects main entity record by primary key range
- Selects data for relevant attributes from EAV tables for all stores
- Generates entries from category lookup data with assigned category
- Uses generators for continuous stream of index data to be sent

</v-clicks>

---

# Reference Setup

<v-clicks>

- 1k categories
- 3 store views
- 260k Real SKUs
- 5k visible configurable products
- ~30% visible simple products

```bash
gh repo clone EcomDev/performance-training ./
make large
```

</v-clicks>

---

# How fast is it?

<v-clicks>

````bash
# Single-threaded
bin/magento indexer:reindex catalog_category_product
> Category Products index has been rebuilt successfully in 00:04:28
````

````bash
# Multi-threaded
bin/magento indexer:reindex catalog_category_product
> Category Products index has been rebuilt successfully in 00:02:16
````

````bash
# New Implementation
bin/magento indexer:reindex catalog_category_product_improved
> Category Products index has been rebuilt successfully in 00:00:06
````

</v-clicks>

---
layout: fact
---

#  It is faster 

<v-click>

## But is it BLAZINGLY FAST 🔥 ?

</v-click>

---

# Execution Timeline

<img src="/sequential-processing.png"  />

--- 

# Non-Blocking Implementation

<img src="/non-blocking-processing.png"  />

---
layout: fact
---

## Proof of Concept 

<img src="/qrcode-index-poc.svg" class="w-40% ml-30% mt-8  shadow" />

---
layout: fact
---

## It is coming to

<img src="/mage-os.svg" class="w-80% ml-10 mt-3  shadow">

---

## 5-Day Performance Training

📅 Date: June 17th, 2024

📍 Location: Innobyte, Bucharest, Romania

🔗 https://mage-os-performance-bucharest.eventbrite.com


---
layout: two-cols
---

# Thank You!

## Support Charities

- Ukrainian Foundation "Come Back Alive"

  https://savelife.in.ua/en/


- Dutch Foundation "Sails of Freedom"

  https://zeilenvanvrijheid.nl/

::right::

# &nbsp;

## Help Ukraine 🇺🇦

<img src="/qrcode-come-back-alive.png" class="w-20 ml-10 mt-8  shadow"  /> 

<img src="/qrcode-zeilen.png"  class="w-20 ml-10 mt-8 shadow"  />
