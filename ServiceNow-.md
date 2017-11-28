# Table of Contents

https://developer.servicenow.com/app.do#!/document/content/app_store_doc_technical_best_practices_jakarta_interacting_with_the_database?v=jakarta

- [General](#general)
- [Interacting with the Database](#interacting-with-the-database)

## General

* 1.1 **Readability:** Remember. other may work with your code in the future. Ensure your code is easy to read and understand. 
* 1.2. **Comment your code:** What may seem obvious now, may not be clear later. Comments should be clear, well written and short. Unusual code snippets should always be commented along with some reasoning (for example: settimeouts, hacks).
* 1.3. **Do not modify / delete built in ServiceNow code:** Try to understand and respect the vision behind the built in code. Try to find a standard solution for your problem. It's highly improbable you need to modify / delete built in code snippets to achieve what you actually need.
* 1.4. **Use descriptive variable and function names**
* 1.5. **Verify values before you use them**
* 1.6. **Use variables to store function results**
* 1.7. **Return Values**: Get in the practice of returning some type of value when you create new functions.

## Interacting with the Database

* 2.1. **Avoid Complex GlideRecord Queries**: Rather than creating a series of addQuery() and addOrCondition() calls to obtain a result, use addEncodedQuery() to make the query easier to create and maintain.
* 2.2. **Use GlideAggregate for Simple Record Counting:** Using GlideRecord to count rows can cause scalability issues as tables grow over time, because it retrieves every record with the query and then counts them. GlideAggregate gets its result from built-in database functionality, which is much quicker and doesn’t suffer from the scalability issues that GlideRecord does.
* 2.3. **Avoid Complex Queries on Large Data Sets**: Limit the number of times you search large tables. As your instance grows, these searches can affect performance.





