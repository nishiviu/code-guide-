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
* 1.10. **Use consistent styling**

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
* 3.4. **Avoid global Client Scripts**: Client Scripts configured on the Global Table will be loaded on every single table form in the system, regardless of whether any of the code is actually needed. This can seriously impact system performance and therefore should be avoided. 
* 3.5. **Avoid using DOM (manipulating elements via the Document Object Model)**: Te HTML in ServiceNow’s table forms is maintained by ServiceNow, and could (and likely will) change with future releases. If the id value changed, your code will fail.
* 3.6.: **Use Asynchronous calls via getReference() or GlideAjax**: Using Asynchronous AJAX calls (e.g. using Callback functions) will avoid freezing the web browser while the data transfer occurs, and results in a much nicer user experience.
Although the GlideRecord Class is available on the Client-side, ServiceNow recommends using GlideAjax instead. First of all, unlike the Server-side equivalent, the Client-side version of the GlideRecord Class will not provide the same functionality (e.g. no dot-walking and associated function calls). Second, whenever the next() or get() functions are called, a Synchronous AJAX call will be made to the Server and this will momentarily freeze the web browser until the data is returned. Using the Client-side GlideRecord Class is actually very similar to using getReference()
* 3.7. **Use g_scratchpad to minimise server calls**: Often, we need to use data in our Client Scripts, which is available on the Server, but not readily available on the Client. If these values are known when the form is loaded, they can be retrieved by a Display Business Rule, and made available to Client Scripts by storing them on a global JavaScript Object called g_scratchpad. Such examples include System Properties or field values from Database records. By first storing them on the g_scratchpad object when the form is loaded, it avoids the need to use an AJAX call to request and receive the required data.
* 3.8. **Set the Display Value as well as the Value on Reference fields**: When setting the value of a Reference field via script, we need to use (at minimum) the sys_id value of the record we want to select. When this happens, ServiceNow will automatically make a Synchronous AJAX call to the Server to determine the Display Value for the record we’ve selected, and then display this value on the form. As per my previous points on Client-Server communication and Asynchronous vs Synchronous AJAX calls, it is Best Practice to avoid Synchronous calls altogether, and to minimise server calls in general. With this in mind, the Best Practice when setting a value into a Reference field is to also set the Display Value at the same time, as this will prevent the additional Synchronous call to the Server. This can be achieved by adding an additional argument to the setValue() function. 
* 3.9.: 

## Sources: 
* ServiceNow Developers -  (https://goo.gl/oNtTE5)
* Best Practices - (https://bestpractices.000webhostapp.com/)
* Client Script Best Practices - (https://goo.gl/sMtGkf)
* Coding Best Practices (https://community.servicenow.com/groups/servicenow-user-group-au-brisbane/blog/2017/05/01/servicenow-scripting-101-technical-best-practices-part-1)





