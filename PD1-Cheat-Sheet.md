# PD1 Cheat Sheet

<details>
	<summary>Dev Fundamentals</summary>

# Developer Fundamentals

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

## Data Imports & Exports

|   | Data Import Wizard      | Data Loader |
| ----------- | ----------- | ----------- |
| **Max Records** | 50K    | 5M       |
| **Pros** | - Choose whether to trigger workflow rules   | - Can save mapping for later use <br> - Can delete and export data    |
| **Cons** |  - Can only insert, update or upsert <br> - Can't use on product and opportunities <br> - Can't save mappings <br> - Can't schedule imports   | - No option to turn off workflow rules |

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
- Trigger Handler Class
- Lightning Web Controller Class
- Visualforce Controller Class
- Exception Class: `public class MyCustomException extends Exception{}`
- Test Data Factory Class: 
- Test Class
- Invocable Methods for Flows & Process Builders to Call
- Web Services Methods for External Services to Call

## Apex Triggers
- Trigger Definition: `trigger AccountTrigger on Account(before update){}`
- 

## Other Apex
- Asynch
- Invocable
- Syncrh
- Anonmyous

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

 </details>
  
<details>
	<summary>Debugging</summary>
 </details>

 <details>
	<summary>Deployments</summary>
 </details>
 
<details>
	<summary>Lightning Web Components</summary>

# User Interface

 </details>
 
<details>
	<summary>Visualforce</summary>
 </details>
