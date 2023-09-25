# Platform Developer 1 Study Guide

:accessibility: [Exam Guide](https://trailhead.salesforce.com/help?article=Salesforce-Certified-Platform-Developer-I-Exam-Guide)

:accessibility:  [Udemy Course](https://bah.udemy.com/course/salesforce-developer/learn/lecture/34602170#overview) 

:accessibility:  [Practice Test](https://www.salesforceben.com/salesforce-platform-developer-1-practice-exams/)

:accessibility:  [Apex Specialist Superbadge](https://trailhead.salesforce.com/content/learn/superbadges/superbadge_apex)

:accessibility:  [LWC Specialist Superbadge](https://trailhead.salesforce.com/content/learn/superbadges/superbadge_lwc_specialist)

<br>

## Developer Fundamentals - 23%

<details>
	<summary>Salesforce Development Fundamentals</summary>

- **Multi-tenant Environment Considerations:**
  - Unique URL for each environment
  - [Governor limits](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm) on every user and every Salesforce org
      - Data retrieval, creation and manipulation
      - API limits

- **Model View Controller Architecture:**
  - Model: where data is saved
      - Custom and standard objects and fields
  - View: how data is visualized
      - UI, page layout, Visualforce pages, apps, tabs, LWC, css, images
  - Controller: how data is manipulated/logic
      - Custom Apex and Javascript, flows, processes, workflow rules, email alerts

</details>

<details>
	<summary>Data Modeling & Management</summary>
	
- **Relationships**
  - Master-detail
    - Creates a special type of parent-child relationship between this object **(the child, or "detail")** and another object (the parent, or "master") where:
      - The relationship field is required on all detail records.
      - The ownership and sharing of a detail record are determined by the master record.
      - When a user deletes the master record, all detail records are deleted.
      - You can create **rollup summary fields on the *master* record** to summarize the detail records.
    - The relationship field allows users to click on a lookup icon **on the child** to select a value from a popup list. The master object is the source of the values in the list.
  - Lookup
    - Creates a relationship that links this object to another object.
    - The relationship field allows users to click on a lookup icon **on the child** to select a value from a popup list. The other object is the source of the values in the list.  
  - Junction
    - Custom **child** object with two master-detail fields. The child object inherits ownership and sharing of the first master detail created on the child object.

- **Security**
  - Object & Field Access
    - Profile
    - Permission Sets & Permission Set Groups 
  -  Record Access
      - OWD (private, public read-only, public read/write, controlled by parent)
      - Sharing Rules & Role Hierarchy
   
- **Data Imports & Exports**
  - Best Practice for Importing: Match Salesforce ID or custom External ID field to a column in the imported file
  - Data Import Wizard
    - Standard Actions: add new records, update existing records, or both
    - Max: 50,000 records
    - File Type: CSV
    - Match Salesforce ID or Name
  - Data Export Service
    - Export now or schedule monthly export
  - Apex Data Loader
    - Standard Actions: insert, update, upsert, delete, export, export all
    - Max: 5,000,000 records
    - File Type: CSV
    - Match by Salesforce ID or External ID
  - Example: Moving analogous-Account, Contact, and Opportunity object records to a new Salesforce CRM and preserving relationships during data migration
  	- Solution:
   		- Add fields flagged as **external IDs** for each of the objects to be imported, populated with its legacy CRM ID
     	- Use the Data Loader tool, and set the relationship fields to match these external IDs

</details>

<details>
	<summary>Declarative Process & Automation</summary>


 

- **Flows**
  - Screen
  - Schedule-Triggered
    - once, daily, weekly
  - Record-Triggered
    -  Fast Field Updates: before the record is saved
    -  Actions & Related Records: after the record is saved
  - Platform Event-Trigger
  - Autolaunched

- **Declarative Caveats**
	- Standard validation rules are unable to operate on parent-child relationships
 	- Roll-up summary fields can only be on the master
  	- Formula fields are calculated at access time and can span multiple objects 
  
- **Best Practices**
	- For complex solutions, check if there is an app on AppExchange. If there are no suitable AppExchange apps, only then should custom
development be considered.
 	- For declarative solutions, do not use workflow rules or process builders.
  	- Junction objects are used to represent many-to-many relationships and can prevent orphan records  
    
</details>

<details>
	<summary>Save Order of Execution</summary>
 <br>

  1. System Validation
  2. Before Save Flows
  3. Before Triggers
  4. System Validation (again) and Custom Validation Rules
  5. Duplicate Rules
  6. *Record is saved to the database but doesn’t commit yet*
  7. After Triggers
  8. Assignment Rules
  9. Auto-response Rules
  10. Workflow Rules
  11. *System validation and Apex triggers will fire again if a workflow rules updates a field.*
  12. Escalation Rules
  13. Flow Automation
  14. After Save Flows
  15. Commit all DML operations to the database
      
  - https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_order_of_execution.htm

</details>

<details>
	<summary>Layouts & Navigation</summary>

- **Page Layout**
  - Fields visibility & order for record type
  - Related Lists
  - Quick Actions & Buttons
 
- **Lightning Pages** 
  - Tab Order
  - Component Visibility
  - Standard Lightning Page Components: Flow, List View, Visualforce, Chatter, Dashboard, Highlight Panel, Highlights Panel, Recent Items, Record Detail, Path,  Related Lists, Custom LWC, etc.
  		 	

</details>

<br>

## Process Automation & Logic - 30%

<details>
	<summary>Object-Oriented Concepts</summary>

- **Salesforce vs. Apex Objects:**
  - Salesforce:
    - Standard and custom objects are built declaratively and used to organize the data we store in the org.
  - Apex: 
    - Apex objects are developed programmatically and used to organize reusable methods and variables.
   
- **Constructor**
	- located near the top of the class
 	- best practice to include a constructor with no arguments
  	- you will likely need to assign ```this.arg = arg;```  


- **Instantiation**
```apex
  data_type variableName = new constructor (parameters);
```

- **Variables**
	- All variables are initialized to null by default
 	- Parallel blocks can use the same variable name
  	- Sub-blocks cannot redeclare a parent's block variable name
  	- Can be declare at any point in a block

- **Subclasses**
	- Inner classes can have their own sharing modes declared, which don’t have to match that of the outer classes. This can be useful for nesting specific methods that require ```without sharing``` inside a class that has ```with sharing``` declared. 
    
</details>

<details>
	<summary>Apex Data Types</summary>

- **Primitive Data Types**
  - String
  - Boolean
  - Integer
  - Double/Decimal
  - Id
  - Date  
  - DateTime
  - Time
 
- **sObjects**
  - Standard and Custom Objects
  - Constructor parameters can include field values
  - Instantiate an object:
    ```apex
    Account acc = new Account(Name = 'Name');
    ```
   
- **Arrays**
  - List
    - Instantiate a list:
      ```apex
      List<String> stringList = new List<String>();
      
      Account[] accountList = new Account[](testAccount1, testAccount2);
      ```
    - Iterate through a list:
      ```apex
      for (datatype element : list) { ... }
      ```
  - Set: unordered collection without duplicates
      - Instantiate a set:
       ```apex
       Set<Integer> intSet = new Set<Integer>(1, 2, 3);
       ````
  - Map: a collection of key and value pairs
      - Instantiate a map:
       ```apex
       Map<String, String> stringMap = new Map<String, String>();
      
       Map<Integer, String> populatedMap = new Map<Integer, String>(1 => 'First, 3 => 'Third');
       ````

- **API Data Type and Salesforce Field Types**
  - ID: lookup relationship and master-detail relationship  
  - string: auto number, email, phone, picklist, multi-select picklist, text, text area, long text area, rich text area     
  - boolean: checkbox 
  - double: currency, formula, number, percent, roll-up summary  
</details>
    
<details>
	<summary>Apex Logic</summary>

- **Control Flow Statements**
  
	- if, else if and else statements
 	- for loops
  	- List or Set for loops
  	- SOQL for loops: utilize more efficient chunking of SObjects behind the scenes, resulting in reduced heap usage and a lower chance of hitting governor limits for large queries. 
  	- while loop
  	- do {...} while loops (will run at least once)
  	- ternary operator
  	  ```apex
  	  variable = condition ? if true action : if false action
  	  ```
  	- switch statements (can only be run on strings, ints, and sObjects)
  	  ```apex
  	  switch on variable {
  	  	when 'variable value' {
				// logic
  	  	}
  	  	when else {
  	  		// logic
  	  	}
  	  }
  	  ```
</details>
	
<details>
	<summary>Apex Classes</summary>
     
- **Class & Method Definition Syntax**
  ```apex
  // Access Modifiers
  private | public | global

  // Interface
  [ virtual | abstract ]

  // Sharing Context
  [ with sharing | without sharing | inherited sharing ]

  // Class Definition
  class ClassName [implements InterfaceNameList] [extends ClassName2] {

  	// Method Definition
  	[public | private | protected | global] [override] [static] [ return_type | void ] method_name(input parameters) {
  		// method body
  		return;
  	}
  }
  ```
- **Class Keywords**
	- ```implements``` an ```interface```, which is a class in which none of the methods have been implemented. The method signatures are there, but the body of each method is empty. To use an interface, another class must implement it by providing a body for all of the methods contained in the interface.
 	- ```extends``` this class with the functionality of another class
  	  
- **Interface Keywords**
	- ```virtual``` makes it inheritable by any other class present in Salesforce that ```extends``` that class. Virtual methods can be defined in virtual or abstract classes
 	- ```abstract``` makes it inheritable by any other class present in Salesforce that ```extends``` that class. Abstract methods can only be defined in abstract classes.
  		

- **Sharing Keywords**
	- ```with sharing``` enforce sharing rules of the current user.
 	-  ```without sharing``` sharing rules for the current user are not enforced
   	- ```inherited sharing``` Inherited sharing takes on the sharing declaration of the class that has executed the code, so if a class with sharing enforced calls a method in a class with inherited sharing, the inherited sharing class code would run with sharing enforced. This is useful for when the sharing model to be used isn’t known at design time, or the code is built to be called from varying places within the system.

- **Access Modifiers**
	- ```global``` Can be accessed by any code in your salesforce org. If a method or variable is declared as global, the class must also be global.
   	- ```private``` Can only be accessible in the class it was created in
   	- ```public``` Can be accessed by code in the same namespace
 	- ```protected``` Accessible to any inner classes in the defining Apex class, and to the classes that extend the defining Apex class
 
- **Key Words** 
	- ```static``` Before an object of a class is created, all static member variables in a class are initialized, and all static initialization code blocks are executed. These items are handled in the order in which they appear in the class.
  	- ```this.``` use with instance/non-static variables 

-  **Class Capabilities**
   - Classes can be used to create: 
  		- Trigger Handlers
   		- Controllers for LWC and Visualforce
   		- Invocable methods for flows and Process Builder to call
   		- Web services methods for external services to call

</details>





<details>
	<summary>Apex Triggers</summary>
    
- **Trigger Syntax**
  ```apex
  trigger TriggerName on sObjectName (trigger_event_context) {
  
      // Trigger.New is a list of records that were just created
      // Trigger.Old provides the old version of sObjects before they were updated in update triggers or a list of deleted sObjects in delete triggers
      // include logic in handler class and methods so the trigger class is logic-less
  
      HandlerClass.handlerMethod(Trigger.New);
  }
  ```
  
- **Trigger Event Context**

  - ```before insert```, ```before update```, ```before delete```
  	- no update is needed since the record has not been committed to the database
   
  - ```after insert```, ```after update```, ```after delete```, ```after undelete```
  	- needs updating since the record has already been committed to the database

- **Context Variables**
  - ```Trigger.New``` returns a list of the new versions of the sObject records
  	- available in ```insert```, ```update```, ```undelete```
   	- records can only be modified in ```before``` triggers
      
  - ```Trigger.newMap``` returns a new map of IDs to the new versions of the sObject records
	- available in ```before insert```,```after insert```,```after update```,```after undelete```
    
  - ```Trigger.Old``` returns a list of the old versions of the sObject records
  	- available in ```update``` and ```delete``` triggers
     
   - ```Trigger.oldMap``` returns a map of IDs to the old versions of the sObject records
   		- available in ```update``` and ```delete``` triggers 

  
- **Best Practices**
  - Only use triggers if no declarative options work
  - Use only one trigger per object.
  	- You can then use context-specific handler methods within triggers to create logic-less triggers
  - Control triggers with declarative functionality.
  	- Allow admins to access custom metadata or custom settings that  can turn triggers on/off.
</details>

<details>
	<summary>Invocable Apex</summary>
	
- **Methods of Invoking Apex**
  - Database Trigger, Anonymous Apex, Asynchronous Apex, Web Services, Email Services, Visualforce controllers and Lightning components   

- **Anonymous Apex**
	- Use the “Execute Anonymous” functionality of the Developer Console
 	- Utilise the REST API ```executeAnonymous``` endpoint
  	- Use the Salesforce CLI ```force:apex:execute``` command 
 
</details>
    

<details>
	<summary>Asynchronous Apex</summary>

- **Asynchronous Apex**
  
	- Future methods (separate transaction)
    - Batch Apex (large data processing)
    - Queueable Apex (sequential processing)
    - Scheduled Apex (scheduled processing)
        
- **Reasons to Program Asynchronously**
  
  - Processing a very large number of records. Limits are larger for asynchronous than synchronous processes
  - Making Callouts to external web services
  - Create a better, faster user experience
  - Queueable (sequential processing) > Future 
  - Uses ```global``` or ```public``` access modifiers

- **Future Methods**
  - Syntax
  	- must include ```@future static void```  
  ```apex
   @future (callout=true) // to use APIs
   static void myFutureMethod (Set<Id> ids){
  	// query for records using Salesforce IDs
  	// loop through records and perform logic
   } 
   ```
  - Limitations:
  	- Parameters must be primitive data types. **You cannot pass sObjects as parameters to future methods**
   	- No execution tracking and no jobId
    	- You cannot chain future methods and have one call another.
     	- Max invocations for 24 hrs: 250k
     - Benefits: if you want to separate transactions in apex due to CPU usage or governor limits

- **Batch Apex Class**
  - Syntax
    ```apex
    global class BatchableClass implements Database.Batchable<sObjects>, Database.Stateful {

    	global Database.QueryLocator start(Database.BatchableContext bc) {
    		// query for records
    	}

    	global void execute(Database.BatchableContext bc, List<sObject> scope){
    		// loop through records and process records
    	}
    	global void finish(Database.BatchableContext bc) {
			// perform actions after data is processed
    	}
    
    }
    ```
	- To execute:
	```apex
  	Database.executeBatch(new BatchableClass(), batchSize);
  	// batchSize maximum == 2,000 records, if value is larger, salesforce automatically makes batch size 2,000
  	// batchSize minimum == 1
   	```
  	- Use this if you need to process a large number of records
      	- ```Database.Stateful``` instance variables of this class are preserved after each execute method call
      	  
  - Limitations:
  	- Troubleshooting can be hard
   	- Jobs are queued and subject to server availability

- **Queueable Apex**
	- Declaration Syntax
  		 ```apex
   		public class QueueableClass implements Queueable {
     			public void execute (QueueableContext context){
     				// Loop through records
     				// Call another method for the callout
     			}
     		}
  		 ```
        - To add class as a job and queue job
        ```apex
        ID jobID = System.enqueueJob(new QueueableClass());
        ```  
 	- Benefits:
  		- Accepts non-primitive types as parameters
 		- Monitoring - Job ID is returned to identify the job and monitor the progress
  		- Chaining Jobs - You can chain one job to another job by starting a second job from a running job. This can be useful for sequential processing.
    		- Max: 50 jobs in the queue with system.enqueueJob  in a single transaction 

- **Scheduled Apex**
  	- Syntax:
	```apex
 	global class ScheduledJob implements Schedulable {
 		global void execute(Schedulable Context SC){}
 	}

 	// to execute class: instantiate the schedulable class
 	String jobID = System.schedule('Job Title' , scheduledDateTime, new ScheduledJob() );
 	```
  	- Max: 100 scheduled apex jobs at a time
  	- To Schedule the job:
  		- Use Apex Scheduler: search apex classes in setup and click Schedule Apex
  			- Weekly or monthly basis 
     		- Use the ```System.schedule``` method within apex
</details>	
 
<details>
	<summary>DML, SOQL & SOSL</summary>

- **DML**
  - Operations
    - ```update``` - use for after triggers
    - ```upsert``` create new and update existing records
    - ```delete```
    - ```undelete``` restores one or more existing sObject records from the recycling bin
    - ```merge``` merges up to three records of the same sObject type into one of the records, deletes the others, and re-parents any related records.
      
  - Best Practices
    - Always use DMLs with lists over single records
    - DML Governor's Limit: 150 per transaction

- **Salesforce Object Query Language (SOQL)**
	- Syntax: returns list of sObjects, single sObject, integer
   
   ```apex
   [
    SELECT one or more fields,
    	(SELECT fields
    	FROM Child Relationship Field Name or Custom Relationship Field Name with appeneded __r
   		)
   FROM an object
   WHERE filter statements and, optionally, results are ordered
   ]
    ```
	- Capabilities in Salesforce
 		- query in the query editor in the developer console
   		- query in apex code
     
     		```apex
       		Account[] parentAccounts = [SELECT Id, Name, Phone FROM Account WHERE Id IN :accountIdsSet];
       
       		// " :value " is needed on the right side of the comparison clause
       		// IN can only be used on a set and not a list
       		```
       
       		- query for related records that have a lookup relationship to the Account object
  			```apex
    		Account[] parentAccounts = [SELECT Id, Name, Phone,
							(SELECT Id, FirstName, LastName FROM Contacts)
							FROM Account WHERE Id IN :accountIdsSet];
    	
    		// Custom relationship fields, use CustomObject__r
    		// Standard relationship fields, use Child Relationship Name instead of Field Name
     		```

- **Salesforce Object Search Language (SOSL)**
	- Syntax: return type list of list of sObjects
   
  	```apex
   	FIND {Search Query Text} // this line is required // apex uses ' ', query editor uses {}
   	[ IN SearchGroup ]
   	[ RETURNING FieldSpec [[ toLabel(fields) ] [ convertCurrency(Amount) ] [ FORMAT() ] ] ]
   	[ WITH DivisionFilter ]
   	[ WITH DATA CATEGORY DataCategorySpec ]
   	[ WITH SNIPPET [ (target_length = n )] ]
   	[ WITH NETWORK NetworkIdSpec ]
   	[ WITH PricebookId ]
   	[ WITH METADATA ]
   	[ LIMIT n ] //default is 2,000 rows that can be returned
   
   	[ UPDATE [TRACKING], [VIEWSTAT] ] 
   	```
  	 - Example
  	   
  	 ```apex
  	 FIND {Booz Allen Hamilton}
  	 IN NAME FIELDS
  	 RETURNING Account(Id, Name, Phone), Opportunity(Id, Name, AccountId LIMIT 5)
  	 ```
    
- **Dynamic SOQL & SOSL**
	- Syntax: construct string with query line
   ```apex
   global static list<sObject> SOQL(List<String> fields, String sobjectType, String filterField, String filterValue){

   	String query = 'SELECT ';

    	// add fields to query
   	for (String field : fields){
   		query = query + field + ', ';
   	}

   	query = query.left(query.length() - 2 ); //removes last comma and space
   	query = query + ' FROM' + sobjectType;

   	if (filterField != null && filterField != '' && filterValue != null && filterValue != ''){
   			query = query + 'WHERE ' + filterField + ' = \'' + filterValue + '\'';
   
   			// WHERE filterField = 'filterValue'
   			// backslash character (\) escapes characters in column names and string values in a predicate expression.

   			}
   	List<sObjects> results = Database.query(query);
   	return results;
   }
   ```
	- Use Cases
		- Don't know the exact field or conditions
		- Querying dynamic objects
</details>

<details>
	<summary>Governor Limits</summary> 
	
- **Data Governor Limits**
	- Per-Transaction Apex Limits
		- Total number of records processed by a trigger at a time: 200
  			- If the number of records being inserted is greater than this (e.g. from the Bulk API or a bulk DML operation), the trigger is invoked in batches of 200 records at a time. 
  		- Total number of records retrieved in SOQL: 50k
 		- Total number of SOQL queries: 100 Synchronous, 200 Asynchronous
   		- Total number of records retrieved by Database.getQueryLocator: 10k
     	- Total number of SOSL queries: 20
      	- Total number of records retrieved in SOSL: 2k
      	- Total number of DML statements: 150
      	- Total records processed by DML statements: 10k
     	- Maximum number of ```@future``` methods: 50
      	- Max Queue Jobs: 50 
     - Solution: Never put SOQL, SOSL, or DML statements in a loop! Bulkify!
 
    </details>

    
<details>
	<summary>Custom Metadata Types</summary>

- **Characteristics**
  
  - Similar to custom objects and custom settings
  - All records maintained in setup under custom metadata
  - ```__mdt``` suffix
  - Governor limits don't apply to queries on custom metadata records
  - Ideal for saving **stagnant/hardcoded values** and then query from apex code
  - Migrated with change sets or developer tools
  - Visibility: all apex code and APIs can use, only apex code in the same namespace, only apex code in the same managed package
  - Includes custom fields, validation rules, and page layout
  - Option to create a new record of the custom metadata type
  - Custom_Metadata_Type_Name__mdt to reference in apex
  - Use ```Custom_Metadata_Type_Name__mdt.getInstance('Record_Name');``` to access custom metadata in Apex

</details>
  
<details>
	<summary>Platform Events</summary>

- **Custom Platform Events**
  - Setup platform events in setup like custom objects
  - ```__e``` suffix for API name
  - Inserting platform event records (from a Flow, Apex, or Process Builder) fires the event
  - Any automation listening to the event will run upon platform event insertions
  - Custom fields can be added to platform events
  - Turn on debug logs for an automated process entity to debug platform events & triggers/flows
    

- **Subscribe & Publish Platform Events**
   - Subscribe and Fire Platform Events:
  		- Apex Triggers (can fire and subscribe)
   			- subscribe: create an ```after insert``` trigger on the platform event object and use ```for (Platform_Event_Name__e event : Trigger.new)``` to create logic to run for each event
     			- publish: in a trigger handler class, instantiate the platform event and use ```EventBus.publish(eventName);``` 
  		- Flows (can fire and subscribe)
   			- subscribe: create an auto-launch flow and create records based on the platform event object
 		- Process Builder (can fire and subscribe) 
   - Fire Platform Events Only:
  		- Apex (fire)
  		- APIs (fire) 
  - Subscribe to Platform Events Only:
  	- Lightning Web Components (subscribe)
   		- use lightning-emp-API to subscribe to any type of platform event published

  - Publish Behavior:
  	- Publish After Commit: don't want event to fire if Apex fails
   	- Publish Immediately: the event will fire immediately even if Apex fails

</details>

<br>

## Testing, Debugging & Deployments - 22%

<details>
	<summary>Test Classes</summary>

- **Purpose**
	- Used to determine whether a piece of code is behaving exactly as it was intended to.
 	- Three Parts to Testing: 
 		- **Setup**: preparing data and the runtime environment for your testing scenario. 
  		- **Execution**: executing the code you wish to test
  		- **Validation**: verifying the results of the executed test against the expected results
    - https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_qs_test.htm  

- **Best Practices**
  
	- Do not access live data in your org in tests. 
 	- Create a class specifically to create data for test methods aka Test data factory class
 		- Add a ```@testSetup``` annotated method to the class. This method is called before any tests are run and allows the test records to be created before the tests themselves are run.
  	- Use ```@IsTest``` for all test classes
  		- Use ```@TestVisible``` for private methods that need to be visible for a test 
  	- Use the ```runAs``` method to test your application in different user contexts.
  	- Use ```System.assert``` methods to prove that code behaves properly.
  	 
- **Methods**
	- ```Test.startTest()``` use method before executing the code we wish, to test to assign that block of code a new set of governor limits.
 	- ```Test.stopTest()``` use once we’ve finished our execution and are ready to validate our results
  	- Asynchronous Apex: If we are testing asynchronous apex (e.g. a batch class), since the code gets flagged to run at an unknown future date, we would not be able to write tests for any asynchronous methods.
  		- Instead by wrapping the code execution in ```Test.startTest()``` and ```Test.stopTest()```, when the stopTest method is called, the async code is executed and so we can test the results of the execution within our test class.
 
- **Test Class Syntax**
  ```apex
  @IsTest (SeeAllData = true)
  global class MyTest {

  	@testSetup
		global static void testSetup(){
  		Account testAccounts = new Account(Name = 'Test Name');
  		// initialize accounts with data using a loop
  		insert testAccounts;
  	}
  

  	@isTest
  	global static void mytestMethod(){
  		// query for the record
  		testAccount = [ SELECT Id, Name From Account, LIMIT 1 ];

  		Test.startTest();
  
  		// actions to test could be a DML statement
  		update testAccount;

  		// for queueable apex testing:
  		System.enqueuJob(new QueueableClass());

  		// for future methods testing:
  		// for 

  
  
  		Test.stopTest();

  		// query for most up-to-date record and values
  		updatedTestAccount = [ SELECT Id, Name From Account, where ID =: testAccount.Id ];
  
  		System.assert( boolean condition );
  		System.assertEquals( variable1, variable2 );
  	}

  	// you can use testMethod type instead of @isTest
  	static testMethod void mytestMethod(){
  	}
  
  }
  
  ```
  
</details>

<details>
	<summary>Exception Handling</summary>
	
- **Try/catch block**
  ```apex
  try {
  	// something you think could fail or error
  } catch ( Exception ex ){
  	throw ex;
  
  	// to call custom exception method:
  	TriggerHandlerClass.throwException(ex.getMessage());
  }
  
  //optional:
  finally{
	// runs after the try block successfully runs or the catch block finishes executing  

  }
  ```
- **Custom Exception Class**
	-  class name must end with ```Exception```
   
   ```apex
   public class AccountTriggerException extends Exception {}
   ```
    
- **Custom Exception Method** 
    
  ```apex
    public static void throwException(String message){
  	System.debug(message);
    	throw new AccountTriggerException(message);
  }
  ```
  
- ```allorNone``` **boolean:**
	- ```false``` allows partial success if an error is thrown. Instead of an exception being thrown when any record encounters an error during save, a ```List<Database.SaveResult>``` is returned instead of an exception being thrown.
  ```apex
  Database.insert(recordToInsert, allOrNone, accessLevel);
  // When we wish to configure the DML operation, or handle failed records,
  // we must use the Database class methods.
  ```

</details>

<details>
	<summary>Trigger Exceptions</summary>
	
- **addError**
  
 	-  ```object_record.addError( 'Text to display to the user!' );``` 
    
  		- Triggers can be used to prevent DML operations from occurring by calling the addError() method on a record or field
  		- When used on Trigger.new records in insert and update triggers, and on Trigger.old records in delete triggers, the custom error message is displayed in the application interface and logged.
    		- When a subset of records are being processed:
      			- If the trigger was spawned by a DML statement in Apex, any one error results in the entire operation rolling back. However, the runtime engine still processes every record in the operation to compile a comprehensive list of errors.
         		- If the trigger was spawned by a bulk DML call in the Lightning Platform API, the runtime engine sets aside the bad records and attempts to do a partial save of the records that did not generate errors. See Bulk DML Exception Handling.
           	- If a trigger ever throws an unhandled exception, all records are marked with an error and no further processing takes place.
           
</details>


<details>
	<summary>Testing Asynchronous Apex</summary>

- Similar as any test class (needs ```Test.startTest``` & ```Test.stopTest```), but use specific methods for
  
	- Queueable Apex: ```System.enqueuJob(new QueueableClass());```
  	- Batchable Apex: ```Database.executeBatch(new BatchableClass(), batchSize);```
  	- Scheduled Apex: ```System.schedule('Job Title' , scheduledDateTime, new ScheduledJob() );```
  	- Future Methods: call the future method between startTest and stopTest
  	  
 </details>

<details>
	<summary>Code Coverage</summary>

- **Requirements**
  
	 - Average of 75% code coverage for all apex code to be deployed to production
  	- Code Coverage = Total number of lines that successfully execute / Total number of lines of code 
	 - Apex triggers being deployed must have at least 1 line being covered (i.e. they must have been called by at least one test class)
  	- Run Specified Set of Tests during deployment: every item in the deployment must average 75% instead.
  	- Run All Tests during deployment: all tests are executed and the total coverage in an org must meet 75%
  	- Your goal should be 100% coverage
- **[Testing Best Practices](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_testing_best_practices.htm)**

</details>


<details>
	<summary>Execution Log</summary>

- **Execution Log**
  - ```EXECUTION_STARTED``` - first line in the execution log marks the execution started event
  - ```EXECUTION_FINISHED``` - last line is the execution finished event. Everything in between is the execution context
  - ```CODE_UNIT_STARTED``` - event marks when the code from the Execute Anonymous window was kicked off
    
- **Log Inspector**
	- Logging Levels: None, Error, Warn, Info, Debug, Fine, Finer, Finest
	- Open the developer console and do actions in UI. Logs will be captured in the dev console automatically.
 	-  You can also run logs on a specific user and get the logs after the UI actions have been completed
    
- **Debug Logs Contains Info About**
	- Database changes
 	- HTTP callouts
 	- Apex errors
 	- Resources used by Apex
 	- Automated workflow processes, such as:
  		- Workflow rules
  		- Assignment rules
  		- Approval processes
  		- Validation rules
      
- **Developer Console Test Tab**
 	- Test Table: displays status, test run class/method, enqueued time, duration, failures, and total
   	- Overall Code Coverage Table: displays clickable class name, code coverage percentage, and # of lines covered/ # of total lines
      
- **Common Errors**

	- ```List has no rows for assignment to sObject``` - running a query that returns no rows
	- ```Index 0 is out of bounds``` - attempting to access value at index 0 when there is no data

</details> 

 <details>
	<summary><b>NEEDS REVIEW: Deployments</b></summary>

- **Org Basics**
- **Change Sets**
	- Target & Source Pairs
 		- Sandbox to Sandbox
   		- Sandbox to Production Org
     		* Org must be tied to a production instance to have change sets feature
      - (https://help.salesforce.com/s/articleView?id=sf.deploy_connection_parent.htm&type=5)   
- **Salesforce CLI**

- **Deprecration**
	- Apex Classes & Metadata

   		- Apex classes and some other metadata cannot be directly deleted from production. While these pieces of metadata can be deleted within a Sandbox, changesets cannot upload these destructive changes.
   			- Instead the Metadata API must be used. This could be with a tool such as ANT to produce a destructive changeset which is deployed into the org.
   	 		- (https://developer.salesforce.com/docs/atlas.enus.api_meta.meta/api_meta/meta_deploy_deleting_files.htm)
       - Fields
       		- Remove all references of this field in the org before deleting

	
- **Scratch Orgs**
  
 	- Enable Dev Hub to allow scratch orgs to be created
   	- Have a user with permissions to create scratch orgs
   	- Have the Salesforce CLI setup to log into the dev hub and request scratch org creation
</details>

<br>

## User Interface - 25%

<details>
	<summary>Extending Declarative Functionality: Flows</summary>   

- **Invocable Methods**
   
   ```apex
    	@InvocableMethod( label = 'methodName' description = 'description' category = 'DML')
    	global static List<List<sObject>> methodName( List<List<sObject>> records){
   		return records;
   	}
   ```
  - Parameter and return type is a list (single record) or list of lists (bulkify) and the method must be static
  - Use Case: Flows cannot upsert records. You can pass the flow records to an invocable apex method to do the upsert action. In the flow, use the action element and search your invocable method label name and category.
  
- **Invocable Variables**

   	```apex
    	@InvocableVariable(required = true)
    ```
    
    - Identifies a class variable used as an input or output parameter for an invocable method's invocable action.
    - Use the action element in a flow and search for the invocable method. The invocable variable names will display at the bottom.
     
- **Apex-Defined Data Type**
	- Apex classes can be used in flows as the data type for a flow variable/resource
 	- Apex class must have a no-parameter constructor
  	- Each invocable variable needs to be ```@AuraEnabled``` to be used as a apex-defined type
  	- Invocable variables cannot be of type ID
  	- Cannot be required

</details>

<details>
	<summary>Visualforce Pages</summary>

- **Visualforce Basics**
  
  	- Created before lightning experience, compatible with classic
  	- Page files end in ```.vfp```
  		- ```apex:page``` header is required
  	 	- Enable override option button in object setup     
	- Component files ends in ```.vfc```
 		- ```apex:component``` header is required
  	
- **Standard Controller**
  ```apex
  <apex:page standardController = "Object" lightningStylesheets = "true">
  // allows access to standard controller methods and fields
  // lightningStylesheets renders page similar to lightning experience display, without it, it looks like salesforce classic
        
  	<apex:form>
  	// allows user to input data to the page
        
  		<apex:pageBlock title = "Page Block Title" mode = "edit">
  		// set page block title and mode
        
  			<apex:pageBlockButtons>
  			// display a page block button
  
  				<apex:commandsButton action = " {!save}" value = "Save" />
  				// create save button and close tag
            
  			</apex:pageBlockButtons>
        
  			<apex:pageBlockSection title = "Section Title" columns = "2">
				// create page block section and title with 2 columns
        
  				<apex:inputField value = "{!objectName.fieldName}" />
  				// reference objects and fields using {! }

        
  			</apex:pageBlockSection>
 			 </apex:pageBlock>
		</apex:form>
	</apex:page>
	``` 

- **Standard List Controller**
  
```apex
<apex:page standardController = "Object Name" recordsSetVar = "object_variable" >
// list controllers must set the recordsSetVar variable
// object_variable represents the list of objects

	<apex:form>
	// apex:commandButton needs to be in an apex:form

 
		<apex:pageBlock>
			<apex:pageBlockTable value = "{!object_variable}" var = "single_element">
			// displays data in a table format
			// value = list, var = element in the list

				<apex:column value = "{!single_element.field}"/>
				// display field as a column in the table

			</apex:pageBlockTable>

			<apex:blockButtons>
			// display previous and next buttons since List Controller only returns the first 20 elements

				<apex:commandButton action = "{!previous} value = "Previous"/>
				<apex:commandButton action = "{!next} value = "Next"/>

			</apex:blockButtons>
		</apex:pageBlock>
	</apex:form>
</apex:page>
```

- **Custom Visualforce Components**
	- ```WelcomeMessage.vfc```
	```apex
	<apex:component>
		<apex:attribute name = "User's Name" type = "String" description = "The name of the user we are welcoming." />
		Welcome {!name} to Salesforce CRM!
	</apex:component>
	```
    
 	- To use in a visualforce controller
    	```apex
     	<c:WelcomeMessage name = "Abbie" />
     	```

- **Debugging Visualforce Tips**
	- Development mode in user setup allows you to directly view and edit visualforce pages
 	- View State: holds state and size of visualforce components and controllers
  		- Max Visualforce View State Size: 170KB
    	- Order of Execution
     		1. Custom controller and controller extension constructors are called
       		2. Custom components are created and associated constructors are executed. Attributes with expressions are evaluated after constructors. 
       		3. Any ```assignTo``` attributes on the page's custom components is executed
         	4. ```apex:form``` is saved to the view state
          	5. HTML is sent to the browser. The browser executes any client-side code.   
   
- **Use Cases**
	- Build wizards and other multi-step processes
 	- Provide low-code solution to non-developers or junior developers
  	- To create custom flow control through an application
  	- Define navigation patterns and data-specific rules for optimal, efficient application interaction
  	- Can add visualforce pages in lightning app builder/flexipage   

- **Important Methods:**
	- To add a related record's field name to a Visualforce page, use formula field syntax 
		- Reference the object's fields using ```{!opportunity.Account.fieldName}``` in a standard controller
 	- To generate a simple PDF
 		- create a visualforce page with ```renderAs="pdf"```
   	- To add pagination to a page
   		- The ```StandardSetController``` is designed to work with sets of records, and provides built-in methods to enable a large set of records to be displayed on a Visualforce page, with methods to assist in pagination of the record list.
    
</details> 


<details>
	<summary>Visualforce Controllers</summary>
	
- **Basic Controller**
  
	- Visualforce Page: ```EditPage.vfp```
   
	```apex
	<apex:page controller = "EditPageController">'
 
 	{ !account.Name }
 
	</apex:page>
	```

	- Apex Class: ```EditPageController.apxc```
   
 	```apex
	public Account[] account;
  
  	// getter and setter
  	// use 'private set' to disable the setter
  	public String searchText {get;set;}
  

  	// expanded getter method
  	public String getSearchText(){
  		return this.searchText;
  	}
  	

  	public EditPageController(){
  		// gets account ID from URL
  		this.account = [SELECT Id, Name, AccountNumber
  				FROM Account
  				WHERE Id = :ApexPages.currentPage().getParameters().get('id')];
  	}
  
	// controller method
  	public PageReference doSearch(){}
  
  	```  
- **Controller Methods**
```apex
<apex:page>
	<apex:form>
	
		<apex:pageBlock id="block">
 		// assigns page block an id
	
			<apex:inputText id = "searchText" value = "{!searchText}"/>
			// assigns input text to searchText 

			<apex:commandButton value = "Search" action = "{!doSearch}" reRender = "block"/>
			// button runs the doSearch method, rerenders the page block titled "block"

		</apex:pageBlock>
	<apex:form>
</apex:page>
```


- **Dynamic Expressions**

```apex
// dynamically decide to show results or not based on how many records are returned using a formula expression
<apex:pageBlockTable> value = {!results} var = "r" rendered = {!NOT (ISNULL (results) )}" />
```

- **Standard Controller Extensions**
  
	- Apex Class: ```EditPageControllerExtension.apxc```

 	```apex
	public class EditPageControllerExtension{

		// need a constructor to take in a standardSetController
		public AccountControllerExtension(ApexPages.StandardSetController stdSetController){}
	}
	```

	- Visualforce Page: ```EditPageController.vfp```
 
   	```apex
    	<apex:page> standardController = "Account" extensions = "AccountControllerExtension" recordSetVar = "acounts"/>
    	```
  
- **Controller Test Coverage**
```apex
// set the current page reference
PageReference pageRef = Page.EditPageController;
Test.setCurrentPage(pageRef);

// start test
Test.startTest();

// instantiates controller
ControllerPageController ctrl = EditPageController();

// runs search action
ctrl.searchText = "Test Account";
ctrl.doSearch();

// save search results
Account[] results = ctrl.results;

// stop test
Test.stopTest();

// verify expected results
System.assertEquals(expected, actual);
```

- **Controller Characteristics**
  
    - Controllers can either be tied to a single record or a collection of records
    	- ```StandardController``` and ```StandardSetController``` 
    - Standard controllers exist for all custom and most standard objects
    - Field level and object level security is enforced by built-in actions
    - Controller extensions can be built to define custom logic and actions to be performed within a controller while retaining the functionality of the standard controller.
   
</details>    













<details>
	<summary><b>IN PROGRESS: Lightning Web Components</b></summary>   
	
 ### Lightning Web Components

- **LWC Characteristics**
	- LWCs use standard HTML and Javascript
 	- Dev tools needed for LWCs are VSCode and Salesforce Extension Pack
  	- LWC can be used in Lightning Experience apps, Experiences, and Salesforce Mobile App
  	- LWC requires all third-party resources, like Javascript and CSS, to be uploaded as Static Resources and loaded through the Platform Resource Loader
  	- LWC require an HTML file, a JavaScript file and a Salesforce-specific JS-META.XML metadata file
  	- LWC must be named in camelCase
  	- Capabilities: Lists View, Related Lists, Lightning Record Form

- **LWC Framework**
	- Benefits:
 		- Lightweight for faster development
 		- Faster performance
  		- Out-of-the-box components
  		- Built upon web standards
 

- **Basic Component**
  
	- JavaScript File: ```home.js```
 
	```javascript
	import {LightningElement} from 'lwc';

	export default class Home extends LightningElement{

 		message = "Hello World"; 
	}
	```
 
	- HTML File: ```home.html```
   
	```html
	<template>
 		// to reference a custom component add c- with all lower case
 		// sets the custom component's message attribute to the message variable in the Home class
 		<c-custom-message> message={message}
 
 		<c-custom-message>
	</template>
	```
 
  	 - Metadata file: ```home.js-meta.xml```
  	   
	```xml
	<LightningComponentBundle xlmns = "salesforce soap metadata url">
		<apiVersion> 55.0 </apiVersion>
		<isExposed> true </isExposed> // allows the component to be visible in the lightning app builder

		<targets>
			<target> lightning__HomePage </tagets> // location on where the file can be used
		</targets>
	</LightningComponentBundle>
	```
 
 	- Optional CSS File: ```home.css`
    
 	```css
  	.test{
  		background-color: aliceblue;
    	}
  	```


- **Component Composition**
  
	- Javascript file: ```customMessage.js```
   
   	```javascript
	import {LightningElement, api} from 'lwc';

	export default class customMessage extends LightningElement{

 		@api  // allows variables to pass in as an attribute
    	message; // stores value from home page input and stores here
	}
	```


	- HTML File: ```customMessage.html```

	```html
	<template>
 		// custom css and styling
 		// slds-box is a class from the lightning design system 
 		<div class = "slds-box" style="background-color: white;">
 
 			// creates a simple white box with the message inside
 			// referencing message variable in customMessage class
 			{message}
 		</div>
	</template>
	```
 
 	 - Metadata file: ```customMessage.js-meta.xml```
 
	```xml
	<LightningComponentBundle xlmns = "salesforce soap metadata url">
		<apiVersion> 55.0 </apiVersion>
		<isExposed> true </isExposed
		// allows the component to be visible in the lightning app builder-->

		<targets>
			<target> lightning__HomePage </tagets>
			// location on where the file can be used
		</targets>
	</LightningComponentBundle>
	```
 
	- Optional CSS File
   
	```css
  	.test{
 		background-color: aliceblue;
    	}
  	```
   
- **Component Composition**
- **Events**
- **Lightning Base Components**
- **Salesforce Data**
- **Lightning Message Service**
- **Use Cases**
	


- **Lightning Data Service**
	- When building components that work on individual records, the Lightning Data Service provides a performant and cached mechanism for loading and updating record data that gets propagated throughout all components utilizing the service.
 	- This offers advantages over performing Apex calls to achieve simple record data since it increases performance and allows changes in other areas of the UI (for example for the standard record details component) to propagate to other components.
   	- (https://developer.salesforce.com/docs/atlas.enus.
lightning.meta/lightning/data_service.htm) 
  
- **HTML Specs**
- The LWC framework follows the HTML specification for how it expects HTML to be written within component templates. This means that for any components that aren’t base HTML tags, it is required that no component tags are self-closing (i.e., there is always an explicit closing tag).
	- picklists: ```<lightning-combobox> </lightning-combobox>```
 	- (https://developer.salesforce.com/docs/componentlibrary/
documentation/en/lwc/lwc.create_components_html_file) 
   
 - **Best Practices**
 	- All event names must not use uppercase letters, have no spaces and use underscores to separate words
    
  - **LWC Security**
  	- Sanitize any user input
   	- Add the ```WITH SECURITY_ENFORCED``` clause to the query to enforce permissions on the query, so if a query attempts to access a field or object the user doesn’t have access to, an exception is thrown.
    - Use bind variables for user input to ensure values are treated as values and not accidentally interpreted as extensions to a query.
    - Hardcode the filterable fields in the Apex controller
    	- A piece of Apex should never trust search parameters from a Lightning Component as these could easily be manipulated. Instead, in scenarios where this is required, alternative approaches should be used such as hardcoding the filter variables in an Apex datatype or as parameters to the method, in order to ensure that any requested fields/filters have been explicitly pre-authorized.	 
    - Utilize the ```with sharing``` keyword on the Apex class

 - **Methods**
 	- ```this.dispatchEvent( my CustomEvent( "my_event", {detail: this.recordId} ))```
  		- Lightning Web Components utilize the standard CustomEvent class within JavaScript, which is then dispatched through the EventTarget.dispatchEvent() method, which in the majority of cases, would be this.dispatchEvent() – since we would want parent components to handle this event. We add information to the event with the detail property of CustomEvent, which the event handlers can access and process accordingly. The detail property can be any datatype.
    	- We should follow the DOM event standard in the naming of our events, meaning no upper-case letters, no spaces, and underscores to separate words.
     	- https://developer.salesforce.com/docs/componentlibrary/documentation/en/lwc/lwc.events_create_dispatch
   


</details>






<details>
	<summary><b>TO DO: Lightning Aura Components</b></summary>  

### Lightning Aura Components

- **LWC vs Aura Components**
  
- **Aura Component Framework**
- **Basic Aura Component**
- **Events**
- **Using LWC with Aura**
- **Aura Component Use Cases**
	


- **Aura Enabled Important Methods & Signatures**
  
	- ```@AuraEnabled(cacheable=true)``` improves the runtime performance of Lightning Components on the Aura Enabled apex methods that are frequently used in multiple LWCs by caching the result on the client side
		- https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/controllers_server_apex_auraenabled_annotation.htm
   
	 - ```Security.stripInaccessible(AccessType, sourceRecords)``` enforces the FLS of the current user by stripping anything which is not accessible in the defined context in @AuraEnabled methods 
 		- https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_classes_with_security_stripInaccessible.htm 

- ``` @AuraEnabled(cacheable=true) public static Object performAction()``` this method signature allows apex methods to  be used by the wire services
 		- The Wire service is designed to provision a component with an immutable stream of data to a component that is ever updating. Because of this, the values returned by the Apex methods must be cached and so we must always annotate a method intended to be used by the Wire service with @AuraEnabled (cacheable = true).
		- (https://developer.salesforce.com/docs/componentlibrary/
documentation/en/lwc/data_wire_service_about) & (https://developer.salesforce.com/docs/componentlibrary/
documentation/en/lwc/lwc.apex_wire_method) 



	 - ```setStorable()``` must be used if we wish for an Apex action to be cached within an Aura component, however there is no such requirement when we are working with LWCs.
 		- https://developer.salesforce.com/docs/atlas.en-us.224.0.lightning.meta/lightning/ref_jsapi_action_setStorable.htm
</details>

