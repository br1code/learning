# Azure Cosmos DB

**Also see:** [NoSQL Databases/Cloud Pripietary/Azure CosmosDB](../../../NoSQL%20Databases/Cloud%20Propietary/Azure%20CosmosDB.md)

Azure Cosmos DB is a globally distributed, multi-model, and fully managed NoSQL database service provided by Microsoft. It's designed to offer low-latency, high-availability, and elastic scalability for mission-critical applications. Here's an overview of its key aspects:

## Global Distribution

Cosmos DB allows you to seamlessly distribute your data across multiple Azure regions, ensuring low-latency access for users worldwide. You can add or remove regions with no downtime, and Cosmos DB automatically replicates your data for high availability.

## Multi-Model Support

Cosmos DB supports multiple data models, including document, key-value, column-family, and graph. It provides APIs for SQL, MongoDB, Cassandra, Gremlin, and Azure Table Storage, allowing you to use familiar query languages and tools.

## Partitioning

Cosmos DB uses horizontal partitioning to distribute your data across multiple physical partitions, enabling it to scale out and handle large amounts of data and high throughput. You need to choose a partition key that evenly distributes data and balances the workload.

## Request Units (RUs)

Cosmos DB measures the cost of database operations in Request Units (RUs). Each operation has an associated RU cost, which depends on factors like item size, query complexity, and indexing. You need to provision enough RUs to handle your workload and avoid throttling.

## Consistency Levels

Cosmos DB offers five well-defined consistency levels, ranging from strong to eventual consistency. You can choose the most suitable level based on your application's requirements and trade-offs between consistency, availability, and performance.

## Indexing

Cosmos DB automatically indexes all properties in your data, enabling you to perform efficient queries without manual schema management. You can also customize the indexing policy to optimize storage and RU consumption.

## Time to Live (TTL)

Cosmos DB supports setting a Time to Live (TTL) value for items in your database. This feature allows you to automatically expire and delete data after a specified period, reducing storage costs and ensuring data compliance.

## Change Feed

Cosmos DB provides a change feed that tracks changes to your data in near real-time. You can use this feature to synchronize data across multiple services, trigger serverless functions, and perform real-time analytics.

## Backup and Restore

Cosmos DB offers automatic and continuous backups, with configurable retention periods. You can also perform on-demand backups and restore your data to a specific point in time.

## Security

Cosmos DB provides multiple layers of security, including network isolation with Azure Virtual Networks, data encryption at rest and in transit, Azure Active Directory integration, and IP firewall rules.