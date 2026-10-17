
# Overview

Data mesh at core is founded in decentralization and distribution of responsibility to people who are closest to the data in order to support continuous change and scalability.

With a data mesh, the number of places and teams who provide data increases.

Data meshes invert the model of responsibility of past models ([[data lakes|data.lake]], [[data warehouses|data.warehouse]]), with the accountability of data quality shifting upstream as close to the source of the data as possible.

Data mesh lets you scale your data architecture in 2 ways:
- horizontal scalability
- across an organization as the business evolves and changes

Data mesh solves the problem of data pipelines, which often become brittle over time, due to things like having to rely on availability of services, incompatible changes to APIs etc.

Data mesh reduces the likelihood of there being inconsistencies between the same sets of data in different parts of the organization.

Data mesh can be thought of as a network to exchange data (between nodes) about the business throughout the organization.
- the nodes are the *data products*

Data mesh takes us from the ad hoc way of building microservices (many apps, dbs and SaaS's that have various pipelines to each other), into something that is conceptually a little more centralized (with something like Kafka in the middle connecting everything)
- the idea is to centralize an immutable stream of facts, but decentralize the freedom to act, adapt and change.

While [[DDD|general.principles.DDD]] has been implemented widely to application code, it has not been applied to domain concepts in a data platform.
- Data mesh takes plenty of concepts from DDD.
- data meshes are to data and analytics what microservices are to application architecture.

A technology like [[kafka]] is a good fit for Data Mesh

Data mesh aims to alleviate the issue of management and access to analytical data at scale.

# Principles
The objective of a Data Mesh is to create a foundation for getting value from analytical data and historical facts at scale.
- To achieve this, there are 4 underpinning principles that a data mesh implementation provides:
    1. domain-oriented data ownership and architecture
    2. data as a product
    3. self-serve data infrastructure as a platform
    4. [[federated|general.terms.federated]] computational governance

These 4 principles are all necessary.

## Domain ownership
A domain usually follows the same lines as the [[bounded context|general.principles.DDD.bounded-context]].
- ex. If we had a music streaming application, the podcast domain would need to be in charge of both APIs to create a podcast episode, but also an analytical data endpoint for retrieving all podcast episode's data over the last 12 months.
    - This implies that the architecture must remove any friction or coupling to let domains serve their analytical data and release the code that computes the data, independently of other domains. 

As a result, there is no data team or data analytics team.

Naturally, each domain can have dependencies to other domains' operational and analytical data endpoints.

## Data as a product
The Data as a Product principle is designed to address the data quality and age-old data silos problem, which is that data is collected from all areas of the business, but is never used to its potential.
- Analytical data provided by the domains must be treated as a product, and the consumers of that data should be treated as customers 

For a domain data to be considered a product, a data mesh implementation should support discoverability, security, explorability, understandability and trustworthines

*Data Product* is the architectural quantum (ie. an independently deployable component) of a data mesh.

Data product is the node on the mesh that encapsulates three structural components required for its function, providing access to domain's analytical data as a product:
- Code for...
    - consuming, transforming and serving upstream data 
    - APIs that provide access to data, semantic and syntax schema, observability metrics and other metadata
    - enforcing traits such as access control policies, compliance, provenance, etc.
- Data & Metadata. Data can be served as events, batch files, relational tables, graphs, etc.
- Infrastructure, which enables building, deploying and running the data product's code, as well as storage and access to big data and metadata.

In a data mesh, pipelines (code) are managed as independent components from the data they produce
- Data product is a composition of all components - code, data and infrastructure - at the granularity of a domain's bounded context.

## Availability and self-serve nature of data
Data is available everywhere and it is self-serve from anywhere in the organization.

If you wanted to be able to source data from multiple domains to aggregate in some kind of reporting system, data mesh should allow you to do that very quickly.

This principle is necessary, due to the extra infrastructure that needs to be provisioned and run, as well as the skills needed to replicate it everwhere.
- the only practical way that each team could realistically own their data products would be to have access to a high-level abstraction of infrastructure, so that they don't have to manually manage the lifecycle of data products.

### Planes
The self-serve platform is composed of multiple planes, each serving a different profile of users.
- A plane is representative of a level of existence - integrated yet separate.
    - this is a similar concept to *control plane* in networking— which is the part of a network that controls how data packets are forwarded from one place to another.
- a hierarchy of planes is desirable

#### Example planes
- *Data infrastructure provisioning plane*, which supports provisioning of the underlying infrastructure which is required to run the components of a data product and mesh of products. This would include the provisioning of distributed file storage, storage accounts, access control management system, the orchestration to run data products internal code, provisioning of a distributed query engine on a graph of data products, etc. This type of plane would likely only be used by higher-level planes, since it is such a low-level data infrastructure lifecycle management plane.
- *Data product developer experience plane*, which is the interface that a data product developer is more likely to use. The purpose of the plane is to use simple declarative interfaces to manage the lifecycle of a data product.
- *Data mesh supervision plane*, which operates at the mesh level, enabling it to work with a graph of connected data products. For instance, this would grant the ability to discover data products for a particular use case.

## Data is governed where it is
Data is governed where it is created and stored.

Because the scope of data is always changing, this principle allows us to more quickly navigate data in the mesh and have confidence in that data.

To get value in forms of higher order datasets, insights, or machine intelligence, there is a need for these independent data products to interoperate. 
- We need to be able to correlate them, create unions, find intersections, or perform other graphs or set operations on them at scale.
- To achieve this, a data mesh implementation requires a governance model that embraces decentralization and domain self-sovereignty, interoperability through global standardization, a dynamic topology and most importantly automated execution of decisions by the platform.

This raises an important point, which is "what should be standardized globally across the organization, and what should be left up to each domain?"
- ex. in the case of an audio streaming app, the *podcast audienceship* data model should be left up to the Podcast domain team, whereas the decision of how to identify a *podcast listener* should be a global concern, since a podcast listener is a member of the more organization-wide concept of a *user*, which is an upstream [[bounded context|general.principles.DDD.bounded-context]], and thus present in other domains. Because we'd realistically want to correlate a single user across multiple domains (e.g. a user who is both a podcast listener and a music listener), this is a global concern.

* * *

## Data Lake/Warehouse vs Data Mesh
| Pre-data mesh governance aspect (data lake/warehouse)               | Data mesh governance aspect                                                                                                           |
|---------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Centralized team                                                    | [[general.terms.federated]] team                                                                                                                        |
| Responsible for data quality                                        | Responsible for defining how to model what constitutes quality                                                                        |
| Responsible for data security                                       | Responsible for defining aspects of data security i.e. data sensitivity levels for the platform to build in and monitor automatically |
| Responsible for complying with regulation                           | Responsible for defining the regulation requirements for the platform to build in and monitor automatically                           |
| Centralized custodianship of data                                   | Federated custodianship of data by domains                                                                                            |
| Responsible for global canonical data modeling                      | Responsible for modeling polysemes - data elements that cross the boundaries of multiple domains                                      |
| Team is independent from domains                                    | Team is made of domains representatives                                                                                               |
| Aiming for a well defined static structure of data                  | Aiming for enabling effective mesh operation embracing a continuously changing and a dynamic topology of the mesh                     |
| Centralized technology used by monolithic lake/warehouse            | Self-serve platform technologies used by each domain                                                                                  |
| Measure success based on number or volume of governed data (tables) | Measure success based on the network effect - the connections representing the consumption of data on the mesh                        |
| Manual process with human intervention                              | Automated processes implemented by the platform                                                                                       |
| Prevent error                                                       | Detect error and recover through platform’s automated processing                                                                      |

# Resources
[Original article](https://martinfowler.com/articles/data-mesh-principles.html)