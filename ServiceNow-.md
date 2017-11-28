# Table of Contents

1. [General](#general)
1. [Interacting with the Database](#interacting-with-the-database)
1. [Client Scripting](#client-scripting)
1. [Server Script Best Practices](#server-script-best-practices)
1. [Tables](#tables)
1. [Form and list](#form-and-list)
1. [UI Actions](#ui-actions)
1. [Application Navigators](#application-navigators)
1. [Security](#security)
1. [SIAM Light Quickguide](#siamlight-quickguide)
1. [Sources](#sources)

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

## Server Script Best Practices

* 4.1. **Use Script Includes instead of Global Business Rules**: If you define a function or Class in a Global Business Rule, this code will be loaded when the Business Rules execute on ANY and EVERY table in the system. This is usually highly undesirable since it adds additional processing load, which will affect the system performance. Script Includes can be used to serve the same purposes, however they are only loaded if their function call is detected. 
* 4.2. **Wrap Business Rule scripts in functions**: Because there are many Business Rules in the system, they are executed in a sequence (based on their Order value). When this happens, the contents of these scripts are compiled together into one big long sequence. If the same variable or function names are used in multiple Business Rules, they could conflict and change the flow of the program, causing unexpected behaviour.
* 4.3. **Avoid updating other records in a Before Business Rule**: Before Insert/Update Business Rules are nice because they allow us to modify the current (record) Object just before the Database operation is executed. However, you can hinder system performance if you are unnecessarily updating other records during this script. This is because an insert/update action on another table will likely invoke any applicable Business Rules configured on that table as well, and thus delay the current action being performed.
* 4.4. **Use Conditions in Business Rules**: Because it’s possible (and likely) that multiple Business Rules (of each type) have been defined, the system will effectively combine all of them into one big long script for processing (where the order is determined by the Order field on each rule). If no Condition is configured on these rules, all of them will be executed every time.
* 4.5. **Use GlideAggregate instead of getRowCount()**: Under-the-hood getRowCount() is not the most efficient means of obtaining a count. This is because the GlideRecord Class will retrieve the values of each record as well as the number of records returned. It is therefore recommended that we use the GlideAggregate Class instead. This Class operates with some similar functions and parameters as the GlideRecord Class, however its purpose is not to retrieve all of the record data but simply to provide statistics based on the records returned by a query.
*  4.6. **Avoid hardcoding values by using System Properties and Messages**: Hardcoding makes the code difficult to maintain and make changes to these values in the future.
*  4.7. **Important to know when they run**:
    *  **Display** - run after the database is queried and before the form is sent to the browser. Typical use for g_scratchpad.
    * **Before** - run after the user hits submit and thus before the database is queried. They have an implicit update : do NOT use current.update().
        * Generally used for manipulating the current record.
    * **After** - run after the database is queried.
        * Generally used for managing related tables and fields.
    * **Async** - run after the database is queried - asynchroniously.
        * Generally used for processing email, calculating SLAs and metrics, generating report fields.
* 4.8. **Should be small and specific**: Easier to debug and maintain.

## Tables
5.1. **New Table**:
* Understand when to extend or create new  
Extend an existing table when there already is one which has similar functionality to the table you are about to create.
* Give tables meaningful names
* Use singular suffixes

5.2. **Table Fields**:
* Give fields meaningful names  
    * The field name should reflect what the field represents.
    * The field label can be more descriptive.
* Re-use / consolidate fields as appropriate
* Re-label fields as needed
* Reference choice lists with the same choices  
    * Use underscores in choice list values.
    * Capitalize words in choice names.
* Create fields in appropriate classes  
* Consider inheritance  
    * Use parent table only if the field is required on other children of the parent table.
* Use correct field types  
    * Avoid using string fields for structured data.
    * Do not change field types on existing fields - instead, create a new field and migrate the data.
* Use dictionary overrides  

## Form and list
* 6.1. **Form Layout**: 
   * **Layout**
    * Should be consistent across all forms.
    * Forms should read from top to bottom.
    * Reference name/number at top left.
    * Use full width for large text fields.
    * Use sections for large forms.
   * **Use Annotations**
    * Blue info-box (question mark) icon.
   *  **Use views**
    * Can speed up performance if fewer UI policies / Client scripts get applied onto the form.
* 6.2. **List Layout**
    * Avoid HTML, large text fields, journal fields on the list  

    * Avoid reference field as the first column (avoid confusion)  

    * Avoid horizontal scrolling  

    * Consider list controlls carefully  
    * **Limit list length**
     * System Properties > System.
    * Should be 100 or less.

## UI Actions
* 7.1. **Create well-designed UI Actions ** 
* 7.2. **Give UI Actions distinctive names**
* 7.3. **Use conditions**:
    * .canWrite() to ensure security.
    * .isActive() to check if it is active.
* 7.4. **Consider the location of the UI Action within the form**:
    * Use form buttons for frequent tasks.
    * Use context menu for less frequent tasks.
    * Use the list context menu for single records.
    * Use the list choice for multiple records.
* 7.5. **Leverage Business Rules and Script Includes**  

## Application navigator

* 8.1. **Consider using menu categories**
* 8.2. **Use consistent names**:
    * i.e. "Open" for open incidents, not "Open incidents"
* 8.3. **Use separators efectively**
* 8.4. **Consider using interceptors**  
    * This may reduce the number of modules and also improve the usability of the system.
* 8.5. **Avoid using the search module** 

## Security
* 9.1. **Use roles as primary means of security**:
    * Map roles to groups.
    * A group is just a collection of users.
* 9.2. **Client scripts and UI Policies are hackable!**:
    * Use ACLs on the server to backup the security.
    * Consider using Data Policies - similar to UI Policy but on the server.
* 9.3. **Enable high security plugin**:
    * Sets a default deny policy - previous version used allow policy.
    * Do this at the beginning of the project.
    * The plugin may be on but the setting still may say "allow" - check the appropriate system property.
* 9.4. **Leverage hierarchical security**  


## SIAM Light
[SIAM Light](https://git.materna-sk.sk/materna/siam_presentation/src/master/siam_light_quick_guide/Siam%20Light%20Quick%20Guide%20-%20Outbound%20Message.md)


## Sources: 
* ServiceNow Developers -  (https://goo.gl/oNtTE5)
* Best Practices - (https://bestpractices.000webhostapp.com/)
* Client Script Best Practices - (https://goo.gl/sMtGkf)
* Coding Best Practices (https://community.servicenow.com/groups/servicenow-user-group-au-brisbane/blog/2017/05/01/servicenow-scripting-101-technical-best-practices-part-1)
* Server Script Best Practices - (https://goo.gl/U6Fzbi)





