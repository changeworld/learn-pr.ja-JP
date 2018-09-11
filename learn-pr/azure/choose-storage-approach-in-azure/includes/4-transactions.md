There are many times when you need to group a series of data updates together, because a change to one piece of data needs to result in a change to another piece of data. Transactions enable you to group these updates so that if one event in a series of updates fails, the entire series can be rolled back, or undone. For example, as an online retailer you could use a transaction for the placement of an order and payment verification. The grouping of the related events ensures that you don't reduce your inventory levels until an approved form of payment is received.

Here, you'll learn what a transaction is, and whether they're required for your data.

## What is a transaction?

A transaction is a logical unit that is independently executed for data retrieval or updates.

Here's the question to ask yourself regarding whether you need a transactional database: Will a change to one piece of data in your dataset impact another? If the answer is yes, then you'll need transaction support in your database service.

Transactions are often defined by a set of four requirements, referred to as ACID guarantees. ACID stands for Atomicity, Consistency, Isolation, and Durability:

- Atomicity means all the data is updated, or all the data is rolled back to its original state.
- Consistency ensures that if something happens partway through the transaction, that part of the data is isn't left without the updates. Across the board, the data is consistent in applying the transaction or not.
- Isolation ensures that one transaction is not impacted by another transaction.
- Durability means that the changes made due to the transaction are permanently saved in the system. Committed data is saved by the system such that, even in the event of a failure and system restart, the data is available in its correct state.

When a database has ACID guarantees, it applies these principles to its transactions, and you can be assured that your transactions will be applied in a consistent manner.

## OLTP vs OLAP

Transactional databases are often called OLTP (Online Transaction Processing) systems. OLTP systems commonly support lots of users, have quick response times, and handle large volumes of data. They are also highly available (meaning they have very minimal downtime), and typically handle small or relatively simple transactions.

On the contrary, OLAP (Online Analytical Processing) systems commonly support fewer users, have longer response times, can be less available, and typically handle large and complex transactions.

The terms OLTP and OLAP aren't used as frequently as they used to be, but the comparison does make it easier to categorize the needs of your application, so it's an important concept to be aware of. 

Now that we're familiar with transactions, OLTP, and OLAP, let's walk through each of the data sets in the online retail scenario, and determine the need for transactions.

## Product catalog data

Product catalog data should be stored in a transactional database. When users place an order and the payment is verified, the inventory for the item should be updated. Likewise, if the customer's credit card is declined, the order should be rolled back, and the inventory should not be updated. These relationships all require transactions.

## Photos and videos

Photos and videos in a product catalog don't require transaction support. The only reason a change would be made to a photo or video is if an update was made, or new files were added. Even though there is a relationship between the image and the actual product data, it's not transactional in nature.

## Business data

For the business data, because all of the data is historical and unchanging, transaction support is not required. The business analysts working with the data also have unique needs in that they often require working with aggregates in their queries, so that they can work with the totals of other smaller data points.

## Summary

Ensuring that your data is in the correct state is not always an easy task. Transactions can help by enforcing data integrity requirements on your data. If your data would benefit from the principles of ACID, you should choose a storage solution that supports transactions.