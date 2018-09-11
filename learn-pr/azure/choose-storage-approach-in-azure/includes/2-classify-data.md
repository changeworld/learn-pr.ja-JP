In an online retail business, there are different categories of data. Some data is structured, like customer financial information, and other data is unstructured, like product images. When it comes to the organization of data, there are three categories: structured, semi-structured, and unstructured. Each category of data may benefit from a different storage solution.

Here, you'll learn how to classify your data into the appropriate categories so that you can choose the correct storage solution.

## Structured data

Structured data is data that adheres to a schema, so all of the data has the same fields or properties. Structured data can be stored in a database table with rows and columns. Structured data relies on keys to indicate how one row in a table relates to data in another row of another table. Structured data is also referred to as relational data, as the data's schema defines the table of data, and the fields in the table, and the clear relationship between the two.

Structured data is straightforward in that it's easy to enter, query, and analyze. All of the data follows the same format.

Examples of structured data include:

- Sensor data
- Financial data

## Semi-structured data

Semi-structured data is less organized than structured data, and does not get stored in relational databases, as the fields do not neatly fit into tables, rows, and columns. Semi-structured data contains tags that make the organization and hierarchy of the data apparent.  

Semi-structured data is also referred to as non-relational or NoSQL data.

Examples of semi-structured data include:

- JSON files
- XML files

## Unstructured data

The organization of unstructured data is generally ambiguous just by looking at the data. Unstructured data often is delivered in files, such as photos or videos. The video file itself may have an overall structure and come with semi-structured metadata, but the data that comprises the video itself is unstructured. Therefore, photos, videos, and other similar files are classified as unstructured data.

Examples of unstructured data include:

- Media files, such as photos, videos, and audio files
- Office files, such as Word documents
- Text files
- Log files

Now that you know the differences between each kind of data, let's look at the data sets used in an online retail business, and classify them.

## Product catalog data

Product catalog data for an online retail business is fairly structured in nature, as each product has a product SKU, a description, a quantity, a price, size options, color options, a photo, and possibly a video. So, this data appears relational to start with, as it all has the same structure. However, as you introduce new products or different kinds of products, you may want to add different fields as time goes on. For example, new tennis shoes you're carrying are Bluetooth enabled, to relay sensor data from the shoe to a fitness app on the user’s phone. This appears to be a growing trend, and you want to enable customers to filter on "Bluetooth enabled" shoes in the future. You don't want to go back and update all your existing shoe data with the Bluetooth enabled property, you simply want to add it to new shoes.

With the addition of the Bluetooth enabled field, your shoe data is no longer heterogeneous, as you've introduced differences in the schema. If this were a one-off instance and the only exception you ever think you'll encounter, you could go back and normalize the existing data so that all products included a "Bluetooth enabled" field. However, this is just one of many specialty fields you envision supporting in the future; thus, the classification of this data is semi-structured. The data is organized by tags, but each product in the catalog could contain unique fields.

Data classification: **Semi-structured**

## Photos and videos

The photos and videos displayed on product pages are unstructured data. Although the media file may contain metadata, the body of the media file is unstructured.

Data classification: **Unstructured**

## Business data

Business analysts want to implement business intelligence to perform inventory pipeline evaluations and sales data reviews. In order to perform these operations, data from multiple months needs to be aggregated together, and then queried. Because of the large need to aggregate similar data, this data must be structured, so one month can be compared against the next.

Data classification: **Structured**

## Summary

There are three categories of data: structured, semi-structured, and unstructured. Understanding the differences and determining the category of your data will help you choose the correct storage solution. Structured data is organized data that neatly fits into rows and columns in tables. Semi-structured data is still organized and has clear properties and values, but there's variety to the data. Unstructured data doesn't fit neatly into tables, nor does it have a schema.