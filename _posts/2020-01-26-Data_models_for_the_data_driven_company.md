---
title: "Data system performance concepts and data models for the data-driven company"
description: In this article we explore basic concepts your data infrastructure needs to adhere to and three data models for describing your business entities.
published: false
toc: true
comments: true
layout: post
author: Georg Walther
categories: [data-driven company, data models]
image: images/posts/kolar-io-lRoX0shwjUQ-unsplash.jpg
---

## Data-driven company

Data-driven companies generate actionable insights from their data - thus being able to take
tractable steps guided by data.

Actionable insights guiding data-driven companies may be:

- Our production data show that lowering roll pressure on this machine by X bar when Y steel from Z supplier is processed will
  increase product quality by 5%
- Our procurement data prove that we should place this order with supplier X instead of supplier Y this month in order to
  save 12% on raw materials cost
- Our sales data indicate that offering client X will increase the likelihood of them increasing their order volume thus
  offsetting our discount favorably

![Stages of the data-driven company](/images/posts/data_driven_company_stages.png)

For enterprises to get to these actionable insights they need to collect, store, model, and analyze data appropriately.
In this article we will explore some basic concepts and technologies that can help you make the right technological
decisions in your path towards becoming a data-driven company.
Many concepts and examples presented here are token from the excellent book
[Designing Data-Intensive Applications](http://shop.oreilly.com/product/0636920032175.do) by Martin Kleppmann -
with updates where I saw them fit.

## Data system performance concepts

The technology stack of your data-driven company needs to be reliable, scalable, and maintainable.

### Reliability

Data collection, storage, and access for analysis are key processes of the data-driven company that
need to reliably work even in the face of faults.

Since faults (hardware faults, software errors, and human errors) always occur,
the goal is to build fault-tolerant systems as opposed to preventing faults altogether.

A common approach to building fault-tolerant systems is the [Netflix chaos monkey](https://github.com/Netflix/chaosmonkey) toolkit
and the related field of chaos engineering.

### Scalability

Blanket statements such as "my data infrastructure scales" are not very meaningful since what we are really interested in is:
"If the load on my data pipeline grows in a particular way, will my key quality metrics drop off a cliff or will my system cope?"

So to track whether your data system scales you need to do two things:

1. Define your load parameters: E.g. data transfer, write, or read rates at key points in your system,
2. Define your performance metrics: E.g. I want my median database query to take no longer than 0.5 seconds, and my 95-percentile query no longer than 1.5 seconds.

### Maintainability

All systems and data pipelines you will build on your journey to becoming a data-driven company will eventually need to be changed, updated, or upgraded.
Your data systems will always need to evolve as your operations scale or change.

Three guiding principles help you set up your data systems for necessary continual evolution:

1. Operability: Making it easy for your operations teams to keep your data systems running - the pillars and principles of DevOps are likely key in achieving this,
2. Simplicity: Removing or lowering barriers for new engineers to come on board and contribute to your data systems - often achieved through removing unnecessary complexity,
3. Evolvability: Making it easy for your engineers to change your data systems to accommodate new or unanticipated use cases or requirements.

## Data models

The data you collect and store describe numerous concepts and entities relevant to your business.
These concepts and entities may include:

- Purchase orders
- Products and items
- Production orders
- Raw materials
- Tools and machinery
- Production lines
- Employees
- Electricity and other utility bills

To collect and store these data points you need to decide on how to model them in your data system.

Depending on the properties of your business entities and - especially - the conceptual connections between
those entities you have three basic data models to choose from.

### Relational / SQL model

The relational, or SQL, model describes your business entities as tables and rows.
Relationships between business entities are modelled as foreign keys thus allowing us to easily
describe one-to-many relationships.
One-to-many relationships include:

- One department has many employees assigned to it,
- One production order can have many manufacture steps assigned to it,
- One production line can have many machines or workstations assigned to it.

SQL databases routinely rely on a schema-on-write approach to defining business entity schemas:
The schema-on-write (or statically typed) approach usually requires you to define the fields, data types, and optionally
value ranges of your entities before you can start writing data points to your database.
Thus this approach does not expect the schema of your business entities to change - and if it does
change requires you to update the definitions of your database tables through a mechanism called migration.

### Document model

Business entities that have no or few connections to other entities are best described with the document model.
In the document model each data point is recorded as a closed entity with all its relevant data fields and values.
A common and flexible format used with the document model is JSON.

Entities that fit this category are, among others:

- Click events on your website,
- Searches or other actions performed by users,
- Time-series data of market demand or economic indicators,
- Time-series data of household energy demand.

Intricately linked to the document model is the concept of NoSQL (not only SQL) databases.
Many NoSQL databases implement a schema-on-read (or schemaless) approach to defining business entities.
This approach allows for more flexibility in what your database objects look like but require your data processing
and analytics pipeline to contain more logic for handling diverging schemas.

### Graph model

The graph model describes business entities as nodes and relationships between them as edges.
The graph model is especially useful when describing business entities that are highly linked with a plethora of many-to-many relationships.

Many-to-many relationships include:

- People networks where each person has numerous friends and colleagues,
- Complex production lines where each workstation is linked to many others,
- Warehouse routing networks where each aisle or storage slot is in close proximity to many others,
- Logistics networks locations are linked to many others,
- Mechanical systems such as train cars or vehicles where each mechanical components is linked to many others -
  this is likely interesting in the context of predictive maintenance and failure cascades.

## References

- [Martin Kleppmann: Designing Data-Intensive Applications](http://shop.oreilly.com/product/0636920032175.do)
- [The evolution of the data-driven company](https://www.ibm.com/blogs/business-analytics/evolution-data-driven-company/)
