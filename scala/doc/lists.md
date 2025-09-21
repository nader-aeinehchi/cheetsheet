```
val intList1 : Int => List[Int] = i => (1 to i).toList
def intList1 : Int => List[Int] = i => (1 to i).toList  //does not compile

val intList2 : Int => List[Int] = {i => (1 to i).toList} //does not compile
val intList2 : {Int => List[Int] = {i => (1 to i).toList}} // does not compile
val intList2 : (Int => List[Int]) = {i => (1 to i).toList}} // does not compile

val intList2 : Int => List[Int]
intList2 = {(1 to i).toList}

val intList3 = (i:Int) => {(1 to i).toList} //compiles
def intList3 = (i:Int) => {(1 to i).toList} //compiles



 def this(code2: Function[ActorRef, Procedure[Any]], discardOld: Boolean) =
    this((self2: ActorRef) ⇒ {
      val behavior = code2(self2)
      val result: Actor.Receive = { case msg ⇒ behavior(msg) }
      result
    }, discardOld)

```