# Platform Developer 1 Notes

[Udemy Platform Developer 1 Course](https://bah.udemy.com/course/salesforce-developer/learn/lecture/34602170#overview)
<br>
[Practice Test](https://www.salesforceben.com/salesforce-platform-developer-1-practice-exams/)


## Developer Fundamentals - 23%

<details>
	<summary>Salesforce Development Fundamentals</summary>

- **Multi-tenant Environment Considerations:**
  - Unique URL for each environment
  - Governor limits on every user and every Salesforce org
      - Data retrieval, creation and manipulation
      - API limits
      - https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_gov_limits.htm

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
    - Match by Salesforce ID

</details>

<details>
	<summary>EXPAND: Declarative Process & Automation</summary>

- **Flows**
  - Screen
  - Schedule-Triggered
    - once, daily, weekly
  - Record-Triggered
    -  Fast Field Updates: before the record is saved
    -  Actions & Related Records: after the record is saved
  - Platform Event-Trigger
  - Autolaunched
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
  - Fields Visibility & Order for record type
  - Related Lists
  - Quick Actions & Buttons
 
- **Flexipage** 
  - Tab Order
  - Component Visibility
  - Components: Screen-flow, Chatter, Highlight Panel, List View, Path, Record Detail, Related Lists, Custom LWC, etc.

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
	- Inner classes can have their own sharing modes declared, which don’t have to match that of the outer classes. This can be useful for nesting specific methods which require without sharing inside a class which has with sharing declared. 
    
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
  	- for loops for arrays or sets
  	- for loops for soql query
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
  	[public | private | protected | global] [override] [static] [ data_type | void ] method_name(input parameters) {
  		// method body
  		return;
  	}
  }
  ```
- **Class Keywords**
	- ```implements``` an interface
 	- ```extends``` this class with the functionality of another class
  	  
- **Interface Keywords**
	- ```virtual```
 	- ```abstract```   

- **Sharing Keywords**
	- ```with sharing``` enforce sharing rules of the current user.
 	-  ```without sharing``` sharing rules for the current user are not enforced
   	- ```inherited sharing``` Inherited sharing takes on the sharing declaration of the class which has executed the code, so if a class with sharing enforced calls a method in a class with inherited sharing, the inherited sharing class code would run with sharing enforced. This is really useful for when the sharing model to be used isn’t known at design time, or the code is built to be called from varying places within the system.

- **Access Modifiers**
	- ```global``` Can be accessed by any code in your salesforce org. If a method or variable is declared as global, the class must also be global.
   	- ```private``` Can only be accessible in the class it was created in
   	- ```public``` Can be accessed by code in the same namespace
 	- ```protected``` Accessible to any inner classes in the defining Apex class, and to the classes that extend the defining Apex class
 
- **Key Words** 
	- ```static``` Before an object of a class is created, all static member variables in a class are initialized, and all static initialization code blocks are executed. These items are handled in the order in which they appear in the class.
  	- ```this.``` use with instance/non-static variables 

-  **Class Capabilities**
   - Can be used to create 
  	- Trigger Handlers
   	- Controllers for LWC and Visualforce
   	- Invokable methods for flows and process builder to call
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
  	- no update needed since record has not been committed to database
   
  - ```after insert```, ```after update```, ```after delete```, ```after undelete```
  	- need updated since record has already been committed to database

- **Context Variables**
  - ```Trigger.New``` returns a list of the new versions of the sObject records
  	- available in ```insert```, ```update```, ```undelete```
   	- records can only be modified in ```before``` triggers
      
  - ```Trigger.newMap``` returns new map of IDs to the new versions of the sObject records
	- available in ```before insert```,```after insert```,```after update```,```after undelete```
    
  - ```Trigger.Old``` returns a list of the old versions of the sObject records
  	- available in ```update``` and ```delete``` triggers
     
   - ```Trigger.oldMap``` returns map of IDs to the old versions of the sObject records
   		- available in ```update``` and ```delete``` triggers 


- **TO DO: Trigger Exceptions** : https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_exceptions.htm
  
- **Best Practices**
  - Only use triggers if no declarative options work
  - Use only one trigger per object.
  	- You can then use context-specific handler methods within triggers to create logic-less triggers
  - Control triggers with declarative functionality.
  	- Allow admins to access custom metadata or custom setting that  can turn triggers on/off.
</details>

<details>
	<summary>Invokable Apex</summary>
	
- **Methods of Invoking Apex**
  - Database Trigger, Anonymous Apex, Asynchronous Apex, Web Services, Email Services, Visualforce controllers and Lightning components   

- **Anonymous Apex**
	- Use the “Execute Anonymous” functionality of the Developer Console
 	- Utilise the REST API “executeAnonymous” endpoint
  	- Use the Salesforce CLI “force:apex:execute” command 
 
</details>
    

<details>
	<summary>Asynchronous Apex</summary>
    
- **Reasons to Program Asynchronously**
  - Processing a very large number of records. Limits are larger for asynchronous than synchronous processes
  - Making Callouts to external web services
  - Create better faster user experience
  - Future methods, Batch Apex, Queueable Apex, Scheduled Apex

- **Future Methods**
  - Syntax
  	- must include ```@future static void```  
  ```apex
   @future (callout=true) // to use APIs
   static void myFutureMethod (Set<Id> ids){
  	// query for records using Salesforce Ids
  	// loop through records and perform logic
   } 
   ```
  - Limitations:
  	- Parameters must be primitive data types. You cannot pass sObjects as parameters to future methods
   	- No execution tracking and no jobId
    	- You cannot chain future methods and have one call another.
     	- Max invocations for 24 hrs: 250k
     - Benefits: if you want to separate transactions in apex due to cpu usage or governor limits

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
  	- Use this if you need to process a large number of records
     	- Processes 200 records at a time
      	- ```Database.Stateful``` instance variables of this class are preserved after each execute method call
      	  
  - Limitations:
  	- Troubleshooting can be hard
   	- Jobs are queued and subject to server availability

- **Queueable Apex**
	- Syntax
  		 ```apex
   		public class QueueableClass implements Queueable {
     			public void execute (QueueableContext context){
     				// loop through records
     				// call another method for the callout
     			}
     		}
  		 ```
 	- Benefits:
  		- Accepts non-primitive types as parameters
 		- Monitoring - Job Id is returned to identify the job and monitor the progress
  		- Chaining Jobs - You can chain one job to another job by starting a second job from a running job. This can be useful for sequential processing.

- **Scheduled Apex**
  	- Syntax:
	```apex
 	global class ScheduledJob implements Schedulable {
 		global void execute(Schedulable Context SC){}
 	}

 	// to execute class: instantiate the schedulable class
 	system.schedule('

 	
 	```
  	- Max: 100 scheduled apex jobs at a time
  	- Use Apex Scheduler: search apex classes in setup and click Schedule Apex
  		- Weekly or monthly basis 
     	- Use the ```System.schedule('Job Title' , scheduleDate, new ScheduledJob() );``` method within apex
</details>	
 
<details>
	<summary>SOQL, SOSL & DML</summary>

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
	<summary>TO DO: Apex Security & Sharing</summary>
	

- **Important Methods**
  
	- ```Security.stripInaccessible(AccessType, sourceRecords)``` enforces the FLS of the current user by stripping anything which is not accessible in the defined context.

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
  - Inserting platform event records (from a Flow, Apex, Process Builder) fires the event
  - Any automation listening to the event will run upon platform event insertions
  - Custom fields can be added to platform events
  - Turn on debug logs for an automated process entity to debug platform events & triggers/flows
    

- **Subscribe & Publish Platform Events**
   - Subscribe and Fire Platform Events:
  		- Apex Triggers (can fire and subscribe)
   			- subscribe: create an after insert trigger on the platform event object and use ```for (Platform_Event_Name__e event : Trigger.new)``` to create logic to run for each event
     			- publish: in a trigger handler class, instantiate the platform event and use ```EventBus.publish(eventName);``` 
  		- Flows (can fire and subscribe)
   			- subscribe: create an auto-launch flow and create records based on the platform event object
 		- Process Builder (can fire and subscribe) 
   - Fire Platform Events Only:
  		- Apex (fire)
  		- APIs (fire) 
  - Subscribe to Platform Events Only:
  	- Lightning Web Components (subscribe)

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
 	- Setup: preparing data and the runtime environment for your testing scenario
  	- Execution: executing the code you wish to test
  	- Validation: verifying the results of the executed test against the expected results  

- **Best Practices**
  
	- Create a class specifically to create data for test methods aka Test data factory class
 	- Add a ```@TestSetup``` annotated method to the class. This method is called before any tests are run and allows the test records to be created before the tests themselves are run.
  	- Use ```@TestVisible``` for private methods that need to be visible for a test
 
- **Methods**
	- ```Test.startTest()``` use method before executing the code we wish, to test to assign that block of code a new set of governor limits.
 	- ```Test.stopTest()``` use once we’ve finished our execution and are ready to validate our results
  	- Asynchronous Apex: If we are testing asynchronous apex (e.g. a batch class), since the code gets flagged to run at an unknown future date, we would not be able to write tests for any asynchronous methods. Instead by wrapping the code execution in Test.startTest() and Test.stopTest(), when the stopTest method is called, the async code is executed and so we can test the results of the execution within our test class. 
  
</details>

<details>
	<summary>Exception Handling</summary>
	
- **Exception Handling**
	- Try/catch block
  ```apex
  try {
  	// something you think could fail or error
  } catch ( Exception ex ){
  	throw ex;
  
  	// to call custom exception method:
  	TriggerHandlerClass.throwException(ex.getMessage());
  } 
  ```
	- Custom Exception Class
   
   ```apex
   public class AccountTriggerException extends Exception {}
   ```
    
  - Custom Exception Method 
    
  ```apex
    public static void throwException(String message){
  	System.debug(message);
    	throw new AccountTriggerException(message);
  }
  ```
  
  - ```allorNone``` boolean: ```false``` allows partial success if an error is thrown. Instead of an exception being thrown when any record encounters an error during save, a ```List<Database.SaveResult>``` is returned instead of an exception being thrown.
  ```apex
  Database.insert(recordToInsert, allOrNone, accessLevel);
  // When we wish to configure the DML operation, or handle failed records, we must use the Database class methods.
  ```

</details>

<details>
	<summary>TO DO: Code Coverage</summary>
 - https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_code_coverage_best_pract.htm
- https://help.salesforce.com/s/articleView?id=000385650&type=1
- When doing a deployment into production, there must be an average of 75% code coverage for all Apex code within the org. Alongside this, Apex triggers being deployed must have at least 1 line being covered (i.e. they must have been called by at least one test class). When running deployments, there is the option to run a subset of tests which changes the code coverage behaviour. When running the default testing mode, all tests are executed and the total coverage in an org must meet 75%. However, when running a specified set of tests, every item in the deployment must average 75% instead.

</details>


<details>
	<summary>TO DO: Deployments</summary>
 </details>
 
<details>
	<summary>EXPAND: Execution Log</summary>

- **Execution Log**
  - EXECUTION_STARTED - first line in the execution log marks the execution started event
  - EXECUTION_FINISHED - last line is the execution finished event. Everything in between is the execution context
  - CODE_UNIT_STARTED - event marks when the code from the Execute Anonymous window was kicked off
    
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
</details>



<details>
	<summary>EXPAND: Common Errors</summary>

- ```List has no rows for assignment to sObject``` - running a query which returns no rows
- ```Index 0 is out of bounds``` - attempting to access value at index 0 when there is no data

</details> 

<details>
	<summary>Scratch Orgs</summary>
	
- **Scratch Orgs**
  
 	- Enable Dev Hub to allow scratch orgs to be created
   	- Have a user with permissions to create scratch orgs
   	- Have the Salesforce CLI setup to log into the dev hub and request scratch org creation
</details>

<br>

## TO DO: User Interface - 25%

<details>
	<summary>Extending Declarative Functionality</summary>   

### Extending Declarative Functionality
</details>

<details>
	<summary>Visualforce Pages</summary>   
	
### Visualforce Pages
- **Topic:**
  - info
    - more info

- to add a related record's field name to a Visualforce page 
	- Reference the object's fields using {!opportunity.Account.fieldName} in a standard controller
 - to generate a simple PDF
 	- create a visualforce page with ```renderAs="pdf"``` 
    
</details> 

<details>
	<summary>Visualforce Controllers</summary>   
	
### Visualforce Controllers
- **Standard Controllers**
  - Characteristics
    - Controllers can either be tied to a single record or a collection of records
    	- ```StandardController``` and ```StandardSetController``` 
    - Standard controllers exist for all custom and most standard objects
    -  Field level and object level security is enforced by built-in actions
    - Controller extensions can be built to define custom logic and actions to be performed within a controller while retaining the functionality of the standard controller.
   

</details>    

<details>
	<summary>Lightning Web Components</summary>   
	
### Lightning Web Components
- **Salesforce Environment for Lightning Components**
  - Lightning Experience
  - Experiences
  - Salesforce Mobile App

- **HTML Specs**
	- picklists: ```<lightning-combobox> </lightning-combobox>```
   
 - Static Resources: Lightning Components require all third-party resources to be uploaded as Static Resources and loaded through the Platform Resource Loader, however Visualforce can reference external URLs.

</details>

<details>
	<summary>Lightning Aura Components</summary>   
	
### Lightning Aura Components
- **Topic:**
  - info
    - more info

- ```@AuraEnabled(cacheable=true)``` improves the runtime performance of Lightning Components on the Aura Enabled apex methods that are frequently used in multiple LWCs
	- https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/controllers_server_apex_auraenabled_annotation.htm
 - ```setStorable()``` https://developer.salesforce.com/docs/atlas.en-us.224.0.lightning.meta/lightning/ref_jsapi_action_setStorable.htm
</details>

