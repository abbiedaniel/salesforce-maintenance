# PD1 Cheat Sheet

<details>
	<summary>Dev Fundamentals</summary>

# Developer Fundamentals


## Standard Relationships & Fields

- **Master-detail**: on the child object, child obj do not have owners so they can't be used with queues
- **Lookup**: on the child object
- **Junction**: child object with two master detail fields, inherits security of first master
- **External Lookup**: external parent object
- **Indirect Lookup**: external child object
- **Roll Up Summary**: on the master, COUNT/SUM/MIN/MAX, works on lookups: Opp-Opp Product, Account-Opp, Campaign-Campaign Members
- **Validation Rules**: don't operate on parent-child relationships
- **Formula Field**: can't be used in a roll up summary field if it references a field on a different object or if NOW() or TODAY() methods are in the formula
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
- SOQL Queries: 100
- DML: 150

## Model View Controller Architecture
- Model: where data is saved
- View: how data is visualized
- Controller: how data is manipulated/logic

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

## Data Imports & Exports

|   | Data Import Wizard      | Data Loader |
| ----------- | ----------- | ----------- |
| **Max Records** | 50K    | 5M       |
| **Pros** | - Choose whether to trigger workflow rules   | - Can save mapping for later use <br> - Can delete and export data    |
| **Cons** |  - Can only insert, update or upsert <br> - Can't use on product and opportunities <br> - Can't save mappings <br> - Can't schedule imports   | - No option to turn off workflow rules |

![image](https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/41026379-2bf2-46e1-a7b5-28dab8514a1b)


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
- Access modifiers: `global`, `public`, `private`, `protected`
- sharing context: `with sharing`, `without sharing`, `inherited sharing`
- Class keywords: `implements`, `extends`
- Interface keywords: `abstract`, `virtual`, `interface`
- Constructors
- Member variables
- Member properties
- Methods

## Apex Data Types
- String: 'hello world'
- Boolean: true or false
- Integer: 7
- Decimal: 7.7
- Id: 006Hs00001KsrsSIAR
- Date: 2024-01-23
- DateTime: 2024-01-23 03:03:03
- Time: 02:39:39.217Z
- Blob: binary data
- Enum: store set of id that are accessed one at a time
- List: `List<String> colorsList = new List<String>{'red'};`
- Set: `Set<Integer> intSet = new Set<Integer>();`
- Map: `Map<Id, String> idList = new Map<Id, String>();`

## Apex Class Use Cases
- Trigger Handler Class: `public class AccountTriggerHandler {}`
- Lightning Web Controller Class: `public class MedsListController{}`
- Visualforce Controller Class: `public class EditPageController{}`
- Exception Class: `public class MyCustomException extends Exception{}`
- Test Data Factory Class:`@isTest public class TestDataFactory{}`
- Test Class: `@isTest private class AccountTriggerHandlerTest{}`
- Invocable Methods for Flows & Process Builders to Call: `@InvocableMethod(callout = true label = 'methodName' description = 'description' category = 'DML')`
- Web Services Methods for External Services to Call: `@future(callout=true) static void myfutureMethod(){}`

## Apex Triggers*
- Trigger Definition: `trigger AccountTrigger on Account(before update){}`
- Trigger Context:
- Trigger Error Handling: 

## Other Apex
- Asynchronous: queueable apex, batchable apex, scheduled apex, future methods
- Anonmyous: execute anonoymous window, salesforce CLI `force:aepx:execute` command, REST API executeAnonymous endpoint
- Invocable: `@InvocableMethod` or `@InvocableVariable` to be used in a flow

## Data Search & Manipulation in Apex
- complicated soql example
- sosl
- parent - child soql
- child -> parent soql
- dml example

## Custom metadata, custom platform events, Custom settings
## Apex Integration



 </details>
 
<details>
	<summary>Testing</summary>

# Testing, Debugging & Depoyments

## Test Class & Methods
## Exception Handling
## Exception Class & Method

## Exception Examples
- `System.DmlException`
- `System.ListException`
- `System.QueryException`

## Asynch Testing
- Queueable Apex: `System.enqueJob()`
- Batchable Apex: `Database.executeBatch()`
- Schedule Apex: `System.schedule()`
- Future Methods: call method between `Test.startTest` and `Test.stopTest`

 </details>
  
<details>
	<summary>Debugging</summary>

## Log Inspector
## Debug Logs

 </details>

 <details>
	<summary>Deployments</summary>

## Sandboxes
## Code Coverage
 - why its required
## Deployment Tools
## Change Sets

</details>
 
 
<details>
	<summary>Visualforce</summary>
	
# User Interface
## Visualforce
## Visualforce Page
## Standard Controller
## Standard List Controller
## Custom Controller
</details>

<details>
	<summary>Lightning Web Components</summary>

## LWC Framework
## LWC Benefits
## LWC Decorators
## Lightning Web Components
## Child to Parent and Parent to Child LWC Communication
## Lightning Message Service
## Lightning Data Service
## LWC Security
## Lightning Aura Components
## Aura Component Framework

</details>
