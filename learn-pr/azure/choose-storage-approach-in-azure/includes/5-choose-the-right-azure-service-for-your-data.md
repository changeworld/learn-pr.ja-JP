Choosing the correct storage solution can lead to better performance, cost savings, and improved managability. Here, you'll apply what you've learned about the data in your online retail scenario, and find the best Azure service for each data set. 

## Product catalog data

**Data classification:** Semi-structured because of the need to extend or modify the schema for new products

**Operations:**

- Customers require a high number of read operations, with the ability to query on many fields within the database.
- The business requires a high number of write operations to track the constantly changing inventory.

**Latency & throughput:** High throughput and low latency

**Transactional support:** Required

### Recommended service: Azure Cosmos DB

Azure Cosmos DB supports semi-structured data, or NoSQL data, by design. So, supporting new fields, such as the "Bluetooth Enabled" field or any new fields you need in the future, is a given with Azure Cosmos DB.

Regarding operations, Azure Cosmos DB supports SQL for queries and every property is indexed by default. Creating queries to match your customers’ need to filter on almost everything is supported.

Regarding latency and throughput, Azure Cosmos DB enables you to configure your both. You can scale up to handle higher customer demand during peak shopping times, or scale down during slower times to conserve cost. And because Azure Cosmos DB indexes all properties by default, customers will be able to query on any field.

Azure Cosmos DB is also ACID-compliant, so you can be assured that your transactions are completed according to those strict requirements.

As an added plus, Azure Cosmos DB also enables you to replicate your data anywhere in the world with the click of a button. So, if your e-commerce site has concentrated users in the US, France, and England, you can replicate your data to those data centers to reduce latency, as you've physically moved the data closer to your users. And even with data replicated around the world, you can choose from one of five consistency levels. By choosing the right consistency level, you can determine the tradeoffs to make between consistency, availability, latency, and throughput.

### Why not other Azure services?

Azure SQL Database would be an excellent choice for this data set, were it not for the need to extend the schema ad-hoc for new products. Azure SQL Database can provide many of the same benefits of Azure Cosmos DB, but it cannot handle heterogeneous data, all data needs to adhere to a schema.

Other Azure services, such as Azure Table storage, Azure HBase as a part of HDInsight, and Azure Redis Cache, can also store NoSQL data. In this scenario, because users will want to query on multiple fields, Azure Cosmos DB is a better fit. This is because Azure Cosmos DB indexes every field by default, whereas the other services are limited in the data they index, so they have reduced abilities to query on any field in the database.

## Photos and videos

**Data classification:** Unstructured

**Operations:**

- Only need to be retrieved by ID.
- Customers require a high number of read operations with low-latency.
- Creates and updates will be somewhat infrequent and can have higher latency than read operations.

**Latency & throughput:** Retrievals by ID need to support low latency and high throughput. Creates and updates can have higher latency than read operations.

**Transactional support:** Not required

### Recommended service: Azure Blob storage

Azure Blob storage supports storing files such as photos and videos. It also works with Azure Content Delivery Network (CDN) by caching the most used content and storing it on edge servers. This reduces latency in serving up those images to your users.

By using Azure Blob storage, you can also move images from the hot storage tier to the cool or archive storage tier, to reduce costs and focus throughput on the most viewed images and videos.

### Why not other Azure services?

You could upload your images to Azure App Service, so that the same server running your app is serving up your images. That would work if you didn't have many file. But if you have lots of files, and a global audience, you'll get more performant results by using Azure Blob storage with Azure CDN.

## Business data

**Data classification:** Structured

**Operations:** Read-only, complex analytical queries across multiple databases

**Latency & throughput:** Some latency in the results is expected based on the complex nature of the queries.

**Transactional support:** Required

### Recommended service: Azure SQL Database

Business data will most likely be queried by business analysts, who are more likely to know SQL than any other query language. Azure SQL Database could be used as the solution by itself, but pairing it with Azure Analysis Services enables data analysts to create a semantic model over the data in SQL Database. They can then share it with business users, so that all they need to do is connect to the model from any business intelligence (BI) tool, and immediately explore the data and gain insights. 

### Why not other Azure services?

Azure SQL Data Warehouse supports OLAP solutions and SQL queries. But your business analysts will need to perform cross-database queries, which SQL Data Warehouse does not support.

Azure Analysis Services could be used in addition to Azure SQL Database. But your business analysts are more well versed in SQL than working with Power BI. So they'd like a database that supports SQL queries, which Azure Analysis Services does not. In addition, the financial data you're storing in your business data set is relational and multidimensional in nature. Azure Analysis Services supports tabular data stored on the service itself, but not multidimensional data. To analyze multidimensional data with Azure Analysis Services, you can use direct query to the SQL Database.

Azure Stream Analytics is a great way to analyze data and transform it into actionable insights, but its focus is on real-time data that is streaming in. In this case, our business analysts will be looking at historical data only.

## Summary

Each category of data has different storage requirements, and it's your job to figure out which solution is best. You should always consider the category of data, required operations, latency, and the need for transactional support.