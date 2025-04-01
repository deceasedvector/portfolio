---
title: Explanation
---

# Flat-File versus Relational

You may not think of something like an Excel workbook as a database, but it is. It's what's known as a Flat-File database. Really, a database is any logical collection of data. Indeed, you're not going to store your WordPress site's data in a Flat-File—only the truly enlightened would consider such a thing. However, large, complex data sets are stored in Flat-File databases all the time (think accounting) because there is a noted simplicity in it. 

## What is Flat-File?

With a Flat-File, all your data is in one place and easy to find. 
You don't necessarily need to learn a complicated query language to view the data, you just need to open Excel or Access, and you're good to go.
Small businesses may not have the resources or capacity to hire a database engineer to craft something like a Relational database. 
Most who use computers day-to-day have familiarity with spreadsheets and can quickly create a low-cost, simple Flat-File solution that fits the needs of many businesses. 

What Flat-File gains in simplicity, however, it loses in efficiency and scalability. 
When it comes time to update the data—for example, a customer changes either their name or email on their account—with a flat file, the business would have to locate everywhere in the database where the customer's redundant data is and manually update each entry. 
Perhaps they don't change every record they should; this can lead to data inconsistency and errors. 

![](../assets/images/mermaid-diagram-2025-04-01-140838.svg)

As the Flat-File database grows, it becomes unwieldy with thousands of records that can bog down the user experience. 
However, there is a solution. 
Although it may not be perfect for every use case, it has become relatively standard in many industries, and that's the Relational database. 

## What is Relational?

The problem with Flat-File databases comes down to the fact that they are not "normalized." 
In 1970, an ingenious fellow named E. F. Codd at IBM determined a mathematical solution for this. 
His standard proved that breaking up duplicate data into smaller, logical tables/data sets allows for cleaner data inserts, updates, and deletions. 
These databases are called Relational. 

If you've ever used an app or website, the chances are that the underlying database was a Relational one. 
By storing similar data together and assigning each row a unique identifier, you can categorize the data. 
You can insert new data without affecting the old data. 
Likewise, you can update the existing data without worrying about data duplication and inconsistency. 

![](../assets/images/mermaid-diagram-2025-04-01-140907.svg)

With Relational databases, there's also an option of collaboration where many users can contribute data without you having to worry about the data drifting out of sync.
Although Relational databases are the perfect solution for many, it does come with its disadvantages. 
While the ease of data retrieval is an advantage, many are intimidated by query languages, such as SQL, being their primary tool. 

## Flat-File versus Relational?

There's a level of complexity and abstraction that is inherent to Relational databases. 
The data isn't just there in front of you; it is obfuscated. 
And with so many different tables in the mix, it eventually makes any system more complicated. 

Many small businesses may not have the resources to implement and manage a Relational database, so it becomes cost-prohibitive. 
Hiring a database architect can be costly, but the physical storage necessary to maintain the vast number of tables and data can also be quite expensive. 

Like everything in this world, there are no perfect solutions. 
Everyone's situation is different. 
It's unwise to claim that you should never use Flat-File databases or that Relational databases are the key to every problem. 
The key is to analyze your needs and pick the best option for you; maybe it's Flat-File, perhaps Relational, or maybe your solution resides in another type of database like a Distributed or Hierarchical. 
There are so many options out there; you just have to find them. 