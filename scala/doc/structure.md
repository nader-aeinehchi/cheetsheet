# Structure

## Map (in Python dictionary)

```
var capital = Map("US" -> "Washington", "France" -> "Paris")
capital += ("Japan" -> "Tokyo")
Prepared for Nader Aeinehchi
println(capital("France"))
```

In the above program, you’ll get a default Map implementation, but you
can easily change that. You could for example specify a particular implemen-
tation, such as a HashMap or a TreeMap, or with Scala’s parallel collections
module, invoke the par method to obtain a ParMap that executes operations
in parallel. You could specify a default value for the map, or you could over-
ride any other method of the map you create. In each case, you can use the
same easy access syntax for maps as in the example above.

