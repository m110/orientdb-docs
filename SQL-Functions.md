# SQL - Functions

SQL Functions are all the functions bundled with OrientDB [SQL engine](SQL.md). You can create your own [Database Functions](Functions.md) in any language supported by JVM. Look also to [SQL Methods](SQL-Methods.md).

SQL Functions can work in 2 ways based on the fact that receive 1 or more parameters:

## Aggregated mode

When only one parameter is passed. They aggregate the result in only one record. The classic example is the sum():
```sql
select sum(salary) from employee
```
This will always return 1 record with the sum of salary field.

## Inline mode

When two or more parameters are passed:
```sql
select sum(salary, extra, benefits) as total from employee
```
This will return the sum of the field "salary", "extra" and "benefits" as "total". In case you need to use a function as inline when you've only one parameter, then add a second one like "null":
```sql
SELECT first( out('friends').name, null ) as firstFriend FROM Profiles
```
In this case `first()` function doesn't aggregate everything in only one record, but returns one record per `Profile` where the `firstFriend` is the first item of the collection received as parameter.

## Bundled functions

### Functions by category



| Graph | Math  | Collections | Misc  |
|-------|-------|-------------|-------|
|[out()](SQL-Functions.md#out)    | [eval()](SQL-Functions.md#eval) | [set()](SQL-Functions.md#set)             | [date()](SQL-Functions.md#date)
|[in()](SQL-Functions.md#in)      | [min()](SQL-Functions.md#min) | [map()](SQL-Functions.md#map)               | [sysdate()](SQL-Functions.md#sysdate)
|[both()](SQL-Functions.md#both)  | [max()](SQL-Functions.md#max) | [list()](SQL-Functions.md#list)             | [format()](SQL-Functions.md#format)
|[outE()](SQL-Functions.md#outE)  | [sum()](SQL-Functions.md#sum) | [difference()](SQL-Functions.md#difference) | [distance()](SQL-Functions.md#distance)
|[inE()](SQL-Functions.md#inE)    |                               | [first()](SQL-Functions.md#first)           | [ifnull()](SQL-Functions.md#ifnull)
|[bothE()](SQL-Functions.md#bothE)|                               | [intersect()](SQL-Functions.md#intersect)   | [coalescence()](SQL-Functions.md#coalescence)
|[outV()](SQL-Functions.md#outV)  | [avg()](SQL-Functions.md#avg) | [distinct()](SQL-Functions.md#distinct)     | [uuid()](SQL-Functions.md#uuid)|
|[inV()](SQL-Functions.md#inV)    | [count()](SQL-Functions.md#count) | [expand()](SQL-Functions.md#expand)|  [if()](SQL-Functions.md#if)
|[traversedElement()](SQL-Functions.md#traversedelement) | [mode()](SQL-Functions.md#mode)                        | [unionall()](SQL-Functions.md#unionall)|
|[traversedVertex()](SQL-Functions.md#traversedvertex) | [median()](SQL-Functions.md#median)                      | [flatten()](SQL-Functions.md#flatten)|
|[traversedEdge()](SQL-Functions.md#traversededge) | [percentile()](SQL-Functions.md#percentile)                  | [last()](SQL-Functions.md#last)|
|[shortestPath()](SQL-Functions.md#shortestpath) | [variance()](SQL-Functions.md#variance)| |
|[dijkstra()](SQL-Functions.md#dijkstra) | [stddev()](SQL-Functions.md#stddev)| |

### Functions by name

|       |       |       |       |
|-------|-------|-------|-------|
|[avg()](SQL-Functions.md#avg) | [both()](SQL-Functions.md#both) | [bothE()](SQL-Functions.md#bothE) | [coalescence()](SQL-Functions.md#coalescence) | 
|[count()](SQL-Functions.md#count)|[date()](SQL-Functions.md#date) | [difference()](SQL-Functions.md#difference) | [dijkstra()](SQL-Functions.md#dijkstra) |
|[distance()](SQL-Functions.md#distance) | [distinct()](SQL-Functions.md#distinct) | [eval()](SQL-Functions.md#eval) | [expand()](SQL-Functions.md#expand) |
|[format()](SQL-Functions.md#format) | [first()](SQL-Functions.md#first) | [flatten()](SQL-Functions.md#flatten) | [if()](SQL-Functions.md#if) | |
[ifnull()](SQL-Functions.md#ifnull) |[in()](SQL-Functions.md#in) | [inE()](SQL-Functions.md#inE) | [inV()](SQL-Functions.md#inV) | 
| [intersect()](SQL-Functions.md#intersect) |[list()](SQL-Functions.md#list) | [map()](SQL-Functions.md#map) | [min()](SQL-Functions.md#min) | 
| [max()](SQL-Functions.md#max) | [median()](SQL-Functions.md#median) | [mode()](SQL-Functions.md#mode) | [out()](SQL-Functions.md#out) |
| [outE()](SQL-Functions.md#outE) | [outV()](SQL-Functions.md#outV) | [percentile()](SQL-Functions.md#percentile) | [set()](SQL-Functions.md#set) | 
| [shortestPath()](SQL-Functions.md#shortestpath) |[stddev()](SQL-Functions.md#stddev)|[sum()](SQL-Functions.md#sum)|[sysdate()](SQL-Functions.md#sysdate)|
| [traversedElement()](SQL-Functions.md#traversedelement) | [traversedEdge()](SQL-Functions.md#traversededge) | [traversedVertex()](SQL-Functions.md#traversedvertex) | [unionall()](SQL-Functions.md#unionall) | 
| [uuid()](SQL-Functions.md#uuid)| [variance()](SQL-Functions.md#variance) |

### out()

Get the adjacent outgoing vertices starting from the current record as Vertex.

Syntax: ```out([<label-1>][,<label-n>]*)```

Available since: 1.4.0

#### Example

Get all the outgoing vertices from all the Vehicle vertices:
```sql
SELECT out() from V
```

Get all the incoming vertices connected with edges with label (class) "Eats" and "Favorited" from all the Restaurant vertices in Rome:
```sql
SELECT out('Eats','Favorited') from Restaurant where city = 'Rome'
```
---
### in()

Get the adjacent incoming vertices starting from the current record as Vertex.

Syntax: ```in([<label-1>][,<label-n>]*)```

Available since: 1.4.0

#### Example

Get all the incoming vertices from all the Vehicle vertices:

```sql
SELECT in() from V
```

Get all the incoming vertices connected with edges with label (class) "Friend" and "Brother":
```sql
SELECT in('Friend','Brother') from V
```
---
### both()

Get the adjacent outgoing and incoming vertices starting from the current record as Vertex.

Syntax: ```both([<label1>][,<label-n>]*)```

Available since: 1.4.0

#### Example

Get all the incoming and outgoing vertices from vertex with rid #13:33:

```sql
SELECT both() from #13:33
```

Get all the incoming and outgoing vertices connected with edges with label (class) "Friend" and "Brother":

```sql
SELECT both('Friend','Brother') from V
```
---
### outE()

Get the adjacent outgoing edges starting from the current record as Vertex.

Syntax: ```outE([<label1>][,<label-n>]*)```

Available since: 1.4.0

#### Example

Get all the outgoing edges from all the vertices:
```sql
SELECT outE() from V
```

Get all the outgoing edges of type "Eats" from all the SocialNetworkProfile vertices:
```sql
SELECT outE('Eats') from SocialNetworkProfile
```
---
### inE()

Get the adjacent incoming edges starting from the current record as Vertex.

Syntax: ```inE([<label1>][,<label-n>]*)```

#### Example

Get all the incoming edges from all the vertices:

```sql
SELECT inE() from V
```

Get all the incoming edges of type "Eats" from the Restaurant 'Bella Napoli':
```sql
SELECT inE('Eats') from Restaurant where name = 'Bella Napoli'
```
---
### bothE()

Get the adjacent outgoing and incoming edges starting from the current record as Vertex.

Syntax: ```bothE([<label1>][,<label-n>]*)```

Available since: 1.4.0

#### Example
Get both incoming and outgoing edges from all the vertices:
```sql
SELECT bothE() from V
```

Get all the incoming and outgoing edges of type "Friend" from the Profile with nick 'Jay'

```sql
SELECT bothE('Friend') from Profile where nick = 'Jay'
```

### outV()

Get outgoing vertices starting from the current record as Edge.

Syntax: ```outV()```

Available since: 1.4.0

#### Example

```sql
SELECT outV() from E
```

### inV()

Get incoming vertices starting from the current record as Edge.

Syntax: ```inV()```

Available since: 1.4.0

#### Example

```sql
SELECT inV() from E
```

### eval()

Syntax: ```eval('<expression>')```

Evaluates the expression between quotes (or double quotes).

Available since: 1.4.0

#### Example

```sql
SELECT eval('price * 120 / 100 - discount') as finalPrice from Order
```

### coalesce()

Returns the first field/value not null parameter. If no field/value is not null, returns null.

Syntax: ```coalesce(&lt;field&#124;value&gt;)```

Available since: 1.3.0

#### Example

```sql
SELECT coalesce(amount, amount2, amount3) from Account
```

### if()

Syntax: ```if(<expression>, <result-if-true>, <result-if-false>) ```

Evaluates a condition (first parameters) and returns the second parameter if the condition is true, the third one otherwise

#### Example: 
```
select if(eval("name = 'John'"), "My name is John", "My name is not John") from Person
```


### ifnull()

Returns the passed field/value (or optional parameter return_value_if_not_null). If field/value is not null, otherwise it returns return_value_if_null.

Syntax: ```ifnull(&lt;field&#124;value&gt;, &lt;return_value_if_null&gt; [,&lt;return_value_if_not_null&gt;](,&lt;field&.md#124;value&gt;]*)```

Available since: 1.3.0

#### Example

```sql
SELECT ifnull(salary, 0) from Account
```

---
### expand()

Expands the collection in the field <field> and use it as result.

Available since: 1.4.0

Syntax: ```expand(<field>)```

#### Example

```sql
select expand( addresses ) from Account. 
```
This replaces the flatten() now deprecated

---
### flatten()

_Deprecated, use the EXPAND() instead_.

Extracts the collection in the field <field> and use it as result.

Syntax: ```flatten(<field>)```

Available since: 1.0rc1

#### Example

```sql
select flatten( addresses ) from Account
```
---
### first()

Retrieves only the first item of multi-value fields (arrays, collections and maps). For non multi-value types just returns the value.

Syntax: ```first(<field>)```

Available since: 1.2.0

#### Example

```sql
select first( addresses ) from Account
```
---
### last()

Retrieves only the last item of multi-value fields (arrays, collections and maps). For non multi-value types just returns the value.

Syntax: ```last(<field>)```

Available since: 1.2.0

#### Example

```sql
select last( addresses ) from Account
```
---
### count()

Counts the records that match the query condition. If \* is not used as a field, then the record will be counted only if the field content is not null.

Syntax: ```count(<field>)```

Available since: 0.9.25

#### Example

```sql
select count(*) from Account
```
---
### min()

Returns the minimum value. If invoked with more than one parameters, the function doesn't aggregate, but returns the minimum value between all the arguments.

Syntax: ```min(<field> [, <field-n>]* )```

Available since: 0.9.25

#### Example

Returns the minimum salary of all the Account records:
```sql
select min(salary) from Account
```
Returns the minimum value between 'salary1', 'salary2' and 'salary3' fields.
```sql
select min(salary1, salary2, salary3) from Account
```
---
### max()

Returns the maximum value. If invoked with more than one parameters, the function doesn't aggregate, but returns the maximum value between all the arguments.

Syntax: ```max(<field> [, <field-n>]* )```

Available since: 0.9.25

#### Example

Returns the maximum salary of all the Account records:
```sql
select max(salary) from Account.
```

Returns the maximum value between 'salary1', 'salary2' and 'salary3' fields.
```sql
select max(salary1, salary2, salary3) from Account
```
---
### avg()

Returns the average value.

Syntax: ```avg(<field>)```

Available since: 0.9.25

#### Example

```sql
select avg(salary) from Account
```

---
### sum()

Syntax: ```sum(<field>)```

Returns the sum of all the values returned.

Available since: 0.9.25

#### Example

```sql
select average(salary) from Account
```
---
### date()

Returns a date formatting a string. &lt;date-as-string&gt; is the date in string format, and &lt;format&gt; is the date format following these [rules](http://download.oracle.com/javase/1.4.2/docs/api/java/text/SimpleDateFormat.html). If no format is specified, then the default database format is used.

Syntax: ```date( <date-as-string> [<format>] [,<timezone>] )```

Available since: 0.9.25

#### Example

```sql
select from Account where created <= date('2012-07-02', 'yyyy-MM-dd')
```
---
### sysdate()

Returns the current date time.

Syntax: ```sysdate( [<format>] [,<timezone>] )```

Available since: 0.9.25

#### Example

```sql
select sysdate('dd-MM-yyyy') from Account
```
---
### format()

Formats a value using the [String.format()](http://download.oracle.com/javase/1.5.0/docs/api/java/lang/String.html) conventions. Look [here for more information](http://download.oracle.com/javase/1.5.0/docs/api/java/util/Formatter.html#syntax).

Syntax: ```format( <format> [,<arg1> ](,<arg-n>]*.md)```

Available since: 0.9.25

#### Example

```sql
select format("%d - Mr. %s %s (%s)", id, name, surname, address) from Account
```
---
### dijkstra()

Returns the cheapest path between two vertices using the [http://en.wikipedia.org/wiki/Dijkstra's_algorithm Dijkstra algorithm] where the **weightEdgeFieldName** parameter is the field containing the weight. Direction can be OUT (default), IN or BOTH.

Syntax: ```dijkstra(<sourceVertex>, <destinationVertex>, <weightEdgeFieldName> [, <direction>])```

Available since: 1.3.0

#### Example

```sql
select dijkstra($current, #8:10, 'weight') from V
```
---
### shortestPath()

Returns the shortest path between two vertices. Direction can be OUT (default), IN or BOTH.

Syntax: ```shortestPath( <sourceVertex>, <destinationVertex> [, <direction>])```

Available since: 1.3.0

#### Example

```sql
select shortestPath(#8:32, #8:10, 'BOTH')
```
---
### distance()

Syntax: ```distance( <x-field>, <y-field>, <x-value>, <y-value> )```

Returns the distance between two points in the globe using the Haversine algorithm. Coordinates must be as degrees.

Available since: 0.9.25

#### Example

```sql
select from POI where distance(x, y, 52.20472, 0.14056 ) <= 30
```
---
### distinct()

Syntax: ```distinct(<field>)```

Retrieves only unique data entries depending on the field you have specified as argument. The main difference compared to standard SQL DISTINCT is that with OrientDB, a function with parenthesis and only one field can be specified.

Available since: 1.0rc2

#### Example

```sql
select distinct(name) from City
```
---
### unionall()

Syntax: ```unionall(<field> [,<field-n>]*)```

Works as aggregate or inline. If only one argument is passed then aggregates, otherwise executes and returns a UNION of all the collections received as parameters. Also works also with no collection values.

Available since: 1.7

#### Example

```sql
select unionall(friends) from profile
```

```sql
select unionall(inEdges, outEdges) from OGraphVertex where label = 'test'
```
---
### intersect()

Syntax: ```intersect(<field> [,<field-n>]*)```

Works as aggregate or inline. If only one argument is passed than aggregates, otherwise executes, and returns, the INTERSECTION of the collections received as parameters.

Available since: 1.0rc2

#### Example

```sql
select intersect(friends) from profile where jobTitle = 'programmer'
```

```sql
select intersect(inEdges, outEdges) from OGraphVertex
```
---
### difference()

Syntax: ```difference(<field> [,<field-n>]*)```

Works as aggregate or inline. If only one argument is passed than aggregates, otherwise executes, and returns, the DIFFERENCE between the collections received as parameters.

Available since: 1.0rc2

#### Example

```sql
select difference(tags) from book

```sql
select difference(inEdges, outEdges) from OGraphVertex
```

### set()

Adds a value to a set. The first time the set is created. If ```<value>``` is a collection, then is merged with the set, otherwise ```<value>``` is added to the set.

Syntax: ```set(<field>)```

Available since: 1.2.0

#### Example

```sql
SELECT name, set(roles.name) as roles FROM OUser
```
---
### list()

Adds a value to a list. The first time the list is created. If ```<value>``` is a collection, then is merged with the list, otherwise ```<value>``` is added to the list.

Syntax: ```list(<field>)```

Available since: 1.2.0

#### Example

```sql
SELECT name, list(roles.name) as roles FROM OUser
```
---
### map()

Adds a value to a map. The first time the map is created. If ```<value>``` is a map, then is merged with the map, otherwise the pair ```<key>``` and ```<value>``` is added to the map as new entry.

Syntax: ```map(<key>, <value>)```

Available since: 1.2.0

#### Example

```sql
SELECT map(name, roles.name) FROM OUser
```
---
### traversedElement()

Returns the traversed element(s) in Traverse commands.

Syntax: ```traversedElement(<index> [,<items>])```

Where:
- ```<index>``` is the starting item to retrieve. Value >= 0 means absolute position in the traversed stack. 0 means the first record. Negative values are counted from the end: -1 means last one, -2 means the record before last one, etc.
- ```<items>```, optional, by default is 1. If >1 a collection of items is returned

Available since: 1.7

#### Example

Returns last traversed item of TRAVERSE command:
```sql
SELECT traversedElement(-1) FROM ( TRAVERSE out() from #34:3232 WHILE $depth <= 10 )
```

Returns last 3 traversed items of TRAVERSE command:
```sql
SELECT traversedElement(-1, 3) FROM ( TRAVERSE out() from #34:3232 WHILE $depth <= 10 )
```
---
### traversedEdge()

Returns the traversed edge(s) in Traverse commands.

Syntax: ```traversedEdge(<index> [,<items>])```

Where:
- ```<index>``` is the starting edge to retrieve. Value >= 0 means absolute position in the traversed stack. 0 means the first record. Negative values are counted from the end: -1 means last one, -2 means the edge before last one, etc.
- ```<items>```, optional, by default is 1. If >1 a collection of edges is returned

Available since: 1.7

#### Example

Returns last traversed edge(s) of TRAVERSE command:
```sql
SELECT traversedEdge(-1) FROM ( TRAVERSE outE(), inV() from #34:3232 WHILE $depth <= 10 )
```

Returns last 3 traversed edge(s) of TRAVERSE command:
```sql
SELECT traversedEdge(-1, 3) FROM ( TRAVERSE outE(), inV() from #34:3232 WHILE $depth <= 10 )
```
---
### traversedVertex()

Returns the traversed vertex(es) in Traverse commands.

Syntax: ```traversedVertex(<index> [,<items>])```

Where:
- ```<index>``` is the starting vertex to retrieve. Value >= 0 means absolute position in the traversed stack. 0 means the first vertex. Negative values are counted from the end: -1 means last one, -2 means the vertex before last one, etc.
- ```<items>```, optional, by default is 1. If >1 a collection of vertices is returned

Available since: 1.7

#### Example

Returns last traversed vertex of TRAVERSE command:
```sql
SELECT traversedVertex(-1) FROM ( TRAVERSE out() from #34:3232 WHILE $depth <= 10 )
```

Returns last 3 traversed vertices of TRAVERSE command:
```sql
SELECT traversedVertex(-1, 3) FROM ( TRAVERSE out() from #34:3232 WHILE $depth <= 10 )
```
---
### mode()

Returns the values that occur with the greatest frequency. Nulls are ignored in the calculation.

Syntax: ```mode(<field>)```

Available since: 2.0-M1

#### Example

```sql
select mode(salary) from Account
```
---
### median()

Returns the middle value or an interpolated value that represent the middle value after the values are sorted. Nulls are ignored in the calculation.

Syntax: ```median(<field>)```

Available since: 2.0-M1

#### Example

```sql
select median(salary) from Account
```
---
### percentile()

Returns the nth percentiles (the values that cut off the first n percent of the field values when it is sorted in ascending order). Nulls are ignored in the calculation.

Syntax: ```percentile(<field> [, <quantile-n>]*)```

Available since: 2.0-M1

#### Examples

```sql
select percentile(salary, 95) from Account
select percentile(salary, 25, 75) as IQR from Account
```
---
### variance()

Returns the middle variance: the average of the squared differences from the mean. Nulls are ignored in the calculation.

Syntax: ```variance(<field>)```

Available since: 2.0-M1

#### Example

```sql
select variance(salary) from Account
```
---
### stddev()

Returns the standard deviation: the measure of how spread out values are. Nulls are ignored in the calculation.

Syntax: ```stddev(<field>)```

Available since: 2.0-M1

#### Example

```sql
select stddev(salary) from Account
```
---
### uuid()

Generates a UUID as a 128-bits value using the Leach-Salz variant. For more information look at: http://docs.oracle.com/javase/6/docs/api/java/util/UUID.html.

Available since: 2.0-M1

Syntax: ```uuid()```

#### Example

Insert a new record with an automatic generated id:

```sql
INSERT INTO Account SET id = UUID()
```


---

## Custom functions

The SQL engine can be extended with custom functions written with a Scripting language or via Java.

### Database's function

Look at the [Functions](Functions.md) page.

### Custom functions in Java

Before to use them in your queries you need to register:

```java
// REGISTER 'BIGGER' FUNCTION WITH FIXED 2 PARAMETERS (MIN/MAX=2)
OSQLEngine.getInstance().registerFunction("bigger",
                                          new OSQLFunctionAbstract("bigger", 2, 2) {
  public String getSyntax() {
    return "bigger(<first>, <second>)";
  }

  public Object execute(Object[] iParameters) {
    if (iParameters[0] == null || iParameters[1] == null)
      // CHECK BOTH EXPECTED PARAMETERS
      return null;

    if (!(iParameters[0] instanceof Number) || !(iParameters[1] instanceof Number))
      // EXCLUDE IT FROM THE RESULT SET
      return null;

    // USE DOUBLE TO AVOID LOSS OF PRECISION
    final double v1 = ((Number) iParameters[0]).doubleValue();
    final double v2 = ((Number) iParameters[1]).doubleValue();

    return Math.max(v1, v2);
  }

  public boolean aggregateResults() {
    return false;
  }
});
```

Now you can execute it:

```java
List<ODocument> result = database.command(
  new OSQLSynchQuery<ODocument>("select from Account where bigger( salary, 10 ) > 10") )
  .execute();
```