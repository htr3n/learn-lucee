# Self-learning Cold Fusion / Lucee

* [Adobe ColdFusion](https://en.wikipedia.org/wiki/Adobe_ColdFusion): web-application development platform, created in 1995 by J. J. Allaire
* [Lucee](https://lucee.org) is an open-source [CFML](https://en.wikipedia.org/wiki/ColdFusion_Markup_Language) application server/engine

## CF Applications

* http://www.learncfinaweek.com/week1/Application_cfc
* https://helpx.adobe.com/coldfusion/developing-applications/user-guide.html
* https://modern-cfml.ortusbooks.com/rapid-app-development/applicationcfc

ColdFusion application is nothing more but a memory space reservation using the `this.name` property as the unique name for it.  It can contain the variables scopes like `application`, `session`, and `client` which are unique per this reservation name. This way two ColdFusion applications can have different persistence variable scopes and can even be embedded between each other:

```
webroot
├── Application.cfc   	// root public app
└── admin/
    └── Application.cfc	// embedded admin app
```

### Lucee `Application.cfc`

* https://docs.lucee.org/guides/cookbooks/application-context-basic.html
* https://rorylaitila.gitbooks.io/lucee/content/applications.html

The `Application.cfc` is a component you put in your web application that then is picked up by Lucee as part of the request. The `Application.cfc` is used to define context specific configurations/settings and event driven functions. Your website can have multiple `Application.cfc` files, every single file then defines an independent application context.

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

## CF Markup and Scripting Languages

- ColdFusion (CFML) is an interpreted and dynamic ECMA Script like language that compiles to Java Bytecode directly, thus running in the Java Virtual Machine (JVM) and in almost every operating system.

- ColdFusion is **case insensitive**

- ColdFusion includes pages (`.cfm`) or components / classes (`.cfc`)

- There are two ways to write ColdFusion:

    - Using **tags**
    - Using **script** syntax with [CFScript](https://cfdocs.org/script)

- Modern CFML advises that view or presentation layers will use the **tag** syntax in `cfm` files whilst the model or business layers will be in **script** syntax in `cfc` files

    - `cfm` - ColdFusion markup file, tag syntax is the default

        `cfc` - ColdFusion Component file (Class or Object), script syntax is the default. 

### Comments

- CFML: similar to HTML comments but with three dash `<!--- a comment --->`
- CFScript: similar to Java, JavaScript `//` for single line, `/* .. */` for a block

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

#### [writeDump](https://cfdocs.org/writedump)

Outputs the elements, variables and values of most kinds of CFML objects. Useful for debugging. You can display the contents of simple and complex variables, objects, components, user-defined functions, and other elements. Equivalent to the [cfdump](https://cfdocs.org/cfdump) tag, useful in [cfscript](https://cfdocs.org/cfscript).

```cfc
writeDump(server);
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

- Local (function-local, UDFs and CFCs only)
- Arguments
- Thread local (inside threads only)
- Query (not a true scope; variables in query loops)
- Thread
- Variables
- CGI
- Cffile
- URL
- Form
- Cookie
- Client

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

### Data Types

#### List

* https://helpx.adobe.com/coldfusion/cfml-reference/coldfusion-functions/functions-by-category/list-functions.html
* https://cfdocs.org/list-functions
* https://docs.lucee.org/categories/list.html

Lucee has lists which are strings with a delimiter (default is a comma). Lists can only contain strings (and they are a string themselves), so use an Array to contain complex data types. Lists are not real objects in Lucee, and are simply strings that contain a delimiter. As such, any string can be a list that has a delimiter other than a zero length string.

List functions are prefixed with `list` in their names to differentiate from string functions. 

##### Creating Lists

```cfml
<cfset newlist = "IL,MO,IA,MN">
<cfoutput>#newlist#</cfoutput>
```

##### List Modification

* [`listAppend()`](https://cfdocs.org/listappend)
* [`listGetAt()`](https://cfdocs.org/listgetat)
* [`listInsertAt()`](https://cfdocs.org/listinsertat)
* [`listDeleteAt()`](https://cfdocs.org/listdeleteat)
* [`listSort()`](https://cfdocs.org/listsort)
* [`listLen()`](https://cfdocs.org/listlen)
* [`listToArray()`](https://cfdocs.org/listtoarray)

##### Looping

```cfml
<cfscript>
myList = "one,two,three";
for(val in myList){
	echo(val & "<br />");
}
</cfscript>

<cfset myList = 'Jeff,John,Steve,Julliane' />
<cfloop list="#myList#" index="item">
	#item#<br />
</cfloop>
<cfloop from="1" to="#listlen(myList)#" index="i">
	#i#: #listGetAt(myList, i)#<br />
</cfloop>
```

#### Arrays

Lucee/CF array indexes **start at 1** and are **passed by reference**. If the elements in the array are Boolean, Numeric or Strings (simple values) they are passed by value. All other complex types are passed by reference.

* https://cfdocs.org/array-functions
* https://rorylaitila.gitbooks.io/lucee/content/arrays.html
* https://docs.lucee.org/categories/array.html
* https://helpx.adobe.com/au/coldfusion/developing-applications/the-cfml-programming-language/using-arrays-and-structures/basic-array-techniques.html

##### Creating Arrays

```cfml
<cfscript>
	threeElements = ["one","two","three"];
	emptyArray = [];
</cfscript>
```

##### Appending Elements

```cfml
<cfscript>
	exampleArray = [];
	exampleArray.append("one");
	exampleArray.append("two");
	exampleArray.append("three");
</cfscript>
```

##### Referencing (1-indexing)

```cfml
<cfscript>
	threeElements = ["one","two","three"];
	echo(threeElements[1]);
	echo(threeElements[3]);
	index = "1";
	echo (threeElements[index]);	// "one"
	echo (threeElements["#index#"]); // ""one
</cfscript>
```

##### Dumping

```cfml
<cfscript>
	threeElements = ["one","two","three"];
	dump(threeElements);
</cfscript>
```

##### Others

* [arrayLen(array)](https://cfdocs.org/arraylen)
* [len(Object)](https://cfdocs.org/len)  

##### Array Loop

CFScript

```cfc
myArray = ["a", "b", "c"];

// For Loop By index for CF <= 9.0
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

#### Structs

Structs in Lucee are containers for data where there is a 'key' that references a 'value'. 

* https://docs.lucee.org/categories/struct.html
* https://helpx.adobe.com/coldfusion/developing-applications/the-cfml-programming-language/using-arrays-and-structures/about-structures.html

##### Creating Structs

```cfml
<cfscript>
	emptyStruct = {};
	nonEmptyStruct = {foo:"one", bar:"two", baz:"three"};
</cfscript>
```

##### Nested Structs

```cfml
<cfscript>
nestedStruct = {foo:"one", 
            	bar:"two", 
            	baz:{
              		key:"value"
            	}
};
</cfscript>
```

##### Creating Ordered Structs

* Using [structNew()](https://cfdocs.org/structnew) with "ordered"

```cfml
<cfscript>
myStruct = structNew("ordered");
myStruct.insert("foo","one");
myStruct.insert("bar","two");
myStruct.insert("baz","three");
</cfscript>
```

##### Inserting Items

```cfml

<cfscript>
myStruct = {foo:"one", bar:"two", baz:"three"};
myStruct.insert("key", "value");
</cfscript>
```

##### Referencing Struct Keys

* Object.property

```cfml
<cfscript>
myStruct = {foo:"one", bar:"two", baz:"three"};
echo(myStruct.foo);
</cfscript>
```

* Dynamic referencing -- associative arrays (structs as arrays with string indexes)

```cfml
<cfscript>
myStruct = {foo:"one", bar:"two", baz:"three"};
myKey = "foo";
echo(myStruct[myKey]); //outputs "one"
</cfscript>
```

##### Looping Structs

```cfml
<cfscript>
myStruct = {foo:"one", bar:"two", baz:"three"};
for(key in myStruct){
  echo(key); //outputs "foo" then "bar" then "baz";
  echo(myStruct[key]); //outputs "one", then "two", then "three"
}

// By structEach() 
myStruct.each(function(key, value) { 
	writeOutput("<li>#key# : #value#</li>"); 
}); 
</cfscript>
```

#### Queries

* https://docs.lucee.org/categories/query.html
* https://helpx.adobe.com/coldfusion/cfml-reference/coldfusion-tags/tags-p-q/cfquery.html

##### Creating Queries

* [queryNew()](https://cfdocs.org/querynew) -- Creates a new query object. The query can be populated with data using functions [queryAddRow](https://cfdocs.org/queryaddrow), [querySetCell](https://cfdocs.org/querysetcell), or by passing it in to the rowData argument.

```cfml
<cfscript>
myQuery = queryNew(columns="col1,col2,col3", 
				   data=[
				   		["one","two","three"], 
				   		["foo","bar","baz"], 
				   		["i","am","query"]
				   ]);
dump(myQuery);
// query without data
myQuery = queryNew("col1,col2,col3");
dump(myQuery);
</cfscript>
```

##### Adding Rows

```cfml
<cfscript>
myQuery = queryNew("col1,col2,col3");
myQuery.addRow(["foo","bar","baz"]);
dump(myQuery);
</cfscript>
```

##### Adding Columns

```cfml
<cfscript>
myQuery = queryNew("col1,col2,col3");
myQuery.addColumn("col4", ["foo","bar","baz"]);
dump(myQuery);
</cfscript>
```

##### Retrieving Data by Column

```cfml
<cfscript>
myQuery = queryNew(columns="col1,col2,col3", 
				   data=[
				   		["one","two","three"], 
				   		["foo","bar","baz"], 
				   		["i","am","query"]
				   ]);
dump(myQuery.columnData("col1"));
</cfscript>
```

##### Query For Loop

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

#### Dates

* Lucee has a built in DateTime object

* [createDateTime()](https://cfdocs.org/createdatetime)
* https://docs.lucee.org/categories/datetime.html
* https://rorylaitila.gitbooks.io/lucee/content/dates.html

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

CFML

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

## Closures & Lambda

* https://rorylaitila.gitbooks.io/lucee/content/closures.html
* https://modern-cfml.ortusbooks.com/cfml-language/closures

### Closures

> A closure is the combination of a function and the lexical environment within which that function was declared.

#### Assigning / Creating Closures

```cfc
function hello(){
    var name = "luis";

    var display = function(){
        systemOutput( name );
    };

    display();
}

hello();
```

#### Returned Closures/High-Order Functions

```cfc
function makeAdder( required x ){
    return function( required y ){
        return x + y;
    };
}

add = makeAdder( 1 );
systemOutput( add( 2 ) );
```

#### Passing Closures

* like `map()`, `reduce()`, `filter()`, etc.

### Lambda Expressions (Lucee Only)

At the moment, only the Lucee CFML engine supports lambda expressions, which basically are a shorthand notation for defining closures.  

Lambda expressions reduce much of the syntax around creating closures. In its simplest form, you can eliminate the `function` keyword, curly braces and `return` statement. Lambda expressions implicitly return the results of the expression body.

```cfc
// Using a traditional closure
makeSix = function(){ return 5 + 1; }

// Using a lambda expression
makeSix = () => 5 + 1;

// returns 6
systemOutput( makeSix() );

// Takes two numeric values and adds them
add = ( numeric x, numeric y ) => x + y;

// returns 4
systemOutput( add( 1, 3 ) );
```

## Components and Functions

### Components (CFC)

* https://docs.lucee.org/categories/component.html
* https://helpx.adobe.com/coldfusion/developing-applications/building-blocks-of-coldfusion-applications/building-and-using-coldfusion-components.html
* https://helpx.adobe.com/coldfusion/cfml-reference/coldfusion-tags/tags-c/cfcomponent.html

CFML

* https://cfdocs.org/cfcomponent

```cfml
<cfcomponent>
```

CFScript

```cfc
component displayname="myComponent" {
   function hello() {}
}
```

#### Scopes

* `variables` -- private scope, visible internally to the CFC
* `this` -- public scope, visible from outside world
* `static`

#### Component Attributes

* `accessors`  -- enables automatic getters/setters
* `extends` -- inheritance
* `implements` -- implements interfaces
* `persistent` -- makes the object a Hibernate entity
* `serializable` 

#### Properties

* https://cfdocs.org/cfproperty

```cfc
property name="firstName" default="";
property name="lastName" default="";
property name="age" type="numeric" default="0";
property name="address" type="array";
```

### Functions

* https://docs.lucee.org/reference/tags/function.html
* https://cfdocs.org/cffunction
* A function can take ANY amount of arguments
* The default return type is `any` --> can return any type of variable
* The default visibility is `public`  (others are `private`, `package`, `remote`)
* Functions in CFML are objects themselves. They can be added, removed, renamed even at runtime as CFML is a dynamic language.

CFML

```cfml
<cffunction name="">
```

CFScript

```cfc
public boolean function myFunction(required any myArgument) { }
```

#### Function Attributes

* `output` 
* `description`
* `returnFormat`

#### Function Arguments

* A function can receive zero or more arguments separated by commas

```cfc
function(
 	required type name=default attribute=value,
 	required type name=default attribute=value){
}
```

#### Function Returns

* A function must use the `return` keyword to return a value
    * can be marked `void` to denote it does not return any value
    * if a function has no return type or `any` and the body does not explicitly return a value, the function will automatically return `null`

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

* Using the [`new` operator](https://cfdocs.org/new), the CFML engine will instantiate the class and call the default constructor (if it exists)

```cfml
<cfset bob = new Person()>
<cfscript>
	myObj = new ClassName();
</cfscript>
```

* [cfobject](https://cfdocs.org/cfobject)  -- Creates a CFML object, of a specified type.
* [createObject()](https://cfdocs.org/createobject) -- to instantiate CFC Components, but also Java, Com and SOAP WebService Objects. It also allows instantiating components with dynamic variable names.

```cfml
<cfset bob = createObject( "component", "Person" ).init()>
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
* https://modern-cfml.ortusbooks.com/cfml-language/queries

### Database Queries

A query is a request to a database. It returns a CFML `query` object containing a **recordset** and other metadata information about the query. The query can ask for information from the database, write new data to the database, update existing information in the database, or delete records from the database.

* Using the `cfquery` tag or function construct. (https://cfdocs.org/cfquery)

* Using the `queryExecute()` function. (https://cfdocs.org/queryexecute)

CFML

```cfml
<cfquery name = "qItems" datasource="pantry"> 
 SELECT QUANTITY, ITEM 
 FROM CUPBOARD 
 ORDER BY ITEM 
</cfquery> 
```

CFScript

```cfc
qItems = queryExecute( 
 "SELECT QUANTITY, ITEM FROM CUPBOARD ORDER BY ITEM"
);
```

#### Displaying Results

CFML

```cfml
<cfoutput query = "qItems">
There are #qItems.Quantity# #qItems.Item# in the pantry<br />
</cfoutput>
```

CFScript

```cfc
for( var row in qItems ){
 systemOutput( "There are #row.quantity# #row.item# in the pantry" );
}

qItems.each( function( row, index ){
 systemOutput( "There are #row.quantity# #row.item# in the pantry" );

} );

for( var i = 1; i lte qItems.recordCount; i++ ){
 systemOutput( "There are #qItems.quantity[ i ]# #qItems.item[ i ]# in the pantry" );
}
```

#### [Query Functions](https://cfdocs.org/query-functions)



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
* [Learn CF in a Week](http://learncfinaweek.com)
* [CFML Tracks](https://exercism.io/tracks/cfml)
* [Lucee Book](https://rorylaitila.gitbooks.io/lucee)
* [Learn Modern ColdFusion (CFML) in 100 Minutes](https://modern-cfml.ortusbooks.com)