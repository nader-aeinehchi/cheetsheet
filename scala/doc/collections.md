# Collections Types

https://docs.scala-lang.org/scala3/book/collections-classes.html

Looking at Scala collections from a high level, there are three main categories to choose from:

Sequences are a sequential collection of elements and may be indexed (like an array) or linear (like a linked list)
Maps contain a collection of key/value pairs, like a Java Map, Python dictionary, or Ruby Hash
Sets are an unordered collection of unique elements

| Collection Type   | Immutable     | Mutable	| Description   | 
| ---               | ---           |---	    | ---	        | 
| List	            | ✓             | 	        | A linear (linked list), immutable sequence | 
| Vector	        | ✓	            |           | An indexed, immutable sequence | 
| LazyList	        | ✓	            | 	        | A lazy immutable linked list, its elements are computed only when they’re needed; Good for large or infinite sequences. | 
| ArrayBuffer       | 	            | ✓	        | The go-to type for a mutable, indexed sequence | 
| ListBuffer        | 	            | ✓	        | Used when you want a mutable List; typically converted to a List | 
| Map	            | ✓	            | ✓	        | An iterable collection that consists of pairs of keys and values. | 
| Set	            | ✓	            | ✓	        | An iterable collection with no duplicate elements | 


# Concurrent lists using Java


| Collection Type   | Synchronize     | Non-blocking	| Description   | 
| ---               | ---           |---	    | ---	        | 
| SynchronizedList	| ✓             | 	        |  | 
| CopyOnWriteArrayList	| ?             | 	  ?      |  | 
| ConcurrentLinkedQueue	| 	            |    ✓       | In Java, the ConcurrentLinkedQueue is the part of the java.util.concurrent package and implements a FIFO(First-In-First-Out) queue. It is a thread-safe, non-blocking, and scalable queue designed for use in highly concurrent environments. The queue uses a lock-free algorithm, ensuring that multiple threads can safely enqueue and dequeue elements without the need for external synchronization. | 


See:

- https://www.geeksforgeeks.org/concurrentlinkedqueue-in-java-with-examples/
- https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/package-summary.html
- https://www.codejava.net/java-core/concurrency/java-concurrent-collection-copyonwritearraylist-examples
- https://medium.com/@AlexanderObregon/javas-collections-synchronizedlist-explained-cce3e7b50725#:~:text=synchronizedList()%20method%20from%20the,synchronized%20access%2C%20ensuring%20thread%20safety.


