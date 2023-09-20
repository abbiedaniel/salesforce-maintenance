## Monday - September 18, 2023

##### Udemy Platform Developer 1 Course: https://bah.udemy.com/course/salesforce-developer/learn/lecture/34602170#overview

### Salesforce Development Fundamentals
- **Multi-tenant Environment Considerations:**
  - Unique URL for each environment
  - Governer limits on every user and every salesforce org
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

### Data Modeling & Management
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
 
### Declarative Process & Automation
- **Flows**
  - Screen
  - Schedule-Triggered
    - once, daily, weekly
  - Record-Triggered
    -  Fast Field Updates: before record is saved
    -  Actions & Related Records: after record is saved
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
  13. Flow Automations
  14. After Save Flows
  15. Commit all DML operations to database
      - https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_order_of_execution.htm

### Layouts & Navigation
- **Page Layout**
  - Fields Visability & Order for record type
  - Related Lists
  - Quick Actions & Buttons
 
- **Flexipage** 
  - Tab Order
  - Component Visibility
  - Components: Screen-flow, Chatter, Highlight Panel, List View, Path, Record Detail, Related Lists, Custom LWC, etc.

<br>

## Tuesday - September 19, 2023
    
### Apex Triggers
- **Trigger Syntax**
  ```
  trigger TriggerName on sObjectName (trigger_event_context) {
  
      // Trigger.New is list of records that were just created
      // Trigger.Old provides the old version of sObjects before they were updated in update triggers or a list of deleted sObjects in delete triggers
      // include logic in handler class and methods so trigger class is logic-less
  
      HandlerClass.handlerMethod(Trigger.New);
  }
  ```
  
- **Trigger Event Context**
  - before insert, before update, before delete
  	- no update needed since record has not been committed to database    
  - after insert, after update, after delete, after undelete
  	- need updated since record has already been committed to database 

- **Best Practices**
  - Only use triggers if no declarative options work
  - Use only one trigger per object.
  	- You can then use context-specific handler methods within triggers to create logic-less triggers
  - Control triggers with declarative functionality.
  	- Allow admins to access custom metadata or custom setting that  can turn triggers on/off.
     
### Apex Classes
- **Class Definition Syntax**
  ```
  // Access Modifiers:
  private | public | global

  // Interface:
  [ virtual | abstract ]

  // Sharing Rules:
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
  ```
  [public | private | protected | global] [override] [static] [ data_type | void ] method_name(input parameters) {
	// The body of the method
  	return;
  }
  ```
- **Access Modifiers and Key Words**
	- global: Can be accessed by any code in your salesforce org. If a method or variable is declared as global, the class must also be global.
   	- private: Can only accessible in the class it was created in 
 	- protected: Accessible to any inner classes in the defining Apex class, and to the classes that extend the defining Apex class
  	- public: Can be accessed by code in the same namespace  
	- static: Before an object of a class is created, all static member variables in a class are initialized, and all static initialization code blocks are executed. These items are handled in the order in which they appear in the class.
  	- this: use with instance/non-static variables 

### Apex Data Types
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
    ```
    Account acc = new Account(Name = 'Name');
    ```
   
- **Arrays**
  - List
    - Instantiate a list:
      ```
      List<String> stringList = new List<String>();
      
      Account[] accountList = new Account[](testAccount1, testAccount2);
      ````
    - Iterate through a list:
      ```
      for (datatype element : list) { ... }
      ```
  - Set: unordered collection without duplicates
      - Instantiate a set:
       ```
       Set<Integer> intSet = new Set<Integer>(1, 2, 3);
       ````
  - Map: a collection of key and value pairs
      - Instantiate a map:
       ```
       Map<String, String> stringMap = new Map<String, String>();
      
       Map<Integer, String> populatedMap = new Map<Integer, String>(1 => 'First, 3 => 'Third');
       ```` 
    
### Apex Logic
- **Control Flow Statements**
	- if, else if and else statements
 	- for loops
  	- for loops for arrays or sets
  	- for loops for soql query
  	- while loop
  	- do {...} while loops (will run at least once)
  	- ternary operator
  	  ```
  	  variable = condition ? if true action : if false action
  	  ```
  	- switch statements (can only be run on strings, ints, and sObjects)
  	  ```
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
  ```
  try {
  	// something you think could fail or error
  } catch ( Exception ex ){
  	throw ex;
  
  	// to call custom exception method:
  	TriggerHandlerClass.throwException(ex.getMessage());
  } 
  ```
	- Custom Exception Class
   
   ```
   public class AccountTriggerException extends Exception {}
   ```
    
  - Custom Exception Method 
    
  ```
    public static void throwException(String message){
  	System.debug(message);
    	throw new AccountTriggerException(message);
  }
  ```

- **Methods of Invoking Apex**
  - Database Trigger, Anonymous Apex, Asynchronous Apex, Web Services, Email Services, Visualforce controllers and Lightning components   
    

### Object-Oriented Concepts
- **Salesforce vs. Apex Objects:**
  - Salesforce:
    - Standard and custom objects are built declaratively and used to organize the data we store in the org.
  - Apex: 
    - Apex objects are developed programmatically and used to organize reusable methods and variables.
   
- **Constructor**
	- located near the top of the class
 	- best practice to include a constructor with no arguments
  	- you will likely need to assign this.arg = arg;  


- **Instantiation**
```
  data_type variableName = new constructor (parameters);
```

- **Variables**
	- All variables are initialized to null by default
 	- Parallel blocks can use the same variable name
  	- Sub-blocks cannot redeclare a parent's block variable name
  	- Can be declare at any point in a block

- **Subclasses**
	- inheritance and polymorphism   
    
### Asynchronous Apex
- **Reasons to Program Asynchronously**
  - Processing a very large number of records. Limits are larger for asynchronous than synchronous processes
  - Making Callouts to external web services
  - Create better faster user experience

- **Future Methods**
  - Syntax
  	- must include @future static void  
  ```
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
    ```
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
  		 ```
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
	
### Testing & Debugging
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
 
<br>
    
## Wednesday, September 19th
### SOQL, SOSL & DML
- **DML**
  - Operations
    - update: use for after triggers
    - upsert: create new and update existing records
    - delete
    - undelete: restores one or more existing sObject records from the recycling bin
    - merge: merges up to three records of the same sObject type into one of the records, deletes the others, and re-parents any related records.
 - Best Practices
    - Always use DMLs with lists over single records
    - DML Governor's Limit: 150 per transaction

- **Salesforce Object Query Language (SOQL)**
	- Syntax: returns list of sObjects, single sObject, integer
   
   ```
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
     
     		```
       		Account[] parentAccounts = [SELECT Id, Name, Phone FROM Account WHERE Id IN :accountIdsSet];
       
       		// " :value " is needed on the right side of the comparison clause
       		// IN can only be used on a set and not a list
       		```
       
       		- query for related records that have a lookup relationship to the Account object
  			```
    		Account[] parentAccounts = [SELECT Id, Name, Phone,
							(SELECT Id, FirstName, LastName FROM Contacts)
							FROM Account WHERE Id IN :accountIdsSet];
    	
    		// Custom relationship fields, use CustomObject__r
    		// Standard relationship fields, use Child Relationship Name instead of Field Name
     		```

  - **Salesforce Object Search Language (SOSL)**
	- Syntax: return type list of list of sObjecs
  	```
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
  	 ```
  	 FIND {Booz Allen Hamilton}
  	 IN NAME FIELDS
  	 RETURNING Account(Id, Name, Phone), Opportunity(Id, Name, AccountId LIMIT 5)
  	 ```
    
- **Dynamic SOQL**
	- Syntax: create string with query line
   ```
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
   			// \' is an escape character which 

   			}
   	List<sObjects> results = Database.query(query);
   	return results;
   }
   ```
   
	- Use Cases
		- Don't know the exact field or conditions
		- Querying dynamic objects

### Custom Metadata Types
- **Topic:**
  - info
    - more info
    
### Platform Events
- **Topic:**
  - info
    - more info

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
