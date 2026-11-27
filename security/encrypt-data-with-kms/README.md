<picture><source media="(prefers-color-scheme: dark)" srcset="https://nextwork.ai/static/nextwork-logo-paper.svg" /><img src="https://nextwork.ai/static/nextwork-logo-leather.svg" alt="NextWork" width="300" /></picture>

# Encrypt Data with AWS KMS

**Project Link:** [View Project](https://nextwork.ai/projects/716e7f81-84d1-51d6-8945-d4860caebf77)

**Author:** Sammy Shah Saleh

---

## Introducing Today's Project!

In this project, I explored AWS Key Management Service (KMS) and learnt how it helps protect data. I used KMS to encrypt a DynamoDB table and tested how encryption and key permissions affect access to stored data.

### ✍️ What are we doing in this step?

In this step, I created an encryption key using AWS KMS and configured its permissions by setting up the key administrator and key user. This allowed me to control who could manage the key and who could use it for encryption and decryption.

### Tools and concepts

I learnt how to use AWS Key Management Service (KMS) to create and manage encryption keys, configure key policies and control user permissions. I also learnt how DynamoDB uses encryption at rest and how KMS permissions determine whether an IAM user can decrypt and view encrypted data. Finally, I explored the difference between encryption and access control.

### Project reflection

It took me approximately one hour to complete this project.

I chose to do this project today because I wanted to understand how AWS protects stored data and how encryption keys and permissions work together. I also wanted to gain practical experience with AWS KMS and DynamoDB security.

## Encryption and KMS

Encryption is the process of converting readable data (plaintext) into an unreadable format (ciphertext) using an encryption key. It protects information from unauthorised access. Only someone with the appropriate key can decrypt the data and make it readable again.

AWS Key Management Service (KMS) is a managed AWS service that allows me to create, manage and control encryption keys. I can use these keys to encrypt and decrypt data and control who has permission to use or manage them.

I chose a symmetric key because symmetric encryption is generally faster and more efficient than asymmetric encryption, especially when protecting large amounts of data. This makes it a suitable choice for encrypting my DynamoDB table.

![Encryption and KMS](https://nextwork.ai/sammy769/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_a2b3c4d5)

### ✍️ What are we doing in this step?

In this step, I created a DynamoDB table and configured its encryption using my AWS KMS customer-managed key to protect the data stored in the table.

## Encrypting Data

Amazon DynamoDB is a fully managed NoSQL database service from AWS that stores and retrieves data with low latency and automatic scaling.

DynamoDB offers three encryption key options:

1. **AWS owned key:** Managed entirely by AWS. I don't need to manage the key or pay for its use.
2. **AWS managed key:** Managed by AWS KMS on my behalf. AWS handles key management and rotation.
3. **Customer managed key:** I create and manage the key in AWS KMS. I have more control over permissions, rotation and key policies.

### ✍️ What are we doing in this step?

In this step, I added sample data to my DynamoDB table and checked whether I could view the stored items after enabling encryption with my KMS key.

![Encrypting Data](https://nextwork.ai/sammy769/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_q8r9s0t1)

## Data Visibility

Rather than directly controlling access to DynamoDB, KMS manages permissions by using key policies and IAM policies. These policies determine which users and roles can perform actions such as encrypting and decrypting data with the key. In my test, the IAM user could access DynamoDB but could not decrypt the data because it lacked the required KMS permission.

Despite encrypting my DynamoDB table, I could still see its items because I was using an IAM user with permission to access the KMS key. DynamoDB uses transparent encryption at rest, which means AWS automatically encrypts data when it is stored and decrypts it when an authorised user accesses it. The encryption happens behind the scenes, so I can view the data normally without manually decrypting it.

### ✍️ What are we doing in this step?

In this step, I created a test IAM user and granted it full access to DynamoDB without giving it permission to use my KMS key. I did this to test how KMS permissions affect access to encrypted DynamoDB data.

![Data Visibility](https://nextwork.ai/sammy769/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_c0d1e2f3)

## Denying Access

I created an IAM user with access to the AWS Management Console and attached the existing `AmazonDynamoDBFullAccess` policy directly to grant the user full access to DynamoDB.

### ✍️ What are we doing in this step?

In this step, I logged into the AWS Management Console using my test IAM user and attempted to view the items in my encrypted DynamoDB table to test whether the user could access the data without permission to use my KMS key.

I received an error stating that I did not have permission to perform `kms:Decrypt`. This happened because my test IAM user had permission to access DynamoDB but did not have the required permission to use the KMS key. When DynamoDB attempted to decrypt the table data on the user's behalf, AWS KMS denied the request.

![Denying Access](https://nextwork.ai/sammy769/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_w0x1y2z3)

## EXTRA: Granting Access

I scrolled down to the **Key policy** section and selected **Add** next to **Key users**. I then added `nextwork-kms-user` as an authorised key user, granting the IAM user permission to use the KMS key.

I returned to the AWS Management Console using my test IAM user and refreshed the DynamoDB table page. I could now view the stored items because I had granted the user permission to use the KMS key to decrypt the data.

I would use **encryption** to protect data from being read by unauthorised parties, especially if the data is stored or transmitted. I would use **access control tools**, such as IAM policies, to decide who can access AWS resources and what actions they can perform. They work best together: encryption protects the data, while access control manages who can access it.

![EXTRA: Granting Access](https://nextwork.ai/sammy769/uploads/716e7f81-84d1-51d6-8945-d4860caebf77_feffb2fb8)
