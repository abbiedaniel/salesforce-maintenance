# Platform Developer 1 Notes

[Udemy Platform Developer 1 Course](https://bah.udemy.com/course/salesforce-developer/learn/lecture/34602170#overview)

<be>

## Monday - September 18, 2023

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

 - **Save Order of Execution**
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

## Tuesday - September 19, 2023

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


- **Best Practices**
  - Only use triggers if no declarative options work
  - Use only one trigger per object.
  	- You can then use context-specific handler methods within triggers to create logic-less triggers
  - Control triggers with declarative functionality.
  	- Allow admins to access custom metadata or custom setting that  can turn triggers on/off.
</details>
	
<details>
	<summary>Apex Classes</summary>
     
- **Class Definition Syntax**
  ```apex
  // Access Modifiers:
  private | public | global

  // Interface:
  [ virtual | abstract ]

  // Sharing Context: with sharing specifies the sharing rules for the current user to be taken into account for a class
  [ with sharing | without sharing]

  // Implements an interface
  // Extends this class with the functionality of another class
  class ClassName [implements InterfaceNameList] [extends ClassName2] { ... }
  ```
  
- **Class Capabilities**
   - Can be used to create 
  	- Trigger Handlers
   	- Controllers for LWC and Visualforce
   	- Invokable methods for flows and process builder to call
   	- Web services methods for external services to call
 
- **Method Definition Syntax**
  ```apex
  [public | private | protected | global] [override] [static] [ data_type | void ] method_name(input parameters) {
	// The body of the method
  	return;
  }
  ```
- **Access Modifiers and Key Words**
	- global: Can be accessed by any code in your salesforce org. If a method or variable is declared as global, the class must also be global.
   	- private: Can only be accessible in the class it was created in 
 	- protected: Accessible to any inner classes in the defining Apex class, and to the classes that extend the defining Apex class
  	- public: Can be accessed by code in the same namespace  
	- static: Before an object of a class is created, all static member variables in a class are initialized, and all static initialization code blocks are executed. These items are handled in the order in which they appear in the class.
  	- this: use with instance/non-static variables 


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
  - allorNone boolean: false allows partial success if an error is thrown.
  ```apex
  Database.insert(recordToInsert, allOrNone, accessLevel);
  ```

- **Methods of Invoking Apex**
  - Database Trigger, Anonymous Apex, Asynchronous Apex, Web Services, Email Services, Visualforce controllers and Lightning components   

- **Anonymous Apex**
	- Use the “Execute Anonymous” functionality of the Developer Console
 	- Utilise the REST API “executeAnonymous” endpoint
  	- Use the Salesforce CLI “force:apex:execute” command 
 

</details>
    
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
	- inheritance and polymorphism   
    
</details>


<details>
	<summary>Asynchronous Apex</summary>
    
- **Reasons to Program Asynchronously**
  - Processing a very large number of records. Limits are larger for asynchronous than synchronous processes
  - Making Callouts to external web services
  - Create better faster user experience

- **Future Methods**
  - Syntax
  	- must include ```@future static void```  
  ```apex
   @future
   static void myFutureMethod (Set<Id> ids){
  	// query for records using Salesforce Ids
  	// loop through records and perform logic
   } 
   ```
  - Limitations:
  	- Parameters must be primitive data types. You cannot pass objects as parameters to future methods
   	- No execution tracking and no jobId
    	- You cannot chain future methods and have one call another.

- **Batch Apex Class**
  - Syntax
    ```apex
    global class BatchableClass implements Database.Batchable<sObjects> {

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
</details>	

<details>
	<summary>Execution Log</summary>


- **Execution Log**
  - EXECUTION_STARTED - first line in the execution log marks the execution started event
  - EXECUTION_FINISHED - last line is the execution finished event. Everything in between is the execution context
  - CODE_UNIT_STARTED - event marks when the code from the Execute Anonymous window was kicked off
    
- **Log Inspector**
	- Logging Levels: None, Error, Warn, Info, Debug, Fine, Finer, Finest
	- Open dev console and do actions in UI. Logs will be captured in the dev console automatically.
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
 
<br>
    
## Wednesday, September 19th

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
	- Syntax: return type list of list of sObjecs
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
   			//  backslash character (\) escapes characters in column names and string values in a predicate expression.

   			}
   	List<sObjects> results = Database.query(query);
   	return results;
   }
   ```
	- Use Cases
		- Don't know the exact field or conditions
		- Querying dynamic objects
    
- **Data Governor Limits**
	- Per-Transaction Apex Limits
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

</details>
  
<details>
	<summary>Platform Events</summary>

- **Custom Platform Events**
  - Setup platform events in setup like custom objects
  - ```__e``` suffix for API name
  - Inserting platform event records (from a Flow, Apex, Process Builder) fires the event
  - Any automation listening to the event will run upon platform event insertions
  - Custom fields can be added to platform events
  - Listen and Fire Platform Events:
  	- Apex Triggers (can fire and subscribe)
    - Flows (can fire and subscribe)
    - Process Builder (can fire and subscribe)
    - Lightning Web Components (subscribe)
    - Apex (fire)
    - APIs (fire)


- **Subscribe & Publish Platform Events**
  - Publish Behavior:
  	- Publish After Commit: don't want event to fire if Apex fails
   	- Publish Immediately: the event will fire immediately even if Apex fails

</details>

<br>

## Thursday, September 20th

### Dev Orgs
- **Scratch Orgs**
	- To utilize scratch orgs in your development process
 		- Enable Dev Hub to allow scratch orgs to be created
   		- Have a user with permissions to create scratch orgs
     		- Have the Salesforce CLI setup to log into the dev hub and request scratch org creation

### Testing and Debugging
- **Test Classes**
	- Streamline Setup
 		-  Create a class specifically to create data for test methods aka Test data factory class
   		- Add a ```@TestSetup``` annotated method to the class. This method is called before any tests are run and allows the test records to be created before the tests themselves are run.

- **Common Errors**
	- ```List has no rows for assignment to sObject``` - running a query which returns no rows
 	- ```Index 0 is out of bounds``` - attempting to access value at index 0 when there is no data
  	- runtime exceptions 

### Apex Security & Sharing
- **Important Methods**
	- ``Security.stripInaccessible(AccessType, sourceRecords)``` enforces the FLS of the current user by stripping anything which is not accessible in the defined context.

### Extending Declarative Functionality
- **Topic:**
  - info
    - more info

### Visualforce Pages
- **Topic:**
  - info
    - more info
    
### Visualforce Controllers
- **Topic:**
  - info
    - more info
    
### Lightning Web Components
- **Topic:**
  - info
    - more info
    
### Lightning Aura Components
- **Topic:**
  - info
    - more info
