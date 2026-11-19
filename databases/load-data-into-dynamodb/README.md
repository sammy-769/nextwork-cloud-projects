<picture><source media="(prefers-color-scheme: dark)" srcset="https://nextwork.ai/static/nextwork-logo-paper.svg" /><img src="https://nextwork.ai/static/nextwork-logo-leather.svg" alt="NextWork" width="300" /></picture>

# Load Data into DynamoDB

**Project Link:** [View Project](https://nextwork.ai/projects/8bd82466-f5ee-569e-9125-2c4c6ce7a07f)

**Author:** Sammy Shah Saleh

---

## Introducing Today's Project!

### ✍️ What are we here to do today?

In this project, I learned how to work with Amazon DynamoDB, a fully managed NoSQL database service. I created DynamoDB tables, uploaded sample data using AWS CloudShell, and practised viewing and updating existing records. This gave me practical experience with storing, organising and managing data in DynamoDB.

### ✍️ What are we doing in this step?

In this step, I created an Amazon DynamoDB table from scratch to store and manage data.

### What is Amazon DynamoDB?

Amazon DynamoDB is a fully managed NoSQL database service provided by AWS. It stores data in tables, which contain items and attributes. It is useful because it offers flexible data structures, low-latency performance and automatic scaling, making it suitable for applications that need to handle large amounts of data.

### How I used Amazon DynamoDB in this project

I created four DynamoDB tables: ContentCatalog, Forum, Post and Comment. I used AWS CloudShell and the AWS CLI to create the tables and upload sample data from JSON files. I then explored the stored items, examined their attributes and updated data to understand how DynamoDB manages information.

### One thing I didn't expect in this project was...

I was surprised by how DynamoDB organises and stores data. I did not expect items within the same table to have different attributes. Seeing Project and Video items with different structures helped me understand the flexibility of a NoSQL database.

### This project took me...

This project took me approximately one hour to complete.

## Create a DynamoDB table

Data in a DynamoDB table is organised into \*\*items and attributes\*\*. Each item represents a single record, while its attributes store the individual pieces of information about that record. Every item must have a primary key, which uniquely identifies it within the table. A primary key can consist of a partition key alone or a combination of a partition key and a sort key.

### ✍️ What are we doing in this step?

In this step, I used AWS CloudShell to interact with Amazon DynamoDB through the command line. I ran AWS CLI commands to create and manage DynamoDB tables directly from the AWS Management Console.

An attribute is a piece of data stored in a DynamoDB item. Each attribute has a name and a value. For example, a user record might contain attributes such as \`user\_id\`, \`username\`, \`email\` and \`age\`. These attributes describe the item and store its information.

![Create a DynamoDB table](https://nextwork.ai/sammy769/uploads/8bd82466-f5ee-569e-9125-2c4c6ce7a07f_a3cefee0)

## Read and Write Capacity

Read capacity units (RCUs) and write capacity units (WCUs) measure the read and write throughput available to a DynamoDB table when it uses provisioned capacity mode. One RCU supports one strongly consistent read per second, or two eventually consistent reads per second, for an item up to 4 KB in size. One WCU supports one write per second for an item up to 1 KB in size. Larger items consume additional capacity units.

The DynamoDB Free Tier includes 25 GB of data storage, 25 provisioned RCUs and 25 provisioned WCUs per month for tables using the DynamoDB Standard table class. It also includes 2.5 million DynamoDB Streams read requests and 1 GB of data transfer out per month. These allowances are subject to AWS Free Tier eligibility and usage conditions. Any usage beyond the included allowances, or features not covered by them, may incur additional charges.

![Read and Write Capacity](https://nextwork.ai/sammy769/uploads/8bd82466-f5ee-569e-9125-2c4c6ce7a07f_ef47dd8f)

## Using CLI and CloudShell

AWS CloudShell is a browser-based shell provided by AWS that allows me to run commands directly from the AWS Management Console. It comes with the AWS CLI and automatically uses temporary credentials associated with my signed-in AWS identity. This allowed me to interact with AWS services without installing or configuring command-line tools on my local machine.

The AWS Command Line Interface (AWS CLI) is a tool that allows me to interact with and manage AWS services by running commands in a terminal or command-line environment. Instead of using the AWS Management Console, I can use commands to create, configure and manage resources such as DynamoDB tables, S3 buckets and EC2 instances.

I used the \`aws dynamodb create-table\` command to create four DynamoDB tables: \`ContentCatalog\`, \`Forum\`, \`Post\` and \`Comment\`. The \`--table-name\` option specified each table's name, while \`--attribute-definitions\` defined the key attributes and their data types `S` for String and `N` for Number). The `--key-schema` option specified each table's primary key structure: `HASH` represents the partition key, and `RANGE` represents the sort key. I also used `--provisioned-throughput` to configure one read capacity unit and one write capacity unit for each table. Finally, `--query "TableDescription.TableStatus"` filtered the command's output to display the table's status.

### ✍️ What are we doing in this step?

In this step, I loaded data into my DynamoDB tables using AWS CloudShell. I then viewed the stored items and practised updating their attributes to understand how to manage and modify data in DynamoDB.

![Using CLI and CloudShell](https://nextwork.ai/sammy769/uploads/8bd82466-f5ee-569e-9125-2c4c6ce7a07f_81e0258b)

## Loading Data with CLI

I used \`curl -O\` to download a ZIP file containing the sample data for this project. I then used `unzip` to extract the files and `cd` to enter the extracted directory. After that, I ran `aws dynamodb batch-write-item` with the corresponding JSON file for each table. These commands sent batch write requests to DynamoDB to insert the sample items into the \`ContentCatalog\`, \`Forum\`, \`Post\` and \`Comment\` tables.

![Loading Data with CLI](https://nextwork.ai/sammy769/uploads/8bd82466-f5ee-569e-9125-2c4c6ce7a07f_791c600b)

## Observing Item Attributes

![Observing Item Attributes](https://nextwork.ai/sammy769/uploads/8bd82466-f5ee-569e-9125-2c4c6ce7a07f_b481c731)

The Project item contains the following attributes:

\- \`Id\`: 1 (Number, partition key)\
\- \`Authors\`: A list of authors\
\- \`ContentType\`: Project (String)\
\- \`Difficulty\`: Easy peasy (String)\
\- \`Price\`: 0 (Number)\
\- \`ProjectCategory\`: Storage (String)\
\- \`Published\`: True (Boolean)\
\- \`Title\`: Host a Website on Amazon S3 (String)\
\- \`URL\`: aws-host-a-website-on-s3 (String)

These attributes describe the project, its category, difficulty, publication status and other details.

The Video item contains six attributes: \`ContentType\` (Video), \`Price\` (Number), \`Services\` (List), \`Title\` (String), \`URL\` (String) and \`VideoType\` (String). Unlike the Project item, it does not contain attributes such as \`Difficulty\`, \`ProjectCategory\`, \`Published\` or \`StudentsComplete\`. This shows that DynamoDB items in the same table can have different attributes.

![Observing Item Attributes](https://nextwork.ai/sammy769/uploads/8bd82466-f5ee-569e-9125-2c4c6ce7a07f_9078b333)

## Benefits of DynamoDB

DynamoDB is more flexible than relational databases because it does not require every item in a table to have the same attributes. As I observed in the ContentCatalog table, Project and Video items had different attributes. This allows me to store different types of data in the same table without defining a fixed set of columns for every item.

DynamoDB can perform faster for certain workloads because it uses a distributed architecture and is designed to provide low-latency access to data at scale. Its key-value and document data models allow it to retrieve items efficiently using their primary keys, without requiring complex joins between tables. However, performance depends on the workload, data design and access patterns; relational databases can be more suitable for queries involving complex relationships and joins.

![Benefits of DynamoDB](https://nextwork.ai/sammy769/uploads/8bd82466-f5ee-569e-9125-2c4c6ce7a07f_b481c730)

### ✍️ What do you think this table is displaying?

This table displays a catalogue of NextWork learning content, including projects and videos. Each item has an ID and attributes describing it, such as its title, author, content type, difficulty, price, related AWS services, publication status and URL. It allows me to see how different types of content are organised and stored as individual items in DynamoDB.
