## User Interface - 25% / 15 Questions
##### 🕴️ Given a scenario, display content or modify Salesforce data using a Visualforce page and the appropriate controllers or extensions as needed.
##### 🕴️ Describe the Lightning Component framework, its benefits, and the types of content that can be contained in a Lightning web component.
##### 🕴️ Given a scenario, prevent user interface and data access security vulnerabilities.
##### 🕴️ Given a scenario, display and use a custom user interface components, including Lightning Components, Flow, and Visualforce.
##### 🕴️ Describe the use cases and best practices for Lightning Web Component events.
##### 🕴️ Given a scenario, implement Apex to work with various types of page components, including Lightning Components, Flow, Next Best Actions, etc.
#
<br>

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
  
  	- **What is it used for:** Used to create custom UI using built in standard controllers, apex, components, HTML and optional styling elements. Visualforce can integrate with standard web technology or javascript framework
  	- **How it works under the hood:** Each page is accessible via URL. User accesses the url and server performs automatic data processing required for the page, renders the page into HTML, and returns the result to the browser to display.
  	- **Pages and Components:** Page files end in ```.vfp``` and ```apex:page``` header is required. Component files ends in ```.vfc``` and ```apex:component``` header is required.
  	- **Override buttons:** You can override standard buttons by **enabling override option in object setup** on the button. Visualforce overrides are supported for new, edit, view, tab, list and clone actions in Lightning console apps. Does not support delete and custom actions.
  
- **Ways to Access Visualforce**
	- Open a visualforce page from the App Launcher under All Items
 	- Add visualforce page to the navigation bar as a tab
  	- Display Visualfroce page within a standard page layout
  	- Add a visualforce page as a component in a custom app page in the Lightning App Builder
  	- Launch a visualforce page as a Quick Action on a page layout
  	- Display visualforce page by overriding standard buttons or links. Override option must be enabled for the button in the object setup.
  	- Display a visualforce page using custom buttons or links

- **Use Cases**
	- Build wizards and other multi-step processes
 	- Provide low-code solution to non-developers or junior developers
  	- To create custom flow control through an application
  	- Define navigation patterns and data-specific rules for optimal, efficient application interaction
  	- Can add visualforce pages in lightning app builder/flexipage   

- **Visualforce Glossary**
	- [Visualforce Functions](https://developer.salesforce.com/docs/atlas.en-us.224.0.pages.meta/pages/pages_variables_functions.htm?_ga=2.66294572.303706887.1705500956-417190799.1635779157)
 	- [Visualforce Standard Components](https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/pages_compref.htm?_ga=2.125023880.303706887.1705500956-417190799.1635779157)
  	- [Visualforce Global Variables](https://developer.salesforce.com/docs/atlas.en-us.224.0.pages.meta/pages/pages_variables_global.htm?_ga=2.58251944.303706887.1705500956-417190799.1635779157)

- **Standard Controller Capabilities**
	-  Easy data access
	- Provides a set of standard actions, such as create edit save and delete that can be added to a page through buttons and links
	- Embed visualforce page within an object's page layout, or use object-speicifc custom action, or use mobile cards in a Salesforce app

  	
- **Visualforce Page with Standard Controller Example**
  ```html
  <apex:page standardController = "Object" lightningStylesheets = "true">
  // allows access to standard controller methods and fields
  // lightningStylesheets renders page similar to lightning experience display, without it, it looks like salesforce classic

  <script> some javascript method </script>
  // runs javascript in visualforce page
  <apex:includeScript value="{!Resource.javascriptLibrary}"/>
  // upload javacsipt as a static resource  to run in visualforce page

  <apex:styleSheet value="{!URLFOR($Resrouce.StyleResource, 'filename.css')}"/>
  // import css stylesheet as a static resource
        
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

  - Standard List Controllers support record pagination, rendering dynamic number of records to the page, an using existing list view filters.
    
```html
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
	```html
	<apex:component>
		<apex:attribute name = "User's Name" type = "String" description = "The name of the user we are welcoming." />
		Welcome {!name} to Salesforce CRM!
	</apex:component>
	```
    
 	- To use in a visualforce controller
    	```html
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
   
	```html
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
```html
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

```html
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
 
   	```html
    	<apex:page> standardController = "Account" extensions = "AccountControllerExtension, AnotherControllerExtension" recordSetVar = "acounts"/>
    	// the first extension class in the list overrides other extension method's with the same name
    
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
  	<schema> <event> </event> </schema> //use in meta-xml file to expose a custom event of a LWC to another LWC within the page

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
  	- Types of Events:
  		- Component Event: supports two types of propagation - bubble and capture. Default phase is the bubble phase, where the source component gets to handle the event first when the event is fired. Next it's parent component, then grand parent component and so on until the root component. If a component uses the caoture phasem the order of event progragation behaves in a top-down manner. When the source component fires the event, the root component gets the handle the event first and the propogatuion traverse down the containment hierarchy until it reaches the source component.
  	 	- Application Event: broadcast events to alll components in an application, which could adversely affect performance.  



- **Aura Enabled Important Methods & Signatures**
  
	- ```@AuraEnabled(cacheable=true)``` improves the runtime performance of Lightning Components on the Aura Enabled apex methods that are frequently used in multiple LWCs by caching the result on the client side [resource](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/controllers_server_apex_auraenabled_annotation.htm)
   
	 - ```Security.stripInaccessible(AccessType, sourceRecords)``` enforces the FLS of the current user by stripping anything which is not accessible in the defined context in @AuraEnabled methods [resource](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_classes_with_security_stripInaccessible.htm) 

	- ``` @AuraEnabled(cacheable=true) public static Object performAction()``` this method signature allows apex methods to  be used by the wire services. The Wire service is designed to provision a component with an immutable stream of data to a component that is ever updating. Because of this, the values returned by the Apex methods must be cached and so we must always annotate a method intended to be used by the Wire service with @AuraEnabled (cacheable = true) [resource](https://developer.salesforce.com/docs/componentlibrary/documentation/en/lwc/data_wire_service_about) & [more resources](https://developer.salesforce.com/docs/componentlibrary/documentation/en/lwc/lwc.apex_wire_method) 

	 - ```setStorable()``` must be used if we wish for an Apex action to be cached within an Aura component, however there is no such requirement when we are working with LWCs. [resource]( https://developer.salesforce.com/docs/atlas.en-us.224.0.lightning.meta/lightning/ref_jsapi_action_setStorable.htm)
</details>


