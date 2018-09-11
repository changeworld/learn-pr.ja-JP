Once you've identified the kind of data you're dealing with (structured, semi-structured, or unstructured), the other important step is to determine how you'll use the data. For example, as an online retailer you know customers need quick access to product data, and business users need to run complex analytical queries. As you start to work through these requirements, and take your data classification into account, you can start to put together your data storage approach.

Here, you'll go through some of the questions you should ask when determining what you'll do with your data.

## Operations and latency

What are the main operations you'll be completing on each data type, and what are performance requirements?

Ask yourself these questions:
* Will you be doing simple lookups by an ID? 
* Do you need to query the database for one or more fields? 
* How many create, update, and delete operations do you expect? 
* Do you need to run complex analytical queries? 
* How fast do these operations need to complete?

Let’s walk through each of the data sets with these questions in mind and discuss the requirements.

## Product catalog data

For product catalog data in an online retail scenario, customers will have the highest priority needs. Customers will want to query the product catalog to find, for example, all men's shoes, then men's shoes on sale, and then men's shoes on sale in a particular size. So, you could categorize customer's needs as requiring lots of read operations, with the ability to query on certain fields.

In addition, when customers place orders, the product data quantities need to be updated by the application. Those update operations need to happen as fast as the read operations so that users don't end up placing items in their shopping baskets when that product has just sold out. So not only do you need lots of read operations, you also require lots of write operations for product catalog data. Be sure to determine the priorities for all the users of the database, not just the primary ones.

## Photos and videos

The photos and videos that are displayed on product pages have different requirements though. They do need fast retrieval times to display on the site at the same time as the product catalog data, but they don't need to be queried independently. Instead, you can rely on the results of the product query, and just have the video ID or URL as a property on the product data. So, photos and videos don't need to be queried by anything other than their ID.

In addition, users will not be making updates to existing photos or videos. They may, however, add new photos for product reviews. So, they might upload an image of themselves showing off their new shoes. As an employee, you also upload and delete product photos from your vendor, but that update doesn't need to happen as fast as your other product data updates. So, in summary, photos and videos just need to be queried by ID to return the whole file, but creates and updates will be less frequent and are less of a priority.  

## Business data

For business data, all the analysis is happening on historical data. None of the original data would be updated based on the analysis. So, the business data is read-only, and users don't expect their complex analytics to run instantly, so having some latency in the results is okay. In addition, business data will be stored in multiple data sets, as users will have different access to write to each data set. However, the business analysts will be able to read from all databases.

## Summary

When deciding what storage solution to use, you should think about how your data will be used. How often will your data be accessed? Is your data read-only? Does query time matter? The answers to these questions will help you decide on the best storage solution for your data.