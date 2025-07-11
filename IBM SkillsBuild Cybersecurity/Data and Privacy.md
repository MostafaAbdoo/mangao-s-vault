## Types of Data
**Public data** is data that anyone can access, use, and redistribute without restriction
**Confidential data** is any data that an organization protects from unauthorized access
**Proprietary data** is organization-owned or generated data relevant to the organization’s products or actions that must remain confidential.
**Private data** is data about a person and their private life that other parties should not be able to collect, use, or disclose unless authorized.
**Personally identifiable information (PII)** is private data that you can use to identify someone. Birthdays, addresses, phone numbers, government-issued ID numbers, and headshots are common examples.
**Protected health information (PHI)** is private data included in someone’s medical records that you can use to identify the person.

When classifying data, recall the relationships between its types. Confidential data includes proprietary and private data, and private data includes PII and PHI.

-------------------------------------------------------

**Data security** refers to how an organization protects confidential data from unauthorized access, disclosure, or destruction.
**Data privacy** is a specific type of data security focused only on preventing unauthorized collection, disclosure, or use of customers’ and employees’ private data.

------------------------------------
**Administrative controls** are guidelines, policies, and procedures written to meet and enforce security goals.
**Physical controls** are devices or structures designed to restrict access to areas or devices containing sensitive data
**Technical controls** are the hardware or software that helps secure data or processes.

**Data loss prevention systems** :
- A **file-level DLP** system identifies sensitive files in a file system. The organization’s security policy specifies attributes that these files must have for security purposes.
- A **network DLP** system protects data at rest, in motion, or in use on an organization’s network. It monitors all data transfers, such as emails, instant messages, or file downloads, for violations of privacy and security policies.
- A **cloud DLP** system is an extension of a network DLP system that protects data stored in cloud repositories. It detects and encrypts any sensitive data before it is stored in the cloud.
- An **endpoint DLP** system monitors all endpoints for data loss or leakage.
---------------------------------
**Full backups** copy the entire contents of your system or device. You back up **all** data, even older data that you’ve already backed up.
**Incremental backups** capture only the changes made since the last backup. First, you perform a full backup. But additional backups include only the changes made since the last backup
**Differential backups** combine the concepts of full and incremental backups. First, you perform a full backup. Next, each additional backup includes only the changes made since the last **full** backup. 

Imagine that you back up data once a day. Your Day 1 backup is a full backup. Your Day 2 backup includes only the changes made since that Day 1. Your Day 3 backup also includes all changes made since Day 1, **including** changes already backed up on Day 2. So, with differential backups, your Day 3 archive file is larger than what you’d create using the incremental approach.
