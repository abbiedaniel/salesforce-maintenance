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
1. System Validation
2. Before Save Flows
3. Before Triggers
4. Validation Rules and System Validation
5. Duplicate Rules
6. _Save to database but not committed_
7. After Trigger
8. Assignment Rules
9. Auto response Rules
10. Workflow Rules
11. Escalation Rules
12. Flow Automation
13. After Save Flows
14. Commit to database

**S**am's **F**amily **T**ook **V**alerie **D**own **S**outh **T**o **A** **A**uto **W**orkshop's **E**nclosed **F**oyer.

## Governor Limits
- **SOQL Queries**: 100
- **DML**: 150

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
- **Before triggers** are used to update or validate record values before they’re saved to the database.
- **After triggers** are used to acceess field values, such as Ids, that are set by the system and to effect changes in other records or other objects. (read-only)
- **Trigger Event Context:** before insert/update/delete and after insert/update/delete/undelete
- **Trigger Definition:** `trigger AccountTrigger on Account(before update){}`
- **Trigger Error Handling:** `addError('Error!')` prevents the dml operation from occurring on the field or record
- **Trigger Context Variables:**:
	- update: `Trigger.old`, `Trigger.oldMap`,  `Trigger.new` & `Trigger.newMap` 
	- delete: `Trigger.old` & `Trigger.oldMap` 
 	- undelete: `Trigger.new` & `Trigger.newMap` (before undelete is not a thing)
  	- insert: `Trigger.new` & **after** insert: `Trigger.newMap`
 
## Asynchronous Apex
- **Future methods:** separate transactions, web service callouts. Must have `@future` annotation.
- **Batch Apex:** large data processing, data cleansing or archiving. Must have `start`, `execute` and `finish` methods. Use `Database.executeBatch(ExampleClass, batchSize)` to run the batch class.
- **Queueable Apex:** sequential processing, external web service callouts. Must have `execute` method. Use `System.enqueJob(ExampleQueueableClass)` to run the class.
- **Scheduled Apex:** scheduled processing, weekly or monthly. must have `execute` method. It can call schedule other async apex classes. Use `System.schedule('Job Title', scheduledDateTime, ExampleScheduledApexClass)` to schedule the class or schedule the class in setup.  

## DML
- DML operations: insert, update, upsert, delete, undelete, merge
- Database methods allow for partial success: `Database.insert(records, allOrNone)`
- Database methods return results objects in `Database.SaveResult` for inserts and updates, `Database.UndeleteResult` for undeletes, `Database.DeleteResult` for deletes, `Database.UpsertResult` for upsert and `Database.MergeResult` for merges. Each object has `getErrors()` and `isSucess()` methods. 

## SOQL
- Standard Object and Fields: `SELECT Id, FirstName, LastName FROM Contact WHERE FirstName='Abbie' AND LastName='Daniel' ORDER BY FirstName ASC LIMIT 10`
- Standard Parent-to-Child: `SELECT Id, Name, ( SELECT Id FROM Contacts ) FROM Account`
- Custom Parent-to-Child: `SELECT Id, Name, ( SELECT Id FROM Course_Deliveries__r ) FROM Course__c`
- Standard Child-to-Parent: `SELECT Id, AccountId, Account.Name FROM Contact`
- Custom Child-to-Parent: `SELECT Id, Course__C, Course__r.Name FROM Course_Delivery__c`
- Geolocation Field: `SELECT Id, Office_Location__Lattitude__s, Office_Location__Longitude__s FROM Account `
- Wildcards: `%` matches 0 or more characters & `_` matches 1 or more characters
- Count & Group By: `SELECT StatusPickList__c, COUNT(Name) FROM Case GROUP BY StatusPicklist__c`

# LEFT OFF HERE

## SOSL
- complicated SOSL example

## Custom metadata, Custom platform events, Custom settings

## Apex Integration

## Exception Handling/Exception Class & Method

<br>

</details>
 
<details>
	<summary>Testing, Debugging & Deployments</summary>
 
# Testing, Debugging & Depoyments

## Test Class & Methods
## Exception Examples
- `System.DmlException`
- `System.ListException`
- `System.QueryException`
- `System.LimitException

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
