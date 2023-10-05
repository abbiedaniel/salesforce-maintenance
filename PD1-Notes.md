# Platform Developer 1 Notes

:accessibility:  ~[Exam Guide](https://trailhead.salesforce.com/help?article=Salesforce-Certified-Platform-Developer-I-Exam-Guide)~

:accessibility:  ~[Udemy Course](https://bah.udemy.com/course/salesforce-developer/learn/lecture/34602170#overview)~ 

:accessibility:  ~[Practice Test](https://www.salesforceben.com/salesforce-platform-developer-1-practice-exams/)~

:accessibility:  ~[PD1 Trailmix](https://trailhead.salesforce.com/users/strailhead/trailmixes/prepare-for-your-salesforce-platform-developer-i-credential)~

:accessibility:  ~[LWC Basics Badge](https://trailhead.salesforce.com/content/learn/modules/lightning-web-components-basics?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-platform-developer-i-credential)~

:accessibility: ~[Org Deployments Badge](https://trailhead.salesforce.com/content/learn/modules/org-development-model?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-platform-developer-i-credential)~

:accessibility:  ~[Apex Basics & Database Badge](https://trailhead.salesforce.com/content/learn/modules/apex_database?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-platform-developer-i-credential)~

:accessibility:  ~[LWC Specialist Superbadge - 3/6](https://trailhead.salesforce.com/content/learn/superbadges/superbadge_lwc_specialist)~

**This Week:** Oct 4-6

:accessibility:  [Apex Specialist Superbadge - 4/5](https://trailhead.salesforce.com/content/learn/superbadges/superbadge_apex)

:accessibility:  [Visualforce Fundamentals Badge](https://trailhead.salesforce.com/content/learn/modules/visualforce_fundamentals?trailmix_creator_id=strailhead&trailmix_slug=prepare-for-your-salesforce-platform-developer-i-credential)

**Next Week:** Oct 9-11

:accessibility:  [Trailhead Academy Review Video](https://www.youtube.com/watch?v=qHrGpF_IaZI)

:accessibility:  [Focus on Force Practice Exams](https://focusonforce.com/courses/salesforce-certified-platform-developer-1-exams/)

<br>

## Developer Fundamentals - 23%

<details>
	<summary>Salesforce Development Fundamentals</summary>

 ####  Salesforce Development Fundamentals

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

####  Data Modeling & Mangement
	
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

####  Declarative Process & Automation

- **Flows**
  - Screen
  - Schedule-Triggered
    - once, daily, weekly
  - Record-Triggered
    -  Fast Field Updates: before the record is saved
    -  Actions & Related Records: after the record is saved
  - Platform Event-Trigger
  - Autolaunched
  - Flows can be configured to run in system with sharing (record level enforced, ignores  fls), system without sharing (acces to all data), or run in the context it was launched in (either user or system).
  - Flow Trigger Explorer is a tool used for identifying the record-triggered flows that are associated with a specific object and trigger type such as created, updated or deleted. The flow trigger explorer also allows sorting of the execution order of the flows via drag-and-drop.

- **Declarative Caveats**
	- Standard validation rules are unable to operate on parent-child relationships
 	- Roll-up summary fields can only be on the master
  	- Formula fields are calculated at access time and can span multiple objects
  	- Workflow rules cannot submit records for approval or update child records. (blocked since Winter'23) 
  
- **Best Practices**
	- For complex solutions, check if there is an app on AppExchange. If there are no suitable AppExchange apps, only then should custom
development be considered.
 	- For declarative solutions, do not use workflow rules or process builders.
  	- Junction objects are used to represent many-to-many relationships and can prevent orphan records  
    
</details>

<details>
	<summary>Save Order of Execution</summary>
	
 ####  Save Order of Execution

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
      
**S**am's **F**amily **T**ook **V**alerie **D**own **S**outh **T**o **A** **A**uto **W**orkshop's **E**nclosed **F**oyer.
      
  - https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_triggers_order_of_execution.htm

</details>

<details>
	<summary>Layouts & Navigation</summary>

 ####  Layouts & Navigation

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

 ####  Object-Oriented Concepts
      

- **About Apex**
	- Apex is a programming language that uses Java-like syntax and acts like database stored procedures.
 	- Hosted—Apex is saved, compiled, and executed on the server—the Lightning Platform.
 	- Object oriented—Apex supports classes, interfaces, inheritance, abstraction, polymorphism, and encapsulation.
  	- Strongly typed—Apex validates references to objects at compile time.
	- Use Cases—Implement a web service, email service, complex validation over multilple objects and complex business processes, execute server-side logic for a custom Lightning component
 	- Unlike other object-oriented programming languages, Apex supports cloud development as Apex is stored, compiled, and executed in the cloud.
    	 <img width="930" alt="Screen Shot 2023-10-03 at 10 28 36 AM" src="https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/42e5af84-1117-4854-b6a9-5de2b04da30b">

   	
- **Constructor**
	- A constructor is a special method that is used to create an instance of a class. 
 	- Initializes the values of the variables in the object and sets the state of the object.
  	- Has the same name as the class (PascalCase) and does not have a return type.
  	- Constructor is invoked when it's apex class is initialized
  	- If a class does not have a user-defined constructor, a default, no-argument, public constructor is used.
  	- You will likely need to assign ```this.arg = arg;```  


- **Instantiation**
```apex
  data_type variableName = new constructor (parameters);
```

- **Variables**
	- All variables are initialized to null by default
 	- Parallel blocks can use the same variable name
  	- Sub-blocks cannot redeclare a parent's block variable name
  	- Can be declare at any point in a block

- **Classes & Subclasses**
	- Top classes must have one access modifiers (such as public or private). This is not mandatory for inner classes.
 	- Top and Inner class is private by default when no modiferies are specified.
  	- Inner classes can have their own sharing modes declared, which don’t have to match that of the outer classes.
 	-  Inner classes can only be one level deep.

 - **Approval Process**
 	- "Enable email approval response" setting to allow approvers to reveive and respond to approvals via email.
  	- "Allow Approvals" in Chatter setting enables approvers to reveive approval request notifications via Chatter
   	- System Admin and specified Approver can edit records while it's locked for approval.
   	- Approval requests can be sent to individual users or a queue.  
    
</details>

<details>
	<summary>Apex Data Types</summary>

 ####  Apex Data Types

- **Data Types**
  - String
  - Boolean
  - Integer
  - Decimal
  - Id
  - Date  
  - DateTime YYYY-MM-DD HR:MIN:SEC
  - Time
  - Blob for binary data
  - Enum to store a set of identifiers that are accessed one at a time
 
- **sObjects**
  - Standard and Custom Objects
  - Constructor parameters can include field values
  - For custom objects and custom fields, the API name always ends with the __c suffix.
  - For custom relationship fields, the API name ends with the __r suffix
  - Instantiate an Sobject with a constructor:
    ```apex
    Account acc = new Account(Name='Acme');
    // new creates an instance of this object in memory
    acct.Phone = '(415)555-1212';
    // can also use dot notation to also initliaze field values
    ```
   
- **Arrays**
  - List
      ```apex
      // Instantiate a list:
      List<String> stringList = new List<String>();
      List<String> colors = new List<String> { 'red', 'green', 'blue' };
      Account[] accountList = new List<Account>();
      List<Contact> conList = new List<Contact> {new Contact(FirstName='Joe',LastName='Smith',Department='Finance')};

      // List Methods:
      colors.add('orange');
      colors.get(0); // returns red
      colors.size(); // returns 4
      
      // Iterate through a list:
      for (datatype element : list) { ... }
      ```
  - Set: unordered collection without duplicates
       ```apex
       // Instantiate a set:
       Set<Integer> intSet = new Set<Integer>(1, 2, 3);


       // Set Methods: // most are used on sets AND lists
       intSet.add(4);
       intSet.contains(4); // returns true
       intSet.isempty(); // returns false
       intSet.toString(); // returns set of strings instead of integers
       ````
  - Map: a collection of key and value pairs
       ```apex
       // Instantiate a map:
       Map<String, String> stringMap = new Map<String, String>();
       Map<Integer, String> populatedMap = new Map<Integer, String>(1 => 'First', 3 => 'Third');
       
       // Map Methods:
       populatedMap.get(key);
       // Returns the value to which the specified key is mapped, or null if the map contains no value for this key.
       
       populatedMap.put(key, value);
       // Associates the specified value with the specified key in the map.
       
       populatedMap.remove(key);
       // Removes the mapping for the specified key from the map, if present, and returns the corresponding value.
       
       populatedMap.containsKey(key);
       // Returns true if the map contains a mapping for the specified key.
       
       populateMap.clone();
       // Makes a duplicate copy of the map.
       
       populateMap.clear();
       // Removes all of the key-value mappings from the map.
       ````

- **API Data Type and Salesforce Field Types**
  - ID: lookup relationship and master-detail relationship  
  - string: auto number, email, phone, picklist, multi-select picklist, text, text area, long text area, rich text area     
  - boolean: checkbox 
  - double: currency, formula, number, percent, roll-up summary  
</details>
    
<details>
	<summary>Apex Logic</summary>

 ####  Apex Logic

- **Control Flow Statements**
  
	- if, else if and else statements
 		- only 
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
  	  	when 'variable value','another variable value' {
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

 ####  Apex Classes
     
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
  	[public | private | protected | global] [override] [static] [ return_type | void ] method_name(data_type parameter) {
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
	- By default, Apex code in classes, triggers or web services run in system mode where permissions and record sharing of the current user are **not** taken into account.
 	- Inner classes do not inherit sharing settings from outer class. 
 	- ```with sharing``` enforce sharing rules of the current user.
 	-  ```without sharing``` sharing rules for the current user are not enforced. This setting will be used for apex classes that do not include any sharing keywords.
   	- ```inherited sharing``` Inherited sharing takes on the sharing declaration of the class that has executed the code, so if a class with sharing enforced calls a method in a class with inherited sharing, the inherited sharing class code would run with sharing enforced. This is useful for when the sharing model to be used isn’t known at design time, or the code is built to be called from varying places within the system.
   

- **Access Modifiers**
	- ```global``` Can be accessed by any code in your salesforce org. If a method or variable is declared as global, the class must also be global.
   	- ```private``` Can only be accessible in the class it was created in
   	- ```public``` Can be accessed by other apex code in the same namespace
 	- ```protected``` Accessible to any inner classes in the defining Apex class, and to the classes that extend the defining Apex class
 
- **Key Words** 
	- ```static``` method, can be a utility method and is called without instantiating the class it is defined in. Can only be used with methods, variables, and initilization code, and is not supported in class definitions.
 		- Static variables only persist within the context of a single transaction. Static Methods cannot access values of instance members of its class.
 		- No parentheses/constructors needed when accessing static variables.
 	- ```void``` specifies the method does not return a value. 
  	- ```this.``` keyword in JavaScript refers to the top level of the current context. 
  	- ```final``` use to define constants. final indicates the variable can only be initlaized once.

-  **Class Capabilities**
   - Classes can be used to create: 
  		- Trigger Handlers
   		- Controllers for LWC and Visualforce
   		- Invocable methods for flows and Process Builder to call
   		- Web services methods for external services to call

</details>

<details>
	<summary>Apex Triggers</summary>

#### Apex Triggers

- **Types of Triggers**
  
	- Before triggers are used to update or validate record values before they’re saved to the database.
 	- After triggers are used to access field values that are set by the system (such as a record's Id or LastModifiedDate field), and to affect changes in other records. The records that fire the after trigger are read-only.
  	- Using an incorrect trigger context variable in a trigger will caus a ```FinalException``` error.
    
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
	
#### Invocable Apex
	
- **Methods of Invoking Apex**
  - Database Trigger, Anonymous Apex, Asynchronous Apex, Web Services, Email Services, Visualforce controllers and Lightning components   

- **Anonymous Apex**
	- Use the “Execute Anonymous” functionality of the Developer Console
 	- Utilise the REST API ```executeAnonymous``` endpoint
  	- Use the Salesforce CLI ```force:apex:execute``` command
  	- Apex run inside anonymous blocks always executes with the current user context.
 
</details>
    

<details>
	<summary>Asynchronous Apex</summary>

#### Asynchronous Apex

- **Asynchronous Apex**
  
	- Future methods: separate transactions, web service callouts
    - Batch Apex: large data processing, data cleansing or archiving
    - Queueable Apex: sequential processing, external web service callouts
    - Scheduled Apex: scheduled processing, daily or weekly
        
- **Reasons to Program Asynchronously**
  
  - For callouts to external systems, operations that require higher limits, and code that needs to run at a certain time.
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
    	- Future methods are not guaranteed to execute in the same order as they are called.  
    	- You cannot chain future methods and have one call another.
     	- Future methods can’t be used in Visualforce controllers in getter, setter or constructor. 
     	- Max Future Calls per Apex Invocation: 50
      	- Max Invocations for 24 hrs: 250k
     - Benefits: if you want to separate transactions in apex due to CPU usage or governor limits
   
- Use Cases
	- Callouts to external Web services. If you are making callouts from a trigger or after performing a DML operation, you must use a future or queueable method. A callout in a trigger would hold the database connection open for the lifetime of the callout and that is a "no-no" in a multitenant environment.
 	- Operations you want to run in their own thread, when time permits such as some sort of resource-intensive calculation or processing of records.
  	- Isolating DML operations on different sObject types to prevent the mixed DML error. This is somewhat of an edge-case but you may occasionally run across this issue.
  	-  Use future methods instead of queueable is when your functionality is sometimes executed synchronously, and sometimes asynchronously. You can simply create a similar future method that calls your synchronous method.

- **Batch Apex Class**
  - Syntax
    ```apex
    global class BatchableClass implements Database.Batchable<sObject>, Database.Stateful {

    	global Database.QueryLocator start(Database.BatchableContext bc) {
    		// query for records
    		// runs once at the beginnging of apex batch job
    		// With the QueryLocator object, the governor limit for the total number of records retrieved by SOQL queries is bypassed and you can query up to 50 million records
    	}

    	global void execute(Database.BatchableContext bc, List<sObject> scope){
    		// scope is the returned QueryLocator from the start method
    		// The default batch size is 200 records. Batches of records are not guaranteed to execute in the order they are received from the start method.
    		// loop through records and process records
    		
    	}
    	global void finish(Database.BatchableContext bc) {
			// perform actions after data is processed
    	}
    
    }
    ```
	- To execute:
	```apex
  	Id batchId = Database.executeBatch(new BatchableClass(), batchSize);
  	// batchSize maximum == 2,000 records, if value is larger, salesforce automatically makes batch size 2,000
  	// batchSize minimum == 1

  	AsyncApexJob job = [SELECT Id, Status, JobItemsProcessed, TotalJobItems, NumberOfErrors FROM AsyncApexJob WHERE ID = :batchId ];
  	// Each batch Apex invocation creates an AsyncApexJob record so that you can track the job’s progress.
  	// You can view the progress via SOQL or manage your job in the Apex Job Queue. 
   	```
  
  	- Use this if you need to count or summarize records as they’re processed.
      	- ```Database.Stateful``` to maintain state across all transactions. Only instance member variables retain their values between transactions.
 
      	  
  - Limitations:
  	- Troubleshooting can be hard
   	- Jobs are queued and subject to server availability
   
  - Best Practices:
  	- Only use Batch Apex if you have more than one batch of records. If you don't have enough records to run more than one batch, you are probably better off using Queueable Apex.
   	- Tune any SOQL query to gather the records to execute as quickly as possible.
   	- Minimize the number of asynchronous requests, i. e. web callouts, created to minimize the chance of delays.
   	- Use extreme care if you are planning to invoke a batch job from a trigger. You must be able to guarantee that the trigger won’t add more batch jobs than the limit.  

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
        // query for the job to monitor progress
        AsyncApexJob job = [SELECT Id, Status, NumberOfErrors FROM AsyncApexJob WHERE Id = :jobID];

        ```
  
 	- Benefits:
  		- Accepts non-primitive types as parameters
 		- Monitoring - Job ID is returned to identify the job and monitor the progress
  		- Chaining Jobs - You can chain one job to another job by starting a second job from a running job. In the ```execute``` method of the first job, run the ```System.enqueueJob(new SecondJob())```
      
    		- Testing Chained Queueable Jobs - You can’t chain queueable jobs in an Apex test so check if Apex is running in test context by calling ```Test.isRunningTest()``` before chaining jobs.
    		- Max: 50 jobs in the queue with system.enqueueJob  in a single transaction 

- **Scheduled Apex**
  	- Syntax:
	```apex
 	global class ScheduledJob implements Schedulable {
 		global void execute(Schedulable Context SC){}
 	}

 	// to execute class: instantiate the schedulable class
 	String scheduledDateTime = '20 30 8 10 2 ?' //Seconds Minutes Hours Day_of_month Month Day_of_week optional_year
 	String jobID = System.schedule('Job Title' , scheduledDateTime, new ScheduledJob() );
 	```
  	- Max: 100 scheduled apex jobs at a time
  	- To Schedule the job:
  		- Use Apex Scheduler: search apex classes in setup and click Schedule Apex
  			- Weekly or monthly basis 
     		- Use the ```System.schedule``` method within apex
</details>	
 
<details>
	<summary>Data Manipulation Language (DML)</summary>

 #### DML

- **Use Cases**
	- Use DML to create and modify records in Salesforce.
 	- Use SOQL to retrieve records for a single object.
  	- Use SOSL to search text fields across multiple objects. 

- **Data Manipulation Language (DML)**
  
  - Operations
    - ```insert```
    - ```update``` use for after triggers
    - ```upsert``` create new and update existing records. Errors out if a key is matched multiple times. You can specify a sObject record's primary key (the ID), an idLookup field, or an external ID to match.
    - ```delete``` records in the Recycle Bin for 15 days from where they can be restored. Supports cascading deletions. If you delete a parent object, you delete its children automatically, as long as each child record can be deleted.
    - ```undelete``` restores one or more existing sObject records from the recycling bin.
    	- Parent and child records are supported. Custom lookups that haven't been replaced can be restored. 
    - ```merge``` merges up to three records of the same sObject type into one of the records, deletes the others, and re-parents any related records.
    - ```Database.insert(sObjectList, allOrNone)``` You can also call the built-in Database class static methods. Optional allOrNone parameter that allows you to specify whether the operation should partially succeed.
    		- The Database methods return result objects containing success or failure information for each record in ```Database.SaveResults, Database.DeleteResult, Database.UpsertResult``` objects. These objects have getErrors() and isSucess() methods.
      
- **DML Best Practices**
    - Always use DMLs with lists over single records
    - Profiles and Record Types do not suport DML operations
    - DML Governor's Limit: 150 per transaction

</details>

<details>
	<summary>Salesforce Object Query Language (SOQL)</summary>
	
  #### SOQL

- **Salesforce Object Query Language (SOQL)**
  
	- Syntax: returns list of sObjects, single sObject, integer
   
   ```apex
   [ SELECT one or more fields,
   	// inner query for child/related objects
   	// inner query objects must be PLURAL
    	(SELECT fields
   	FROM Child Objects or Custom Child Objects with appeneded __r)
   FROM an object
   WHERE (field=value OR|AND anotherField>value)
   ORDER BY field ASC|DESC
   LIMIT n
   ]
    ```
	- Capabilities in Salesforce
 		- query in the query editor in the developer console
   		- query in apex code
     
     		```apex
       		Account[] parentAccounts = [SELECT Id, Name, Phone FROM Account WHERE Id IN :accountIdsSet ORDER BY Name ASC LIMIT 10];
       
       
       		// " :value " is needed on the right side of the comparison clause.
       		// SOQL statements in Apex can reference Apex code variables and expressions if they are preceded by a colon. Example: field=:apexReferenceVariable
       		// IN can only be used on a set and not a list
       		```
       
       		- query for related records that have a lookup relationship to the Account object
  			```apex
    		Account[] parentAccounts = [SELECT Id, Name, Phone,
							(SELECT Id, FirstName, LastName FROM Contacts)
							FROM Account WHERE Id IN :accountIdsSet];
    	
    		// Custom relationship fields, use CustomObject__r
    		// Standard relationship fields, use Child Relationship Name instead of Field Name



     		//SOQL For Loop
     		for (data_type variable|variableList : [soql_query]) {
    			// the loop executes once for each batch of 200 sObjects
     			// example: if you were to increment integer i in this code block,
     			// it would only increase once if the query returns less than 200 records
     		}
     		```

     
<img width="1327" alt="Screen Shot 2023-10-03 at 6 17 15 PM" src="https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/817e1ec3-9551-453b-9f5e-650bde8ed97e">
<img width="1325" alt="Screen Shot 2023-10-03 at 6 17 43 PM" src="https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/51c5db98-ac74-4dd3-8ab6-e1a9d2b13a63">



- **SOQL Tips**
	- You don’t have to specify the Id field in a SOQL query as it is always returned in Apex queries, whether it is specified in the query or not.
 	- Use ```GeolocationField__Lattitude__s``` and ```GeolocationField__Longitude__s``` to retreive latitude and longitude values of a geloation field
 	- Invalid SOQL syntax results in query exception
  	- GROUP BY clause can be used together with the COUNT(fieldName) aggregate function to return data that groups and counts the number of records based on a field value.
  		- Example: ```SELECT LeadSource, COUNT(Name) FROM Lead GROUP BY LeadSource```
  	 - SOQL does not support the asterisk * wild card character
  	 - Heap Limits: Use ```transient``` keyword prevents an Apex instance variable from being transmitted as part of the view state of a Visualforce page, which reduces view state size. Use SOQL loops for controlled batches. Clear variables once done to clear up memory.
  	 - Backslash Escape Character in SOQL:  \'  or \" or \n or \? etc.
  	 - The % and _ wildcards are suppored for with the LIKE operator.
  	 	- 'xyz%' matches zero or more characters
  	  	- '_xyz' matches exactly one character 

</details>

<details>
	<summary>Salesforce Object Search Language (SOSL)</summary>

 ### SOSL

- **Salesforce Object Search Language (SOSL)**
  
	- Syntax: return type list of list of sObjects
 		- SearchQuery is the text to search for (a single word or a phrase). Text searches are case-insensitive.
  		- Search terms can be grouped with logical operators (AND, OR) and parentheses.
  		- Also, search terms can include wildcard characters (*, ?). The * wildcard matches zero or more characters at the middle or end of the search term. The ? wildcard matches only one character at the middle or end of the search term.    
   
  	```apex
   	FIND {Search Query Text} 
    	// this line is required
   	// apex uses 'SearchQuery ', query editor uses {SearchQuery}
   	// Use double quotes for a specific phrase i.e. "hello world" 
   
   	[ IN SearchGroup ]
   	// If not specified, the default search scope is all fields.
   	// Search Group Options: ALL FIELDS, NAME FIELDS, EMAIL FIELDS, PHONE FIELDS, SIDEBAR FIELDS
   
   	[ RETURNING FieldSpec [[ toLabel(fields) ] [ convertCurrency(Amount) ] [ FORMAT() ] ] ]
   	// optional RETURNING clause specifies the information that should be returned in the search result i.e. Account(Name)
   	//  If not specified, the search results contain the IDs of all objects found.
   	// You can also use WHERE, LIMIT and ORDER BY in the returning clause
   
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
  	  	// apex
  	  	List<List<sObject>> results = [FIND :name IN NAME FIELDS RETURNING Contact(Name), Lead(Name)];
  	  
  	  	// query editor
  	  	FIND {Booz Allen Hamilton}
  	  	IN NAME FIELDS
  	  	RETURNING Account(Id, Name, Phone WHERE Industry='Apparel' ORDER BY Name), Opportunity(Id, Name, AccountId LIMIT 5)
  	 	```

- **SOSL Tips**
	- Results are evenly distributed among the returned objects, ig a limit is set on the entiure query. Limits can also be set per individual object. 

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

 #### Governor Limits
	
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
	
 #### Custom Metadata Types

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

 #### Platform Events

- **Custom Platform Events**
  - Platform events connect business processes in Salesforce and external apps through the exchange of real-time event data. Platform events are secure and scalable messages that contain data. Publishers publish event messages that subscribers receive in real time. To customize the data published, define platform event fields.
  - Create in setup. 
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


<details>
	<summary>Apex Integration</summary>

 ####  Apex Integration

- **Apex Callout**
  
	- An Apex callout enables you to tightly integrate your Apex code with an external service. The callout makes a call to an external web service or sends an HTTP request from Apex code, and then receives the response.
 	- Apex callouts come in two flavors:
 		- Web service callouts to SOAP web services use XML, and typically require a WSDL document for code generation.
  		- HTTP callouts to services typically use REST with JSON. Can also be used with SOAP.
	- SOAP or REST? REST services are typically easier to interact with, require much less code, and utilize easily readable JSON. SOAP web services are commonly used for enterprise applications, integrating with legacy applications or for transactions that require a formal exchange format or stateful operations.


- **HyperText Transfer Protocol  Review**
	- Each callout request is associated with an HTTP method and an endpoint URL. The HTTP method indicates what type of action is desired.
 	- HTTP Methods: GET, POST, DELETE PUT
 	- Status Codes: 200 OK, 404 NOT FOUND, 500 INTERNAL SERVER ERROR
  	- To create HTTP request and store response:
  
   	```apex
    // instantiate http class and http request class
    Http http = new Http();
	HttpRequest request = new HttpRequest();
    
    // define endpoint service
	request.setEndpoint('https://th-apex-http-callout.herokuapp.com/animals');
    
    // use GET to get data from a service 
	request.setMethod('GET');
    
    // use POST and set header and body to send data to a service
    request.setMethod('POST')
    request.setHeader();
    request.setBody()
    
    // store response from request
	HttpResponse response = http.send(request);
    
    if (response.getStatusCode === 200){
    	// deserialize JSON string and store in a Map<string, Object>
    }
    ```
    
- **Testing HTTP REST Callouts**
	- Apex test methods don’t support callouts, and tests that perform callouts fail. To test your callouts, use mock callouts by either implementing an interface or using static resources.
 	- Static Resource
    	```apex
     	// use apex's static resource callout mock class
     	StaticResourceCalloutMock mock = new StaticResourceCalloutMock();

     	// Instead of sending the request to the endpoint, instead the Apex runtime knows to return the the response specified in the static resource
        mock.setStaticResource('StaticResourceFileName');
     	
     	// The Test.setMock method informs the runtime that mock callouts are used in the test method. 
        Test.setMock(HttpCalloutMock.class, mock);
     	```
     
 	- Have your mock callout class ```implements HttpCalloutMock``` and implement the ```respond(HTTPRequest)``` interface method. Use ```test.setMock(HttpCalloutMock.class, new MockCalloutClass())```

- **Best Practice**
	- Place the callout code in an asynchronous method that’s annotated with @future(callout=true) or use Queueable Apex. This way, the callout runs on a separate thread, and the code after the callout isn't blocked.

- **SOAP Services**
	- Download the web service's WSDL file, and go to  SetUp to upload the WSDL file. The WSDL2Apex generates the Apex classes. Then create a class to make the callout.
	- WSDL Apex code must have code coverage. Create a callout mock class and a test class. Then use ```Test.setMock(WebServiceMock.class, new CalloutMockClass()``` in the test class.
  
 </details>

<br>

## Testing, Debugging & Deployments - 22%

<details>
	<summary>Test Classes</summary>

#### Test Classes

- **Purpose**
	- Used to determine whether a piece of code is behaving exactly as it was intended to.
 	- Three Parts to Testing: 
 		- **Setup**: preparing data and the runtime environment for your testing scenario. 
  		- **Execution**: executing the code you wish to test
  		- **Validation**: verifying the results of the executed test against the expected results
    - https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_qs_test.htm
      

- **Best Practices**
  
	- Do not access live data in your org in tests.
 	- Test classes can be either private or public. If you’re using a test class for unit testing only, declare it as private. Public test classes are typically used for test data factory classes.
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
  
  		// actions to test: could be a DML statement
  		update testAccount;

  
  		Test.stopTest();

  		// query for most up-to-date record and values
  		updatedTestAccount = [ SELECT Id, Name From Account, where ID =: testAccount.Id ];
  
  		System.assert( boolean condition );
  		System.assertEquals( expected, actual );
  	}

  	// you can use testMethod type instead of @isTest
  	static testMethod void mytestMethod(){
  	}
  
  }
  
  ```
  
</details>

<details>
	<summary>Exception Handling</summary>

 #### Exception Handling
	
- **Try/Catch Block**
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
  
- ```allorNone``` **Boolean:**
	- ```false``` allows partial success if an error is thrown. Instead of an exception being thrown when any record encounters an error during save, a ```List<Database.SaveResult>``` is returned instead of an exception being thrown.
  		```apex
  		Database.insert(recordToInsert, allOrNone, accessLevel);
  		// When we wish to configure the DML operation, or handle failed records,
  		// we must use the Database class methods.
 		```

</details>

<details>
	<summary>Trigger Exceptions</summary>

 #### Trigger Exceptions
 
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

 #### Testing Asynchronous Apex

- Similar as any test class (needs ```Test.startTest``` & ```Test.stopTest```), but use specific methods for
  
	- Queueable Apex: ```System.enqueuJob(new QueueableClass());```
  	- Batchable Apex: ```Database.executeBatch(new BatchableClass(), batchSize);```
  	- Scheduled Apex: ```System.schedule('Job Title' , scheduledDateTime, new ScheduledJob() );```
  	- Future Methods: call the future method between startTest and stopTest
  	  
 </details>

<details>
	<summary>Code Coverage</summary>

 #### Code Coverage

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

 #### Execution Log

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
	<summary>LWC Error Handling</summary>

#### LWC Error Handling

- **Wired Properties**
	- Use ```.error``` on the wired property/field
 	- Import reduceErrors from ldsUtils and use ```reduceErrors(field.errors)``` to format the error
  	- Create a getter for errors to define the errors property and add errors to html and display if true
  	   
- **Wired Functions**
	- define an errors variable/property
 	- When you use @wire to decorate a function, the function receives as a parameter an object that includes errors (if there are any errors). ``` wiredFunction({data, error})```
  	- Import reduceErrors from ldsUtils and use ```reduceErrors(error)``` on the error parameter to format the error and associate it to the errors variable
  	  
- **Imperative Function Calls**
	-  Define an errors variable/property
 	-  Import reduceErrors from ldsUtils   
 	-  Imperative apex calls are created with a promise (```.then(result``` and ```.catch(error``).
  	-  If the promise is rejected, we use the reduceErrors helper function to format the received error and store it in the errors property.
 
	<img width="950" alt="Screen Shot 2023-09-28 at 3 06 36 PM 1" src="https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/75550f8b-9681-4fbf-9ba6-1bccc2c12b5f">

  	
 </details> 

 <details>
	<summary>Deployments</summary>

 #### Deployments

- **Deployment Tools**
	- VSCode and Salesforce Extension Pack: deploy and retrieve from orgs and write, debug, and refactor code
 	- Developer Console: create, debug, and test applications
  	- Change Sets: deploy workflows, rules, Apex classes and triggers, and other customization from a sandbox org to your production org 
 	- Metadata API: retrieve, deploy, create, update or delete customization information, such as custom object definitions and page layouts, for your org. This API is intended for managing customizations and for building tools that can manage the metadata model, not the data itself.
  	- Ant Migration Tool: perform a file-based deployment of metadata changes and Apex classes from a Developer Edition
  

- **Change Sets**
	- Deployment Connection: A deployment connection is required between two Salesforce orgs to send change sets from one org to another. You can’t create deployment connections between arbitrary orgs. Instead, you create connections between all orgs affiliated with a production org.
 	- Authorize inbound changes so that another Salesforce org can send change sets to the org you are logged into.
  	- Tyoes: Inboard Change Sets and Outbound Change Sets  
 	- Target & Source Pairs: Sandbox to Sandbox OR Sandbox to Prod

- **Salesforce CLI Capabilities**
  
	- Authorize sandboxes (headless or web flow)
	- Create and manage DX projects
	- Import and export test data
	- Retrieve and deploy metadata
	- Run and automate tests

  
- **Deprecration**
	- Apex Classes & Metadata

   		- Apex classes and some other metadata cannot be directly deleted from production. While these pieces of metadata can be deleted within a Sandbox, changesets cannot upload these destructive changes. The Metadata API must be used for deleting apex classes. This could be with a tool such as ANT to produce a destructive changeset which is deployed into the org.
   	 		  
	- Fields
       		- Remove all references of this field in the org before deleting

- **Scratch Orgs**
 	- Enable Dev Hub to allow scratch orgs to be created
   	- Have a user with permissions to create scratch orgs
   	- Have the Salesforce CLI setup to log into the dev hub and request scratch org creation
   	  
 - **Sandboxes**

   <img width="940" alt="Screen Shot 2023-10-02 at 6 57 43 PM" src="https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/e1ded12c-8f6a-47a4-9bfe-161608df0a3d">

 	- **Developer Sandbox:** A Developer sandbox is intended for **development and testing** in an isolated environment. A Developer Sandbox includes a copy of your production org’s configuration (metadata).

   	- **Developer Pro Sandbox:** A Developer Pro sandbox is intended for development and testing in an isolated environment and can host larger data sets than a Developer sandbox. A Developer Pro sandbox includes a copy of your production org’s configuration (metadata). Use a Developer Pro sandbox to **handle more development and quality assurance tasks and for integration testing or user training.** 
   	- **Partial Copy Sandbox:** A Partial Copy sandbox is intended to be used as a **testing environment.** This environment includes a copy of your production org’s configuration (metadata) and **a sample of your production org’s data** as defined by a sandbox template. Use a Partial Copy sandbox for quality assurance tasks such as **user acceptance testing, integration testing, and training.**
   	- **Full Sandbox:** A Full sandbox is intended to be used as a **testing environment.** Only Full sandboxes support **performance testing, load testing, and staging.** Full sandboxes are a **replica of your production org,** including all data, such as object records and attachments, and metadata. The length of the refresh interval makes it difficult to use Full sandboxes for development.
    

- **Salesforce DX**
	- The Salesforce DX project contains the source and files that comprise your changes. A DX project has a specific project structure and source format.
	- In addition to source files, the project contains a configuration file, sfdx-project.json. This file contains project information and enables you to leverage Salesforce DX tools for many of your development tasks.
 	-  Manifest file (package.xml) lists the components to be deployed
  	- Deployment Steps (IDE & Source Control, No Copado):
  		- Build Release Artifact
  	 	- Test the Release Artifact in the Test (Partial) Sandbox
  	  	- Test the Release Artifact in the Staging (Full) Sandbox
  	  	- Release to Production
  	  	- Perform Post-Deployment Tasks Listed in Deployment Run List   

</details>

<br>

## User Interface - 25%

<details>
	<summary>Extending Declarative Functionality: Flows</summary>   

 #### Extending Declarative Functionality: Flows

- **Invocable Methods**
   
   ```apex
    	@InvocableMethod( callout = true label = 'methodName' description = 'description' category = 'DML')
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

 #### Visualforce Pages

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
	- To reference an instantiation of a page:
		 - Use  ```PageReference pageRef = new PageReference('URL');``` or ```Page.visualforcePageName```
 	- To add a related record's field name to a Visualforce page, use formula field syntax 
		- Reference the object's fields using ```{!opportunity.Account.fieldName}``` in a standard controller
 	- To generate a simple PDF
 		- create a visualforce page with ```renderAs="pdf"```
   	- To add pagination to a page
   		- The ```StandardSetController``` is designed to work with sets of records, and provides built-in methods to enable a large set of records to be displayed on a Visualforce page, with methods to assist in pagination of the record list.
    
</details> 


<details>
	<summary>Visualforce Controllers</summary>

 #### Visualforce Controllers
	
- **Basic Controller**
  
	- Visualforce Page: ```EditPage.vfp```
   
	```apex
	<apex:page controller = "EditPageController">
 
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
	<summary>Lightning Web Components</summary>   

 #### Lightning Web Components

- **LWC Framework**
  
	- HTML provides the structure for your component.
	- JavaScript defines the core business logic and event handling.
	- CSS provides the look, feel, and animation for your component. Use Salesforce Lightning Design System (SLDS), a CSS framework, to provides a look and feel that's consistent with Lightning Experience. 
 	- Salesforce-specific JS-META.XML metadata file
    
- **LWC Benefits**
  	- Lightweight for faster development
 	- Faster performance
  	- Out-of-the-box components
  	- Built upon web standards 
  
- **LWC Characteristics**
  
 	- Dev tools needed for LWCs are VSCode and Salesforce Extension Pack
  	- LWC can be used in Lightning Experience apps, Experiences, and Salesforce Mobile App
  	- LWC requires all third-party resources, like Javascript and CSS, to be uploaded as Static Resources and loaded through the Platform Resource Loader
  	- LWC must be named in camelCase
  	- A single property or function can have only one decorator. For example, a property can't have @api and @wire decorators.



- **Lifecycle Hooks**
	- Lightning Web Components provides methods that allow you to “hook” your code up to critical events in a component's lifecycle. These events include when a component is:
 		- Created
   		- Added to the DOM
       - Rendered in the browser
       - Encountering errors
       - Removed from the DOM
       - Respond to any of these lifecycle events using callback methods.

- **LWC Decorators**
	- ```@api``` Marks a field as public. Public properties define the API for a component. An owner component that uses the component in its HTML markup can access the component's public properties. All public properties are reactive, which means that the framework observes the property for changes. When the property's value changes, the framework reacts and rerenders the component.
 	- ```@track``` Tells the framework to observe changes to the properties of an object or to the elements of an array. If a change occurs, the framework rerenders the component. All fields are reactive. If the value of a field changes and the field is used in a template—or in the getter of a property used in a template—the framework rerenders the component. You don't need to decorate the field with @track. Use @track only if a field contains an object or an array and if you want the framework to observe changes to the properties of the object or to the elements of the array. If you want to change the value of the whole property, you don't need to use @track.
  	- ```@wire``` Gives you an easy way to get and bind data from a Salesforce org.
  		- Syntax
  	   	```apex
  	    import { adapterId } from 'adapter-module';
		@wire(adapterId, adapterConfig)
		propertyOrFunction; // A private property or function that receives the stream of data from the wire service.
  	    	// If a property is decorated with @wire, the results are returned to the property's data property or error property.
  	    	// If a function is decorated with @wire, the results are returned in an object with a data property and an error property.
  	    ```   

- **Basic Component**
  
```javascript
// JavaScript File: home.js

import {LightningElement} from 'lwc';

export default class Home extends LightningElement{

	message = "Hello World"; 
}

__________________________________________________

// HTML File: home.html
// how to pass the parameter message from the parent home.js into a child event customMessage.js
   
<template>

// to reference a custom component add c- with all lower case
// sets the custom component's message attribute to the message variable in the Home class 
	<c-custom-message> message={message} </c-custom-message>

</template>
	
__________________________________________________
 
// Metadata File: home.js-meta.xml
  	   
<LightningComponentBundle xlmns = "salesforce soap metadata URL">
	<apiVersion> 55.0 </apiVersion>
	<isExposed> true </isExposed> // allows the component to be visible in the lightning app builder

	<targets>
		<target> lightning__HomePage </tagets> // location on where the file can be used //lightning__RecordPage _AppPage, _flowScreen, etc.
	</targets>
</LightningComponentBundle>

__________________________________________________

// Optional CSS File: home.css

.test{
	background-color: aliceblue;
}

```

- **HTML Specifications**
  
	- Can use ```<tempalate if:true={methodThatReturnsBoolean}``` to only render if true.
	- ```<template for:each={listName.data} for:item={elemnt}>``` to iterate HTML
 	- Any components that aren’t base HTML tags (i.e. custom lwc), it is required that no component tags are self-closing (i.e., there is always an explicit closing tag)
  	- HTHML Comment Format ```<!-- text here -->```

- **Component Composition**
  
```javascript
// Javascript File: customMessage.js
   
import {LightningElement, api} from 'lwc';

export default class customMessage extends LightningElement{

 	@api  // allows variables to pass in as an attribute
    	message; // stores value from home page input and stores here
}

__________________________________________________
 
// HTML File: customMessage.html

<template>
 
 	// custom css and styling
 	// slds-box is a class from the lightning design system 
 	<div class = "slds-box" style="background-color: white;">
 
 		{message}
 		// creates a simple white box with the message inside
 		// referencing message variable in customMessage class
 
 	</div>
</template>

__________________________________________________
 
// Metadata File: customMessage.js-meta.xml

<LightningComponentBundle xlmns = "salesforce soap metadata url">
	<apiVersion> 55.0 </apiVersion>
	
	<isExposed> true </isExposed
	// allows the component to be visible in the lightning app builder

	<targets>
		<target> lightning__HomePage </tagets>
		// location on where the file can be used
  
	</targets>
</LightningComponentBundle>

__________________________________________________

// Optional CSS File: customMessage.css
   
.test{
	background-color: aliceblue;
}

```
   
- **Events**
  
	-  All event names must not use uppercase letters, have no spaces and use underscores to separate words
  	- ```this.dispatchEvent( my CustomEvent( "my_event", {detail: this.recordId} ))``` Lightning Web Components utilize the standard CustomEvent class within JavaScript, which is then dispatched through the EventTarget.dispatchEvent() method, which in the majority of cases, would be this.dispatchEvent(). Since we would want parent components to handle this event. We add information to the event with the detail property of CustomEvent, which the event handlers can access and process accordingly. The detail property can be any datatype.
  	
```javascript

// JavaScript File: customMessage.js 

clickHandler(){

	// instantiates a new event using the CustomEvent class 
  	const clickEvent = new CustomEvent(

		// this is the event name
  		'clicked',

  		// parameters for the event.detail
  		{ detail: 'CLICKED!' }
  	);
  
  	// dispatch event and passes to the home parent event
  	this.dispatchEvent(clickEvent);
  }

__________________________________________________
  
// HTML File: customMessage.html
   
<template>

	// when this div is clicked the clickHandler method is called and the clickEvent is dispatched to the parent 
	<div class = "slds-box" style = "background-color: white;"
   
		onclick = {clickHandler}">
		// add onclick event 
		// add the clickHandler method as an attribute
		
			{message}
	</div>
</template>
```

<br>



```javascript
// HTML File: home.html
   
<template>
 
	// home is the parent event since it references customMessage 
	<c-custom-message> message={message}
 				
		onclicked = {handleClicked}
		// reference event using 'on' and the event name, in this case, the 'clicked' event from customMessage.js
 
	</c-custom-message>
</template>

__________________________________________________

// JavaScript File: home.js

import {LightningElement} from 'lwc';

export default class Home extends LightningElement{

	message = "Hello World"; // default message
	handleClicked(event){

		// set  the message to be the event detail which is 'CLICKED!' from  customMessage.js
		this.message = event.detail;

 		}
	}
```

<img width="935" alt="Screen Shot 2023-09-29 at 10 36 12 AM" src="https://github.com/abbiedaniel/salesforce-maintenance/assets/116677150/e627e5b0-2fb5-44a5-bcaf-5d95860420ff">

- **From Child to Parent**
  
	-  In the child components, the handler method use ```dispatchEvents()``` to send the event name and parameters to the parent component.
 	-  The parent component recieves the event from the child and parameters using ```event.detail``` in the handler method.
  	-  The child and parent javascript handler methods are referenced in the corresponding html files to run ```onaction```.
  	-  Note: By default, a custom event doesn't bubble up past the host. Use ```bubbles:true```.
  	  
  		-  Example: ```controls``` is the host for the custom event ```button``` because it contains a reference to <c-button>.
  	   
  	   ```html
  	   <lightning-layout-item flexibility="auto" padding="around-small" onbuttonclick={handleDivision}>
			<template for:each={factors} for:item="factor">
          		<c-button/>
  	   ```
  		 -  Solution: To allow the event (```buttonclick```) to bubble up to the ```lightning-layout-item```, we add ```bubbles: true``` in the ```CustomEvent``` by calling ```this.dispatchEvent(new CustomEvent('buttonclick',{ bubbles: true }``` in the button handler method.
  	 
 

  	-  Example: Child Event - ```controller```, Parent Event - ```numerator```. 
  		-  In the child event ```controls```, the handler method uses ```dispatchEvent``` to send the event name and parameters to the parent ```numerator```. The controls html specifies to run this method ```onbuttonclick```.
  	 	- 	```javascript
  	     	// dispatchs event to parent with event name and parameter factor
  	     
  	     	handleMultiply(event) {
  	     		const factor = event.target.dataset.factor;
  	     		this.dispatchEvent(new CustomEvent('multiply', { detail: factor }));
     		}
  
  	 	-  In Parent event ```numerator```, the parent handler method receives the event from the child ```controller``` and uses the event as a parameter and sets the factor property as the ```event.detail```. The numerator html specifies to run this method ```onclick```.
  	 	


  		```javascript
 		// receives event from child controller
 
  		handleMultiply(event) {
  			const factor = event.detail;
  			this.counter *= factor;
  		}
  		```	


- **From Parent to Child**
  
	- Child component uses ```@api``` decorator on the input variable to allow the child to receive input from parent.
 	- Child component contains process method for parent to reference. Use the ```@api``` decorator on the method, so the parent component can call it.
  	- Parent's html contains a reference to the child component.
  	- In the parent component, the handler method  uses  ```this.template.querySelector(c-childcomponent).childComponentMethod;``` to call the child component's method. Reference this handler method in the html to run ```onaction```


 

- **Lightning Base Components**
  
	- Helpful Components in  Lightning Component library
 	```html
  	<lightning-button> </lightning-button>
	<lightning-button-group> </lightning-button-group>
	<lightning-datatable> </lightning-datatable>
	<lightning-combobox> </lightning-combobox> // use this for picklists and dropdowns
	<lightning-card> </ligthning-card> // puts text in a card format
	<lightning-input> </lightning-input> // input field

	import from 'lightning/empAPI' // provides access to methods for subscribing to a streaming channel and listening to event messages aka subscribe to platform events
  ```


- **Retrieve Salesforce Data**
  
	- Create Apex Controllers to retrieve data for LWC
 	- Apex Controller methods must be ```@AuraEnabled(cacheable = true)``` to be accessible for LWC. Cacheable is needed when you are retrieving records and you do not need it if you are inserting or updating records.
  	- Import the apex aura enabled method in the LWC using ```import methodName from '@salesforce/apex/ControllerName.methodName';```
  	- LWC must have ```import wire from 'lwc'``` to allow the javascript to interact with apex.
  		- Use ```@wire(methodName, {methodParameters: '$api_variable_in_lwc'}) variable;``` to call the apex method and save the return value. 	```'$ api_variable_in_lwc'``` allows us to dynamically reference the variable.
  

- **Create Salesforce Data**
  
	- Use the ```<lightning-input/>``` component and include your method in the ```onchange``` attribute. In the method, set the variable to display as ```event.target.value```
 	- Import the apex method and use a promise to handle an asynchronous result. And assumes it is true, if it is not true then handles it by catching an error
  	  ```javascript
  	  handleSave(){
  	  	apexControllerMethod({ apexParameter : this.lwc_variable })
  	  		.then( result => {
  	  			// actions to run if the apexControllerMethod is run successfully and result is true
  	  		})
  	  		.catch( error => {
  	  			// actions to run if apexControllerMethod errors out
  	  		});
  	  }
  	  ``` 
	- Add the LWC to the Lightning page using the Lightning App Builder
   
- **Lightning Message Service**
  
	- Use LMS for **communication between unrelated components** unless you control both components and a common parent.
 	- Need Two Components: ```remote``` and ```receiver```
  		- Create a message channel folder and a specific-message channel metadata file
    		- Import the channel metadata file in both ```remote``` and ```receiver```
  		- In the remote component:
    			- Import ```publish``` and ```@wire MessageConext``` from lightning/messageService
      			- The data payload is sent with the ```publish``` function
     
    		- In the receiver component:
      			- Import ```subscribe``` and ```MessageConext``` from lightning/messageService
         		- The is sent with the ```subscribe``` function
           		- ```connectCallback``` function to run when a component is loaded. Inside this method, you can pass parameters to handler functions
  	- Use Cases: Need to communicate between components with a parent that you can’t control, such as two App Builder slots

- **Lightning Data Service**
  
	- Use Lightning Data Service to load, create, edit, or delete a record in your component without requiring Apex code. Lightning Data Service handles sharing rules and field-level security for you.
  	- When building components that work on individual records, the Lightning Data Service provides a performant and cached mechanism for loading and updating record data that gets propagated throughout all components utilizing the service. This offers advantages over performing Apex calls to achieve simple record data since it increases performance and allows changes in other areas of the UI (for example for the standard record details component) to propagate to other components.
  	- LDS is available through ```force:recordData```  and other base components
  

- **LWC Security Best Practice**
    
	- Sanitize any user input!
 	- Add the ```WITH SECURITY_ENFORCED``` clause to the query to enforce permissions on the query, so if a query attempts to access a field or object the user doesn’t have access to, an exception is thrown.
  	- Use bind variables for user input to ensure values are treated as values and not accidentally interpreted as extensions to a query.
  	- Hardcode the filterable fields in the Apex controller. A piece of Apex should never trust search parameters from a Lightning Component as these could easily be manipulated. Instead, in scenarios where this is required, alternative approaches should be used such as hardcoding the filter variables in an Apex datatype or as parameters to the method, in order to ensure that any requested fields/filters have been explicitly pre-authorized.
  	- Utilize the ```with sharing``` keyword on the Apex class
</details>


<details>
	<summary>Lightning Aura Components</summary>  
	
 #### Lightning Aura Components

- **LWC vs. Aura Components**
	- Developed with HTML and Javascript
 	- Created with original lightning experience, more specialized for the Salesforce platform
  	- Choose LWC over Aura Components
  	- Case sensitive, variable references must be in quotes
  	- LWC can be inside an Aura Component markup, but you cannot have an Aura component in an LWC markup
  	- Server-side Lightning component controllers, or @AuraEnabled classe, use with sharing if no sharing keywords is defined on the class. Record level security is enforced by default in custom lightning components.
     
- **Aura Component Framework**
  
	- ```AuraComponent.auradoc``` documentation
 	- ```AuraComponent.cmp``` base page of the componet
  	- ```AuraComponent.cmp-meta.xml``` Salesforce metadata file for offline development
  	- ```AuraComponent.css``` standard styling sheet
  	- ```AuraComponent.design``` design component is where we place our configurations to make the component available in different locations in salesforce
  	- ```AuraComponent.svg```  icon component drawing
  	- ```AuraComponentController.js``` main logic called from component
  	- ```AuraComponentHelper.js``` helper methods called by controller
  	- ```AuraComponentRenderer.js``` runs code and overrides basic rendering of component
 
- **Basic Aura Component**
	
	- AuraComponent.cmp
 		- variables are stored in tags and methods are in the controller
		```html

		<aura:component implements = "flexipage:availableForAllPageTypes,force:hasRecordId" controller = "ApexControllerName">
		// implements allows the component to be accessed in lightning pages
  		// force:hasRecordId to display on a record page
		// defines controller
  		

			// handler name is init
			// parameter is this, must used "{! }"
			// c.doInit references the controller's doInit method. This method runs when the component initializes/loads
			<aura:handler name="init" value = "{!this}" action={!c.doInit}/>

  			// create variables for controller
  			<aura:attribute name = "recordId" type = "String"/>
  			<aura:attribute name = "account" type = "Account"/>

  			// from here add display components
		</aura:component>
		```

	- AuraComponentController.js

		```javascript
		({
			methodName : function(component, event, helper){
  			
				// get apex method from component
				var action = component.get("c.methodName");

  				// get parameters for apex method and set parameter as attribute
				action.setParams({
					"apexParameter" : component.get("v.recordId")
				});

				// this runs when the apex method is done running
				action.setCallback(this, function(response){
  
  					// best practice is to get the state
  					var state = response.getState();
  
  					if (state === 'SUCCESS'){
  						// set the component attribute with the return value
  						component.set("v.account", response.getReturnValue());
	  				}

 				 });
  				// run action 
  				$A.enqueueAction(action);
			}
		})
		```
  
- **Events**
  
	- Use events to communicate from child to parent event
 	- create two aura events ```.evt``` and two controllers ```.js``` for each event
  	- Use ```event.setParams``` and ```event.fire``` on the child controller to pass the method parameters to the parent
  	- Use ```component.set``` and ```component.get``` to initialize or retrieve the component


- **Aura Enabled Important Methods & Signatures**
  
	- ```@AuraEnabled(cacheable=true)``` improves the runtime performance of Lightning Components on the Aura Enabled apex methods that are frequently used in multiple LWCs by caching the result on the client side [resource](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/controllers_server_apex_auraenabled_annotation.htm)
   
	 - ```Security.stripInaccessible(AccessType, sourceRecords)``` enforces the FLS of the current user by stripping anything which is not accessible in the defined context in @AuraEnabled methods [resource](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_classes_with_security_stripInaccessible.htm) 

	- ``` @AuraEnabled(cacheable=true) public static Object performAction()``` this method signature allows apex methods to  be used by the wire services. The Wire service is designed to provision a component with an immutable stream of data to a component that is ever updating. Because of this, the values returned by the Apex methods must be cached and so we must always annotate a method intended to be used by the Wire service with @AuraEnabled (cacheable = true) [resource](https://developer.salesforce.com/docs/componentlibrary/documentation/en/lwc/data_wire_service_about) & [more resources](https://developer.salesforce.com/docs/componentlibrary/documentation/en/lwc/lwc.apex_wire_method) 

	 - ```setStorable()``` must be used if we wish for an Apex action to be cached within an Aura component, however there is no such requirement when we are working with LWCs. [resource]( https://developer.salesforce.com/docs/atlas.en-us.224.0.lightning.meta/lightning/ref_jsapi_action_setStorable.htm)
</details>

