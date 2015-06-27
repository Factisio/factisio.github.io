![](logo-01.png "Factis database system")

The modular database system.

# Presentation

Factis is a modular datastore written in pure javascript (native C datastore coming). Factis is similar to RDF datastores, and in particular Hexastore implementations.

The `factis` package is a ready-to-use version of the whole Factis ecosystem.

Factis is made of a core module `factis-core` which contains the query API and the query engine.

Then different datastore modules `factis-store-something` can be plugged in in order to provide different functionalities. The datastore modules comply to a simple triple store interface. Available modules are:

  - `factis-store-group` the basic store that allow to group several stores, providing the composition semantics.
  - `factis-store-hexastore` an efficient hexastore implementation in pure JS with no dependencies
  - `factis-store-identity` which provides the semantics for the `equals` and `not equals` predicates
  - Some more to come ! (Dates, Geometry, JSON...)

# Install

~~~{.bash}
npm install factis
~~~

# Usage

~~~{.javascript}
var Factis=require('factis');

var f = new Factis();

f.add([
  f.fact("Factis","is","nice"),
  f.fact("Factis","is","fast")
  ]);

f.query(f.fact(f.the("Thing"),"is","nice"));
~~~

# Documentation

Factis is basically an augmented triple store. CRUD operations are implemented by using only three operations: Add, Remove, Query.

## Add and Remove

Add and remove single facts, or triples:

~~~{.javascript}
f.add(    f.fact("subject","predicate","object") );
f.remove( f.fact("subject","predicate","object") );
~~~

Add and remove multiple facts, or triples:

~~~{.javascript}
f.add([
   f.fact("s1","p1","o1")
   f.fact("s2","p2","o2")
])
f.remove([
   f.fact("s1","p1","o1")
   f.fact("s2","p2","o2")
])
~~~

## Query


Query operators are first order logic operators:

~~~{.javascript}
var query =
  f.and(
    f.fact( f.the("thing") , "is" , "nice" ),
    f.fact( f.the("thing") , "is" , "fast" ),
    f.not(
      f.fact( f.the("thing") , "is" , "bad" )
      )
    );
~~~

To perform a query:

~~~{.javascript}
var result =
  f.query( query );
~~~

Query returns an array of maps which give values to the query variables:

~~~{.javascript}
result == [
  { Thing : "Factis" }
  ]
~~~

If the query result is infinite, the query returns `null`
