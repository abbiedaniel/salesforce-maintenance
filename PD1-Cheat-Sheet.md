# PD1 Cheat Sheet

<details>
	<summary>Developer Fundamentals</summary>

# Developer Fundamentals

## Standard Relationships & Fields

- **Master-detail**: on the child object, child obj do not have owners so they can't be used with queues
- **Lookup**: on the child object
- **Junction**: child object with two master detail fields, inherits security of first master
- **External Lookup**: external parent object
- **Indirect Lookup**: external child object
- **Roll Up Summary**: on the master, `COUNT/SUM/MIN/MAX`, works on lookups: Opp-Opp Product, Account-Opp, Campaign-Campaign Members
- **Validation Rules**: don't operate on parent-child relationships, can't be used on record deletion
- **Formula Field**: can't be used in a roll up summary field if it references a field on a different object or if `NOW()` or `TODAY()` methods are in the formula
- **Cross-Object Formula Field**: created on child to reference data from parent, can't be used in roll-up summary fields

## Save Order of Execution

1. Record Initialization
2. **System** Validation
3. Before Save **Flows**
4. Before **Triggers**
5. **Validation** Rules and System Validation
6. **Duplicate** Rules
7. _**Save** to database but not committed_
8. After **Trigger**
9. **Assignment** Rules (Cases & Leads)
10. **Auto-response** Rules (Cases & Leads)
11. **Workflow** Rules
12. **Escalation** Rules (Cases)
13. Flow Automation (Processes & Flows launched by processes or workflows)
14. After Save **Flows**
15. Entitlement Rules (Cases & Word Orders)
16. Roll Up Summary Fields & Cross-Object Workflow
17. Criteria-Based Sharing Rules
18. _Commit to database_
19. Post Commit Logic like sending emails, outbound message or future methods

**S**am's **F**amily **T**ook **V**alerie **D**own **S**outh **T**o **A** **A**uto **W**orkshop's **E**nclosed **F**oyer.

## Governor Limits
- **SOQL Queries**: 100
- **DML**: 150
- When a governor limit is reached, all changes are rolled back up to the error, a limit exception is thrown and the entire execution process is exited.

## Model View Controller Architecture
- **Model**: where data is saved
- **View**: how data is visualized
- **Controller**: how data is manipulated/logic

## Data Imports & Exports

|   | Data Import Wizard      | Data Loader |
| ----------- | ----------- | ----------- |
| **Max Records** | 50K    | 5M       |
| **Pros** | - Choose whether to trigger workflow rules   | - Can save mapping for later use <br> - Can delete and export data    |
| **Cons** |  - Can only insert, update or upsert <br> - Can't use on product and opportunities <br> - Can't save mappings <br> - Can't schedule imports   | - No option to turn off workflow rules |

<br>

![image](https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/41026379-2bf2-46e1-a7b5-28dab8514a1b)

## Schema Namespace
- `DescribeSObjectResult` Methods
	- `getLabel()` label may or may not match object name, `getName()` name of object
 	- `isDeletable()`, `isAccessible()` and `isCreatable()`
  	- `getSobjectType()`
  	- `getRecordTypeInfos()`, `getRecordTypeInfosByDeveloperName()`, `getRecordTypeInfosById()`, and `getRecordTypeInfosByName()`
- `DescribeFieldResult` Methods
	- `getDigits()` for ints, `getScale()` for doubles 
 	- `getLength()` for max size of field in char
  	- `getLabel()`
  	- `getSObjectType()`
  	- `getDescribe()` instantiates a field describe result object

## Object & Field Level Security
- `WITH SECURITY_ENFORCED`: enable FLS and object level secuirty permissions checking in a SOQL query. TYhrows an exception if a field or object referenced is inaccessible.
- `Security.stripInaccessible(AccessType.CREATABLE, sourceRecords)`: strip fields from SOQL results that fail FLS checks. No exception is thrown.
- `Contact.sobjectType.getDescribe().isCreateable()` and `Contact.LastName.getDescribe().isReadable()`: respect the object and field access of the running user. This can also be accomplished with `Schema.sObjectType.Contact.isDeletable()`.

<br>

</details>

<details>
	<summary>Process Automation & Logic</summary>

# Process Automation & Logic


<details>
	<summary>Apex</summary>
	
## About Apex
- Apex is a programming language that uses Java-like syntax and acts like database stored procedures.
- **Hosted**: Apex is saved, compiled, and executed on the server—the Lightning Platform.
- **Object oriented**: Apex supports classes, interfaces, inheritance, abstraction, polymorphism, and encapsulation.
- **Strongly typed**: Apex validates references to objects at compile time.

## Apex Class Definition & Members
- Access Modifiers: `global`, `public`, `private`, `protected`
- Sharing Context: `with sharing`, `without sharing`, `inherited sharing`
- Class Keywords: `implements`, `extends`
- Interface keywords: `abstract`, `virtual`, `interface`
- Constructors
- Member variables
- Member properties
- Methods

## Apex Data Types
- **String**: 'hello world'
- **Boolean**: true or false
- **Integer**: 7
- **Decimal**: 7.7
- **Double**: 3.14159265
- **Id**: 006Hs00001KsrsSIAR
- **Date**: 2024-01-23
- **DateTime**: 2024-01-23 03:03:03
- **Time**: 02:39:39.217Z
- **Blob**: binary data
- **Enum**: store set of id that are accessed one at a time
- **List**: `List<String> colorsList = new List<String>{'red'};`
- **Set**: `Set<Integer> intSet = new Set<Integer>();`
- **Map**: `Map<Id, String> idList = new Map<Id, String>();`

## Apex Class Use Cases
- **Trigger Handler Class:** `public class AccountTriggerHandler {}`
- **Lightning Web Controller Class:** `public class MedsListController{}`
- **Visualforce Controller Class:** `public class EditPageController{}`
- **Exception Class:** `public class MyCustomException extends Exception{}`
- **Test Data Factory Class:** `@isTest public class TestDataFactory{}`
- **Test Class:** `@isTest private class AccountTriggerHandlerTest{}`
- **Invocable Methods for Flows & Process Builders to Call:** `@InvocableMethod(callout = true label = 'methodName' description = 'description' category = 'DML')specialMethod(){}`
- **Web Services Methods for External Services to Call:** `@future(callout=true) static void myfutureMethod(){}`

## Apex Triggers
- **Before triggers** are used to update or validate record values on the same record/object before they’re saved to the database.
- **After triggers** are used to access field values, such as Ids, that are set by the system and to effect changes in other or related records or objects. (Records that trigger the after trigger are **read-only**).
- **Trigger Event Context:** before insert/update/delete and after insert/update/delete/undelete
- **Trigger Definition:** `trigger AccountTrigger on Account(before update){}`
- **Trigger Error Handling:** `addError('Error!')` prevents the dml operation from occurring on the field or record
- **Trigger Context Variables:**
	- update: `Trigger.old`, `Trigger.oldMap`,  `Trigger.new` & `Trigger.newMap` 
	- delete: `Trigger.old` & `Trigger.oldMap` 
 	- undelete: `Trigger.new` & `Trigger.newMap` (before undelete is not a thing so this is only for after undelete triggers)
  	- insert: `Trigger.new` & **after** insert: `Trigger.newMap` (before insert won't have access to the new map since the record id won't exist yet)
- **Other Context Variables available in all triggers:** `isBefore`, `isAfter`, `isUndelete`, `isDelete`, `isUpdate`,  `isInsert`, `isExecuting`, `operationType`, `size`

 
## Asynchronous Apex
- **Future methods:** separate transactions, web service callouts. Must have `@future` annotation. 
- **Batch Apex:** large data processing, data cleansing or archiving. The class must `implements Database.Batchable<sObject>, Database.Stateful`. Must have `start(Database.BatchableContext bc)`, `execute(Database.BatchableContext bc, List<sObject> scope)` and `finish(Database.BatchableContext bc)` methods. Use `Database.executeBatch(ExampleClass, batchSize)` to run the batch class.
- **Queueable Apex:** sequential processing, external web service callouts. Must have `execute(QueueableContext context)` method and  `implements Queueable` on the class definition. Use `System.enqueJob(ExampleQueueableClass)` to run the class.
- **Scheduled Apex:** scheduled processing, weekly or monthly. Must have `execute(SchedulableContext SC)` method and `implements Schedulable` in the class definition. It can call other async apex classes and schedule them. Use `System.schedule('Job Title', scheduledDateTime, ExampleScheduledApexClass)` to schedule the class or schedule the class in setup.
- **Monitor Jobs:** View in progress or completed jobs in **Apex Jobs** and view future scheduled jobs in **Scheduled Jobs**. The calling methods for batch, queueable and scheduled apex return a job id, which can be used to query for the `AsyncApexJob` object. Example: `ID jobID = System.enqueueJob(queueClass);`
`AsyncApexJob job = [SELECT Id, Status, NumberOfErrors FROM AsyncApexJob WHERE Id = :jobID];`

## Exception Handling
`throw` statements can be used to generate exceptions. 
`try`, `catch`, and `finally` can be used to gracefully recover from an exception.

```apex
try {
	// something you think could fail or error out like an dml operation
	Database.SaveResult results = Database.insert(listToInsert, allOrNone, accessLevel);
	/// allOrNone boolean allows for partial sucess if an error is thrown when it is set to false
} catch ( DmlException ex ){
	// if a DmlException occurs, the other catch blocks do not execute

} catch ( Exception ex ){
	// call a custom exception method to handle the exception
	Trigger.HandlerClass.throwException(ex.getMessage()); // gets string message of an exception 
} finally {
	// optional finally block
	// runs after the try block successfully runs or the catch block finishes executing
}
```

## Custom Exception Class & Method

```apex
public class AccountTriggerException extends Exception {
	// exception class name must end with "exception"
}

// you can instantiate this class with
new AccountTriggerException(); // no params
new AccountTriggerException('error message to display'); // string param
new AccountTriggerExceptions(e); //single exception param
new AccountTriggerException('error message to display', e); // with string and exception param
```

```apex
public static void throwException(String message){
	System.debug(message);
	throw new AccountTriggerException(message);
}
```

## Built-In Exceptions
- `System.DmlException` if there are any problems with a DML statment like an object missing a required field
- `System.ListException` if there are any problems with a list like attempting to access an index that is out of bounds
- `System.QueryException` if there are any problems with SOQL queries like assigning a query that returns no records to a singleton sObject variable
- `System.LimitException` for exceeded governor limits. This type of exception cannot be caught. 
- `System.NullPointerException` any problems with derefencing a null variable
- `System.SObjectException` for any problems with sObject records like referencing a field that was not queried for
- Common Exception Methods: `getCause`, `getLineNumber`, `getMessage`, `getStackTraceString`, `getTypeName`

</details>

<details>
	<summary>DML, SOQL & SOSL</summary>

## DML
- DML operations: insert, update, upsert, delete, undelete, merge
- Database methods allow for partial success: `Database.insert(records, allOrNone)`
- Database methods return results objects in `Database.SaveResult` for inserts and updates, `Database.UndeleteResult` for undeletes, `Database.DeleteResult` for deletes, `Database.UpsertResult` for upsert and `Database.MergeResult` for merges. Each object has `getErrors()` and `isSucess()` methods. 

## SOQL
- Return Types: Integer, List of sObjects, 1 sObject
- Standard Object and Fields: `SELECT Id, FirstName, LastName FROM Contact WHERE FirstName='Abbie' AND LastName='Daniel' ORDER BY FirstName ASC LIMIT 10`
- Standard Parent-to-Child: `SELECT Id, Name, ( SELECT Id FROM Contacts ) FROM Account`
- **Custom Parent-to-Child:** `SELECT Id, Name, ( SELECT Id FROM Course_Deliveries__r ) FROM Course__c`
- Standard Child-to-Parent: `SELECT Id, AccountId, Account.Name FROM Contact`
- **Custom Child-to-Parent:** `SELECT Id, Course__C, Course__r.Name FROM Course_Delivery__c`
- Geolocation Field: `SELECT Id, Office_Location__Lattitude__s, Office_Location__Longitude__s FROM Account `
- Wildcards: `%` matches 0 or more characters & `_` matches 1 or more characters
- Count & Group By: `SELECT StatusPickList__c, COUNT(Name) FROM Case GROUP BY StatusPicklist__c`


## SOSL
- Return Type: list of a list of sObjects  `List<List<sObject>>`
- SOSL Example: `FIND 'Senior Engineer' IN ALL FIELDS RETURNING Contact(Id, Name, Role__ c), Account(Id, Name ORDER BY Name DESC NULLS last) WITH METADATA='Labels' LIMIT 10`
- Format Example: `FIND {Acme} RETURNING Account(Id, LastModifiedDate, FORMAT(LastModifiedDate) FormattedDate)`
- Offset Example: `FIND {test} RETURNING Account(Name, Id ORDER BY Name LIMIT 100 OFFSET 100)` returns rows 101-200
- To Label Example: `FIND {Joe} RETURNING Lead(company, toLabel(Recordtype.Name))` returned records are translated into the user's language
- Search.query method uses a String and brackets for search term: `Search.query('Find {Acme} RETURNING Accounts')`

</details>

<details>
	<summary>Apex Integration</summary>

## Platform Events
- Deliver secure, scalable and customizable event notification within Salesforce or from external sources with platform events. 
- A **Publisher** publishes messages to an event bus/channel and sends them without knowledge of the subscriber that will receive it. Publish event with the `EventBus.publish()` method.
	- **Publish & Subscribe**: Apex Triggers (after insert only for subscribe), Flows, Process Builder
	- **Publisher Only**: Apex, APIs 
- A **Subscriber** expresses subsc ribes to one ore more event bus/channels and only receives messages that of interest, without knowledge of the publisher that produced them.
	- **Publish & Subscribe**: Apex Triggers (after insert only for subscribe), Flows, Process Builder
	- **Subscriber Only**: LWC

## Apex Integration
- **Apex Callouts:** makes a call to an external web service or sends HTTP Request from Apex code, and then receives the response.
- **SOAP Callouts:** Web service callouts to SOAP web services use XML, and typically require a WSDL (web services description language) document for code generation. Usually used for enterprise applications or integrations.
- **REST Callouts:** HTTP callouts to services typically use REST with JSON. Can also be used with SOAP web service and XML. 
- **Apex Web Services:** Expose apex class methods as a REST or SOAP web service operation. For REST, use the `@RestResource` and `@HttpGet` annotation to the `global` class and method. For SOAP, use the `webservice` keyword on the method definition.
- **Best Practice:** Place the callout code in an asynchronous method that’s annotated with @future(callout=true) or use Queueable Apex. This way, the callout runs on a separate thread, and the code after the callout isn't blocked.

## HTTP Callout
- **HTTP Methods:** GET to retrieve data from a URL, POST to create a resource or post data to the server, DELETE to delete a resource identified by a URL, PUT to create or replace resource sent in a request body
  
```apex
public static HttpResponse makeGetCallout(){
	Http http = new Http();
	HttpRequest request = new HttpRequest();
	request.setEndpoint('https://endpoint.com/data');
	request.setMethod('GET');
	HttpResponse response = http.send(request);

	if (response.getStatusCode() == 200){
		// deserialize json string into collection of primitive data types
	}
	return response;
} 
```

  
## Salesforce APIs

- **Bulk API:** Specialized for bulk data load or query. Used for DataLoader
- **Metadata API:** Specialized for migrating changes from a sandbox or testing org to production. Used for Ant Migration Tool via command line.
- **Tooling API:** Specialized for building custom development tools or apps for Platform applications.

![image](https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/656217d8-c27f-4759-90bd-efe3197c1c36)

  
</details>

<br>

</details>
 
<details>
	<summary>Testing, Debugging & Deployments</summary>
 
# Testing, Debugging & Depoyments

## Test Class & Methods

## Mock Testing
- **StaticResourceCalloutMock** for GET Callout
```apex
StaticResourceCalloutMock mock = new StaticResourceCalloutMock();
mock.setStaticResrouce('some_static_resource_json_file');
Test.setMock(HttpCalloutMock.class, mock);
HttpResponse result = CalloutClass.makeGetCallout();
```
- **HTTPCalloutMock** for POST callout
```apex
//CalloutClassMock implements HttpCalloutMock class and has a respond(HttpRequest) method that creates a fake response
Test.setMock(HttpCalloutMock.class, new CalloutClassMock());
```

## Log Inspector
## Debug Logs
## Sandboxes
## Code Coverage
 - why its required
## Deployment Tools
- VSCode & Salesforce CLI
- **ANT Migration Tool:** Java/Ant-based command-line utility for moving metadata between a local directory and a Salesforce org. Can be used for deployment in a scripting environment and is best to use in repetetive deployments using the same parameters.
## Change Sets

<br>


</details>


<details>
	<summary>User Interface</summary>

 # User Interface

<details>
	<summary>Visualforce</summary>

## Visualforce
## Visualforce Page
## Standard Controller
## Standard List Controller
## Custom Controller

<br>


</details>

<details>
	<summary>Lightning Web Component</summary>

## LWC Framework
## LWC Benefits
## LWC Decorators
## Lightning Web Components
## Child to Parent and Parent to Child LWC Communication
## Lightning Message Service
## Lightning Data Service
## LWC Security
</details>

<details>
	<summary>Lightning Aura Component</summary>
	
## Lightning Aura Components
## Aura Component Framework

<br>


</details>


</details>
