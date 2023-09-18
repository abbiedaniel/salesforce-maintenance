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
      - OWD (private, public read only, public read/write, controlled by parent)
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
  - Quick Actions & Buttons
- **Flexipage** 
  - Tab Order
  - Component Visibility
  - Components: Screen-flow, Chatter, Highlight Panel, List View, Path, Record Detail, Related Lists, Custom LWC, etc.
 
### Apex Data Types
- **Primitive Data Types**
  - String
  - Boolean
  - Integer
  - Double/Decimal
  - Id
      - Salesforce Id, string of 15-18 characters 
  - Date  
  - DateTime
  - Time  
   
## Tuesday - September 19, 2023
    
### Apex Triggers
- **Topic:**
- info
  - more info
    
### Apex Classes
- **Topic:**
  - info
    - more info
    

 
    
### Apex Logic
- **Topic:**
  - info
    - more info
    
### Object Oriented Concepts
- **Topic:**
  - info
    - more info
    
### Custom Metadata Types
- **Topic:**
  - info
    - more info
    
### Platform Events
- **Topic:**
  - info
    - more info
    
### Asynchronous Apex
- **Topic:**
  - info
    - more info
    
### Extending Declarative Functionality
- **Topic:**
  - info
    - more info
    
### Testing & Debugging
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
