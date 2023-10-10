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

🔄 Real-time Inventory Updates

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

🔀 Merging values in app code

    Do not use queries where application shines


</v-clicks>

---

# ⚠️ Mind the gap

### Identifier can have huge gaps

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
layout: fact
---

## 🔥 Blazingly Fast Indexer 🔥

---

# 📂 Category Products Index

<v-clicks>

🔁 *Anchor* categories include products from child subtree

⛔  *In-active* categories disable whole subtree

⏭️ *Disabled* and *Not Visible Individually* products skipped

</v-clicks>

--- 
layout: fact 
---

## 💁 Finite-State Machine

--- 

# Category Tree FSM

<v-clicks>

⬇️ Order categories by path in ascending order

⛔ Transition to inactive state including level

⏭️ Ignore records on deeper level when marked in-active

⤵️ Push active parent category to anchor ids stack

⤴️ Pop from anchor ids stack when tree level gets reduced

</v-clicks>

---

# Supporting Table Schema

```xml {all|3-5,9-12|6-8}
<schema ...>
    <table name="blazingly_fast_index_category_lookup_anchors">
        <column xsi:type="int"
                unsigned="true" nullable="false" 
                name="entity_id" comment="Category ID" />
        <column xsi:type="varchar" 
                length="255" nullable="false"
                name="anchor_ids" comment="Anchor Parents" />

        <constraint xsi:type="primary" referenceId="PRIMARY">
            <column name="entity_id" />
        </constraint>
    </table>
</schema>
```

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

- 36 categories
- 3 store views
- 260k Real SKUs
- 5k visible configurable products

```bash
gh repo clone EcomDev/performance-training ./
make large
```

</v-clicks>

---

# How it compares

<v-clicks>

## Standard Indexer Performance

````bash
bin/magento indexer:reindex catalog_category_product
> Category Products index has been rebuilt successfully in 00:04:31
````

&nbsp;
## Data Stream

````bash
bin/magento indexer:reindex catalog_category_product_improved
> Category Products index has been rebuilt successfully in 00:00:22
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

## Proof of Concept Indexer Implementation

https://www.dropbox.com/s/vg3nlg5fxuh0mmv/index-poc-from-training.tgz?dl=0

<img src="/qrcode-index-implementation.png" class="w-50 ml-40 mt-8  shadow" />

---

## Mage-OS performance work-in-progress

**Database Changelog**

Replace MView with better context aware change detection 

https://github.com/EcomDev/mage-os-database-changelog

**Indexation Framework**

Make indexation 🔥 Blazingly Fast 🔥

https://github.com/EcomDev/mage-os-indexer


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
