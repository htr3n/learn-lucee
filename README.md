# Self-learning Cold Fusion / Lucee

* [Adobe ColdFusion](https://en.wikipedia.org/wiki/Adobe_ColdFusion): web-application development platform, created in 1995 by J. J. Allaire
* [Lucee](https://lucee.org) is an open-source [CFML](https://en.wikipedia.org/wiki/ColdFusion_Markup_Language) application server/engine

## CF Applications

* http://www.learncfinaweek.com/week1/Application_cfc
* https://helpx.adobe.com/coldfusion/developing-applications/user-guide.html

### Lucee `Application.cfc`

* https://docs.lucee.org/guides/cookbooks/application-context-basic.html
* https://rorylaitila.gitbooks.io/lucee/content/applications.html
* The `Application.cfc` is a component you put in your web application that then is picked up by Lucee as part of the request. The `Application.cfc` is used to define context specific configurations/settings and event driven functions. Your website can have multiple `Application.cfc` files, every single file then defines an independent application context.

* With the default setting Lucee always picks the "nearest" Application.cfc, so it searches from the current location down to the webroot.
* The Application.cfc supports [multiple event driven functions](https://docs.lucee.org/guides/cookbooks/application-context-basic.html)
    * [`onApplicationStart()`](https://cfdocs.org/onapplicationstart)
    * [`onApplicationEnd()`](https://cfdocs.org/onapplicationend)
    * [`onSessionStart()`](https://cfdocs.org/onsessionstart)
    * [`onSessionEnd()`](https://cfdocs.org/onsessionend)
    * [`onRequestStart()`](https://cfdocs.org/onrequeststart)
    * [`onRequestEnd()`](https://cfdocs.org/onrequestend)
    * [`onRequest()`](https://cfdocs.org/onrequest) -- If this function exists, Lucee only executes this function and no longer looks for the "targetPage" defined with the request. So let's say you have the call "/index.cfm", if there is an "/index.cfm" in the file system or not, does not matter, it is not executed anyway.
    * [`onError()`](https://cfdocs.org/onerror)

### Elements of a ColdFusion application

#### Reusable application elements:

* User-defined functions (UDFs)
* CFML custom tags
* ColdFusion components (CFCs)
* CFX (ColdFusion Extension) tags
* Pages that you include using the [`cfinclude`](https://cfdocs.org/cfinclude) tag

#### Shared variables

| Variable scope | Variables available                                    |
| -------------- | ------------------------------------------------------ |
| Server         | To all applications on a server and all clients        |
| Application    | To all pages in an application for all clients         |
| Client         | For a single client browser over multiple browser sessions in one application |
| Session        | For a single client browser for a single browser session in one application |

#### Application events and the [`Application.cfc`](https://cfdocs.org/application-cfc) file

| Event             | Trigger                                             |
| ----------------- | --------------------------------------------------- |
| Application start | ColdFusion starts processing the first request for a page in an application that is not running. |
| Application end   | An application time-out setting is reached or the server shuts down. |
| Session start     | A new session is created as a result of a request that is not in an existing session. |
| Session end       | A session time-out setting is reached.              |
| Request start     | ColdFusion receives a request, including HTTP requests, messages to the event gateway, SOAP requests, or Flash Remoting requests. |
| Request           | Immediately after ColdFusion finishes processing the request start event. The handler for this event is intended for use as a filter for the request contents. |
| Request end       | ColdFusion finishes processing all pages and CFCs for the request. |
| Exceptions        | An exception occurs that is not handled in a try/catch block. |

#### Other application-level settings and functions

If you do not have an `Application.cfc` file, CF processes the following two pages, if they are available, every time it processes any page in the application:

- The `Application.cfm` page is processed before each page in the application.
- The `OnRequestEnd.cfm` page is processed after each page in the application.

## ColdFusion Markup and Scripting Languages

- https://helpx.adobe.com/coldfusion/developing-applications/the-cfml-programming-language.html
- ColdFusion is **not case sensitive**
- ColdFusion includes [CFML](https://en.wikipedia.org/wiki/ColdFusion_Markup_Language) (tags) and [CFScript](https://en.wikipedia.org/wiki/CFScript) `<cfscript>...</cfscript>`

### Comments

- Similar to HTML comments but with three dash `<!--- a comment --->`

### Components

ColdFusion components encapsulate multiple, related, functions. A ColdFusion component is essentially a set of related user-defined functions and variables, with additional functionality to provide and control access to the component contents. ColdFusion components can make their data private, so that it is available to all functions (also called methods) in the component, but not to any application that uses the component.

ColdFusion components have the following features:

- They are designed to provide related services in a single unit.
- They can provide web services and make them available over the Internet.
- They have several features that are familiar to object-oriented programmers, including data hiding, inheritance, packages, and introspection.

CFML

* [`<cfcomponent>`](https://cfdocs.org/cfcomponent)

CFScript

```cfc
component {
   function hello() {}
}
```

### Data Types

ColdFusion is considered *typeless* because you do not explicitly specify variable *data types*. 
However, ColdFusion data, the constants and the data that variables represent, *do* have data types, which correspond to the ways the data is stored on the computer.

| Category | Description and types                                        |
| :------- | :----------------------------------------------------------- |
| Simple   | Represents one value. You can use simple data types directly in ColdFusion expressions. ColdFusion simple data types are:<br />+ strings A sequence of alphanumeric characters enclosed in single or double quotation marks, such as "This is a test."<br />+ integers A sequence of numbers written without quotation marks, such as 356.<br />+ real numbers, such as -3.14159<br />+ Boolean values Use `True`, `Yes`, or `1` for true and `False`, `No`, or `0` for false. Boolean values are not case sensitive.<br />+ date-time values ColdFusion supports a variety of data formats. |
| Complex  | A container for data. Complex variables generally represent more than one value. ColdFusion built-in complex data types are: *arrays, structures, queries* |
| Binary   | Raw data, such as the contents of a GIF file or an executable program file |
| Object   | COM, CORBA, Java, web services, and ColdFusion Component objects: Complex objects that you create and access using the cfobject tag and other specialized tags. |

> ColdFusion does not have a data type for unlimited precision decimal numbers, but it can represent such numbers as strings and provides a function that supports unlimited precision decimal arithmetic.

### Variables

*Variables* are the most frequently used operands in ColdFusion expressions. Variable values can be set and reset, and can be passed as attributes to CFML tags. Variables can be passed as parameters to functions, and can replace most constants. 

#### [cfset](https://cfdocs.org/cfset)

```cfml
<cfset str = "Hello">
<cfset temp = 5>
```

#### [cfoutput](https://cfdocs.org/cfoutput)

* To convert the variable name into its value is to place pound signs around the variable name. The variable name and pound signs must be placed inside of a cfoutput tag.

```cfml
<cfoutput>#str# CF World!</cfoutput>
```

* Can also perform calculations within the pound signs

```cfml
<cfoutput>#3 + 5#</cfoutput>
```

#### [cfdump](https://cfdocs.org/cfdump)

Outputs the contents of a variable of any type for debugging purposes. The variable can be as simple as a string or as complex as a cfc component instance.

CFML

```cfml
<cfdump var="#server#" label="Server Scope">
```

CFScript

```cfc
writeDump(var = server, label = "Server Scope");
```

##### Lucee [dump()](https://docs.lucee.org/reference/functions/dump.html)

```
dump(var = server, label = "Server Scope");
```

#### [evaluate()](https://cfdocs.org/evaluate)

* The `evaluate()` function shall evaluate a string expression at runtime

```cfml
<!--- Evaluate the expression ---> 
 <cfset first = "ColdFusion"> 
 <cfset second = "ColdFusion"> 
 <cfset op = "eq"> 
 <cfoutput>#evaluate("first #op# second")#</cfoutput> 
```

* Since CF 6.1, highly recommend to use bracket notation instead

```cfml
<cfset queryname[columnname][rownumber] />
<cfset x = "foo" />
<cfoutput>#variables['x']#</cfoutput>
```

#### Variable Lookup

- Local variables
- CGI variables
- File variables
- URL variables
- Form variables
- Cookie variables
- Client variables

#### Session Variables

* A session variable is used to allow variables to live beyond the life of the page in which they are created in but will die when the users session dies.

* On every page of your application where you are going to use the session variable you need to include:

```cfml
<cfapplication sessionmanagement="true">
```

* This code should normally be placed in your `Application.cfm` file.

* To set a session variable:

```cfml
<cfset session.name = "UserSessionVariable">
```

* To get value on a different page called after that

```cfml
<cfoutput>#session.name#</cfoutput>
```

### Data Structures

#### List

* A list is a string separated by a delimiter
* The default delimiter is a comma

* https://helpx.adobe.com/coldfusion/cfml-reference/coldfusion-functions/functions-by-category/list-functions.html
* https://cfdocs.org/list-functions

```cfml
<cfset newlist = "IL,MO,IA,MN">
<cfoutput>#newlist#</cfoutput>
```

##### List Modification

* [listAppend()](https://cfdocs.org/listappend)
* [listGetAt()](https://cfdocs.org/listgetat)
* [listInsertAt()](https://cfdocs.org/listinsertat)
* [listDeleteAt()](https://cfdocs.org/listdeleteat)
* [listSort()](https://cfdocs.org/listsort)
* [listLen()](https://cfdocs.org/listlen)

#### Array

Lucee Array indexes start at 1 and are passed by reference. If the elements in the array are Boolean, Numeric or Strings (simple values) they are passed by value. All other complex types are passed by reference.

* https://cfdocs.org/array-functions
* https://rorylaitila.gitbooks.io/lucee/content/arrays.html
* https://helpx.adobe.com/au/coldfusion/developing-applications/the-cfml-programming-language/using-arrays-and-structures/basic-array-techniques.html

### Flow Control Structures

#### If-ElseIf-Else

* [cfif](https://cfdocs.org/cfif), [cfelseif](https://cfdocs.org/cfelseif), [cfelse](https://cfdocs.org/cfelse)

CFML

```cfml
<cfset count = 10> 
<cfif count GT 20> 
	<cfoutput>#count#</cfoutput> 
<cfelseif count EQ 8> 
	<cfoutput>#count#</cfoutput> 
<cfelse> 
	<cfoutput>#count#</cfoutput> 
</cfif>
```

CFScript

```cfc
count = 10; 
 if (count > 20) { 
 	writeOutput(count); 
 } else if (count == 8) { 
 	writeOutput(count); 
 } else { 
 	writeOutput(count); 
 }
```

#### Switch-Case

* [cfswitch](https://cfdocs.org/cfswitch), [cfcase](https://cfdocs.org/cfcase), [cfdefaultcase](https://cfdocs.org/cfdefaultcase)

CFML

```cfml
<cfset fruit = ""> 
<cfswitch expression="#fruit#"> 
    <cfcase value="Apple">I like apples!</cfcase>
    <cfcase value="Orange,Citrus">I like oranges!</cfcase> 
    <cfcase value="Kiwi">I like kiwi!</cfcase>
    <cfdefaultcase>Fruit, what fruit?</cfdefaultcase> 
</cfswitch>
```

CFScript

```cfc
fruit = "Orange";
switch(fruit) {
    case "Apple":
        writeOutput("I like apples!");
        break; 
    case "Orange": case "Citrus":
         writeOutput("I like oranges!");
        break; 
    case "Kiwi":
        writeOutput("I like kiwi!"); 
        break; 
    default: 
        writeOutput("Fruit, what fruit?"); 
        break; 
 }
```

#### For Loops

* [cfloop](https://cfdocs.org/cfloop)

CFScript

```
for (i = 1; i <= 10; i++) {
    writeOutput(i); 
}
```

CFML

```
<cfloop index="i" from="1" to="10">
    <cfoutput>#i#</cfoutput>
</cfloop>
```

##### Array Loop

CFScript

```cfc
myArray = ["a", "b", "c"];

// For Loop By index for CF9.0.0 and lower 
for (i = 1; i <= arrayLen(myArray); i++) {
    writeOutput(myArray[i]);
}

// For In CF9.0.1+ 
for (currentIndex in myArray) {
    writeOutput(currentIndex);
}

// arrayEach() member function CF11+
myArray.each(function(element, index) {
    writeOutput(element & " : " & index);
});
```

CFML

```cfml
<cfset myArray = ["a", "b", "c"]> 

<!--- By index ---> 
<cfloop index="i" from="1" to="#arrayLen(myArray)#"> 
	<cfoutput>#myArray[i]#</cfoutput> 
</cfloop> 

<!--- By array ---> 
<cfloop array="#myArray#" index="currentIndex"> 
	<cfoutput>#currentIndex#</cfoutput> 
</cfloop>
```

##### List Loop

```cfml
<!--- List ---> 
<cfset myList = 'Jeff,John,Steve,Julliane' />
<cfloop list="#myList#" index="item">
	#item#<br />
</cfloop>
<cfloop from="1" to="#listlen(myList)#" index="i">
	#i#: #listGetAt(myList, i)#<br />
</cfloop>
```

##### Struct Loop

```cfc
myStruct = {name: "Tony", state: "Florida"}; 
// By struct 
for (key in myStruct) { 
 	writeOutput("<li>#key# : #myStruct[key]#</li>"); 
} 

// By structEach() 
myStruct.each(function(key, value) { 
	writeOutput("<li>#key# : #value#</li>"); 
}); 
```

##### Query Loop

```cfml
<cfscript>
	myQuery = queryNew("id,user");
	queryAddRow(myQuery);
	querySetCell(myQuery, 'id', '1');
	querySetCell(myQuery, 'user', 'Jeff');
	queryAddRow(myQuery);
	querySetCell(myQuery, 'id', '2');
	querySetCell(myQuery, 'user', 'John');
	queryAddRow(myQuery);
	querySetCell(myQuery, 'id', '3');
	querySetCell(myQuery, 'user', 'Steve');
</cfscript>

<cfloop query="myQuery">
	#myQuery.id# #myQuery.user#<br />
</cfloop>
```

CFScript

```cfc
// CF < 9.0
for ( i=1;i<=myQuery.recordCount;i++ ) {
	writeOutput('#myQuery.id#: #myQuery.user[i]#<br />');
}
// (CF10+)
for ( row in myQuery ) {
	writeOutput('#row.id#: #row.user#');
}
```

#### [While Loop](https://docs.lucee.org/reference/tags/while.html) (in Lucee)

* [`cfwhile`](https://cfdocs.org/cfwhile)

CFML

```cfml
<cfwhile [condition=boolean] [label=string]>
	<!--- body --->
</cfwhile>
```

CFScript

```cfc
  while(condition) { }
```

#### Continue / Break / Exit

* [`cfcontinue`](https://cfdocs.org/cfcontinue)
* [`cfbreak`](https://cfdocs.org/cfbreak)
* [`cfabort`](https://cfdocs.org/cfabort): stops processing of the current page at the location of the `cfabort` tag.
* [`cfexit`](https://cfdocs.org/cfexit): controls the processing of a custom tag, and can only be used in ColdFusion custom tags.

#### Exception

* [`cftry`](https://cfdocs.org/cftry)
* [`cfcatch`](https://cfdocs.org/cfcatch)

* [`cffinally`](https://cfdocs.org/cffinally)

Tag

```cfml
<cftry>
	hello world<br/>
	<cfthrow message="threw on purpose!" />
	<cfcatch type="any">
		Caught an exception<br/>
	</cfcatch>
	<cffinally>
		Ran clean-up code regardless of error
	</cffinally>
</cftry>
```

CFScript

```cfc
try {
	x = 5/0;
}
catch (any e) {
	writeOutput("Error: " & e.message);
}
```

## Object-Oriented Programming

* http://www.learncfinaweek.com/week1/OOP
* https://rorylaitila.gitbooks.io/lucee/content/classes.html

### Classes

In Lucee, classes are components and stored in files `.cfc`. Lucee supports OOP concepts like interfaces, inheritance, public and private methods.

```cfc
component {...}
```

#### Constructors

The constructor method ~ function `init()`. There can be **only one constructor**, Lucee **does not support method overloading**.

```cfc
component {
  function init(){
    return this;
  }
}
```

#### Methods

Components can define additional methods and they can have the following access levels:

- Public - Accessible to all callers
- Private - Accessible to only callers within this class
- Package - Accessible to other Components in a package
- Remote - Accessible to remote callers via web services, REST or invoking components from HTTP

```
[access] [return type] function [function name](){
}
```

#### Method arguments

In Lucee, method arguments can be optionally typed, optionally required, and have default values.

```cfc
component {
	// typed arguments
	public function myFunc(string argumentName1, struct argumentName2){...}
	// required arguments
 	public function myFunc(required argumentName1, required argumentName2){...}
 	// required typed arguments
 	public function myFunc(required string argumentName1, required struct argumentName2){...}
 	// default arguments
 	public function myFunc(required string argumentName1="test", required struct argumentName2={key:"value"}){}
}
```

#### Instantiation

* Using the [`new` operator](https://cfdocs.org/new), Lucee will instantiate the class and call the default constructor (if it exists)

```cfml
<cfscript>
	myObj = new default_constructor();
</cfscript>
```

* [cfobject](https://cfdocs.org/cfobject) 
* [createObject()](https://cfdocs.org/createobject) -- to instantiate CFC Components, but also Java, Com and SOAP WebService Objects. It also allows instantiating components with dynamic variable names.

```cfml
<cfscript>
tellTimeCFC=createObject("component","appResources.components.tellTime"); 
	tellTimeCFC.getLocalTime();
</cfscript>
```

SOAP WS

```cfml
<cfscript> 
	ws = createObject("webservice", "http://www.xmethods.net/sd/2001/TemperatureService.wsdl"); 
	xlatstring = ws.getTemp(zipcode = "55987"); 
	writeOutput("The temperature at 55987 is " & xlatstring); 
</cfscript>
```

* [entifyNew()](https://cfdocs.org/entitynew)

#### Component Lookup Rules

When instantiating components they are found in the following ways

- Any Components that are in the same directory as the script instantiating the component
- Any Components imported into the script using the import keyword
- Any Components pathed to using dotted notation, starting from the webroot
- Any Components pathed to using dotted notation, that can be found from a mapping in `Application.cfc`

### Interfaces

Lucee supports Interfaces which Components can implment multiple interfaces. Combined with the use of Mixins, Lucee Interfaces can help type check and document concise code.

E.g. `IMyInterface.cfc`

```cfc
interface {
  public function myFunc(required string myString){ }
  public function myOtherFunc(required array myArray){ }
}
```

### Implementing an interface

```cfc
component implements="IMyInterface" {
  public function myFunc(required string myString){
    return arguments.myString;
  }  
  public function myOtherFunc(required array myArray){
    return myArray;
  } 
}
```

### Interface for Type Checking

```cfc
component {
	public function init(required IMyInterface object){
		//Will error if the object passed in did not implement IMyInterface
	}
}
```

## Databases

* http://www.learncfinaweek.com/week1/Databases
* http://www.learncfinaweek.com/week1/Intro_to_ORM

## Testing

* [TestBox](https://www.ortussolutions.com/products/testbox)

## [CFWheels - MVC](https://cfwheels.org/)

- https://guides.cfwheels.org/docs
- Controllers  (`controllers`)
    - `Controllers.cfc`  --> root controller
    - New controller --> `component extends = "Controller" {...}`
- Views (`views`)
- Models (`models`)
- Routes
    - root --> `/` --> Controller = wheels, Action = wheels
    - `/[controller]` --> action `index`
    - `/[controller]/[action]`  
        - e.g.   `/say/hello`  --> `controllers/Say.cfc`  + `function hello() {}`  + `views/say/hello.cfm`

## [CommandBox](https://www.ortussolutions.com/products/commandbox)

* Operation System integration for executing commands
* Ability to create and execute commands built using ColdFusion (CFML)
* [ForgeBox](https://www.coldbox.org/forgebox) integration for cloud package management and installations
* ColdBox Platform, TestBox, and CommandBox CMS Integrations
* Can spin up an ad hoc, lightweight, ColdFusion server (Lucee or Adobe ColdFusion) in any directory from the command line
    * change the working directory to the root of the app
    * type `server start` / `server stop`
* Extensible via ColdFusion (CFML)
    * CommandBox is small, lightweight, integrated at the operating system level, but actually running on CFML powered by [WireBox](https://www.ortussolutions.com/products/coldbox).

## References

* https://cfdocs.org
* https://cfwheels.org
* http://learncfinaweek.com
* https://exercism.io/tracks/cfml
* https://helpx.adobe.com/coldfusion/developing-applications/user-guide.html