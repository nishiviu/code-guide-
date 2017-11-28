# Table of Contents

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
* 1.8. **Use Self-Executing Functions**: Immediately invoked functions are typically used to create a local function scope that is private and cannot be accessed from the outside world and can define it's own local symbols without affecting the outside world.
* 1.9. **Enclose Code in Functions**

## Interacting with the Database

* 2.1. **Avoid Complex GlideRecord Queries**: Rather than creating a series of addQuery() and addOrCondition() calls to obtain a result, use addEncodedQuery() to make the query easier to create and maintain.
* 2.2. **Use GlideAggregate for Simple Record Counting:** Using GlideRecord to count rows can cause scalability issues as tables grow over time, because it retrieves every record with the query and then counts them. GlideAggregate gets its result from built-in database functionality, which is much quicker and doesn’t suffer from the scalability issues that GlideRecord does.
* 2.3. **Avoid Complex Queries on Large Data Sets**: Limit the number of times you search large tables. As your instance grows, these searches can affect performance.
* 2.4. **Let the Database Do the Work**: Whenever possible, leverage the power of the database to retrieve the proper records. Use the setLimit() method to instruct the database to return only one record. Returning one record is much faster than returning all the records.

## Client Scripting

**Client Scripts have no Condition field. This means onLoad() and onChange() scripts run in their entirety every time the appropriate form is loaded. To avoid running time-consuming scripts unnecessarily, make sure Client Scripts perform only necessary tasks.**

**With the use of a client script you can:**
*With the exception of onCellEdit Client Scripts, UI policies and Client Scripts apply to forms only. If you create UI policies or Client Scripts for fields on a form, you must use another method to ensure that data in those fields is similarly controlled in a list.*

* Disable list editing for the table.
* Create appropriate business rules or access controls for list editing.
* Create data policies.
* Create a separate onCellEdit Client Script.

* 3.1. **Use Client Scripts to Validate Data**: An excellent use for Client Scripts is validating input from the user. Validation improves the user experience because the user finds out if there are data issues before submitting the information. 
* 3.2. **Set Client Script Order**
* 3.3. **Use UI Policies Instead of Client Scripts to Set Field Attributes**

## Sources: 
* ServiceNow Developers -  (https://goo.gl/oNtTE5)
* Best Practices - (https://bestpractices.000webhostapp.com/)






