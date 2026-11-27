<picture><source media="(prefers-color-scheme: dark)" srcset="https://nextwork.ai/static/nextwork-logo-paper.svg" /><img src="https://nextwork.ai/static/nextwork-logo-leather.svg" alt="NextWork" width="300" /></picture>

# Query Data with DynamoDB

**Project Link:** [View Project](https://nextwork.ai/projects/0001cf4c-8256-52ae-b09c-6e99ed36462f)

**Author:** Sammy Shah Saleh

---

## Introducing Today's Project!

### ✍️ What are we here to do today?

In this project, I explored Amazon DynamoDB and learnt how to work with NoSQL databases in AWS. I created tables, loaded and viewed data, and practised querying and retrieving specific items using partition keys and sort keys. I also used AWS CloudShell and the AWS CLI to manage my tables and perform database operations. Finally, I explored DynamoDB transactions and learnt how to perform multiple related operations together, such as adding a comment and updating the corresponding comment count, while maintaining data consistency.

### ✍️ What are we doing in this step?

In this step, I used AWS CloudShell to run AWS CLI commands and create DynamoDB tables `using AWS CloudShell in the AWS Management Console`

### ✍️ What are we doing in this step?

In this step, I loaded sample data into my DynamoDB tables using AWS CloudShell. I then viewed the stored items and practised updating their attributes.

### ✍️ What are we doing in this step?

In this step, I ran basic queries on my DynamoDB tables to retrieve specific items. I used partition keys and sort keys to find and filter data based on the table's primary key structure.

### ✍️ What do you think this table is displaying?

This table displays comments posted by users on specific NextWork forum discussions. Each item contains an `Id` identifying the discussion, a `CommentDateTime` showing when the comment was posted, a `Message` containing the comment text, and a `PostedBy` attribute identifying the user who wrote it. The combination of the partition key (`Id`) and sort key (`CommentDateTime`) allows multiple comments to be organised by discussion and retrieved in chronological order.

### What is Amazon DynamoDB?

Amazon DynamoDB is a fully managed NoSQL database service provided by AWS. It stores data in tables containing items and attributes. It is useful because it offers flexible data structures, low-latency performance and automatic scaling, making it suitable for applications that need to manage large amounts of data.

### How I used Amazon DynamoDB in this project

I used AWS CloudShell and the AWS CLI to query my DynamoDB tables using partition keys and sort keys. I retrieved specific items, explored relationships between the Forum, Post and Comment tables, and used a DynamoDB transaction to add a new comment while increasing the related forum's comment count.

### One thing I didn't expect in this project was...

I was surprised by DynamoDB transactions and how they allow multiple operations across different tables to be handled as a single unit. I found it interesting that I could add a comment and update its related forum's comment count together, helping maintain data consistency.

### This project took me...

This project took me approximately one and a half hours to complete.

## Querying DynamoDB Tables

A partition key is the primary key attribute that DynamoDB uses to determine how data is distributed across its storage partitions. It identifies items within a table when used on its own.

A sort key is an additional primary key attribute used alongside the partition key to organise and distinguish items within the same partition key value.

![Querying DynamoDB Tables](https://nextwork.ai/sammy769/uploads/0001cf4c-8256-52ae-b09c-6e99ed36462f_d105b0b0)

## Limits of Using DynamoDB

My query resulted in an error because I did not provide a value for the partition key (`Id`). DynamoDB requires a partition key value when performing a query, so leaving it empty prevented the query from running successfully.

We can identify the comments posted in a particular discussion, see what users have written, find out who posted each comment and examine when the comments were submitted.
We cannot easily retrieve all comments written by a particular user across different discussions or search for specific words within comment messages. These operations require access patterns or indexes that are not directly supported by the table's primary key structure.

### ✍️ What are we doing in this step?

In this step, I used AWS CLI commands in AWS CloudShell to query my DynamoDB tables and retreive specific items.

### ✍️ Before read the explanation for this query, what do you think it'll return?

I expected this command to return no item because my `ContentCatalog` table did not contain an item with the partition key `Id` equal to `101`. The command attempted to retrieve that specific item and, if found, return only its `Title`, `ContentType` and `Services` attributes.

![Limits of Using DynamoDB](https://nextwork.ai/sammy769/uploads/0001cf4c-8256-52ae-b09c-6e99ed36462f_cb3e260c)

## Running Queries with CLI

I ran the `aws dynamodb get-item` command to retrieve the item with the partition key `Id` equal to `202` from the `ContentCatalog` table. I used three options:

- `--key`: Specifies the primary key value `Id = 202`) of the item I wanted to retrieve.

- `--projection-expression`: Limits the returned attributes to `Title`, `ContentType` and `Services`, rather than returning the entire item.

- `--return-consumed-capacity TOTAL`: Reports the total read capacity consumed by the operation.

### ✍️ What are we doing in this step?

In this step, I used AWS CLI commands to work with related data across multiple DynamoDB tables. I also learnt how to update items in two tables within a single command.

![Running Queries with CLI](https://nextwork.ai/sammy769/uploads/0001cf4c-8256-52ae-b09c-6e99ed36462f_18379f04)

### ✍️ How are your DynamoDB tables related?

My DynamoDB tables are related through the forum discussion structure. Each `Forum` can contain multiple `Posts`, and each `Post` can contain multiple `Comments`. The `Forum` table also stores counts of the posts and comments associated with each forum, while the `Comment` table stores the actual comments. This means that when I add a new comment, I need to update the corresponding comment count in the related `Forum` item to keep the data consistent.

![Running Queries with CLI](https://nextwork.ai/sammy769/uploads/0001cf4c-8256-52ae-b09c-6e99ed36462f_733d9399)

## Transactions

A transaction is a group of database operations that are treated as a single unit. In DynamoDB, transactions provide ACID properties, meaning the operations either all succeed or all fail. This helps maintain data consistency when multiple related items need to be changed together.

I used the `aws dynamodb transact-write-items` command to perform two operations in a single transaction. First, I used `Put` to add a new comment to the `Comment` table. Second, I used `Update` to increase the `Comments` attribute in the related `Forum` item by one ``sing the `ADD` update expression.``I also used `--client-request-token` to provide an idempotency token. This helps prevent the same transaction from being applied more than once when an identical request is retried within DynamoDB's 10-minute idempotency window. This ensured that the new comment and the updated comment count were handled together.

![Transactions](https://nextwork.ai/sammy769/uploads/0001cf4c-8256-52ae-b09c-6e99ed36462f_2f65f83e)
