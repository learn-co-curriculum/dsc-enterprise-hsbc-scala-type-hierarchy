
# Type Hierarchy

##  Reviewing the relationship of class and objects

A type is a `class`, `trait`, a primitive, or an `object` in Scala.

A class for those who don’t know is a code template, or blueprint, that describes what the objects created from the blueprint will look like.

![vmarchitect.png](../images/vmarchitect.png)


* `Int` is a type
* `String` is a type
* If we created a `Car` class, `Car` would be a type
* If we created `InvoiceJSONSerializer`, `InvoiceJSONSerializer` would be a type

## Matching Types

Given these two methods:


```scala
def add(x:Int,y:Int):Int = x + y
def subtract(x:Int, y:Int):Int = x - y
```




    add: (x: Int, y: Int)Int
    subtract: (x: Int, y: Int)Int
    



To use them together, you would just need to match the types:


```scala
add(subtract(10, 3), subtract(100, 22))
```




    res2: Int = 85
    



If we changed `subtract` to use `Double` instead:


```scala
def add(x:Int,y:Int):Int = x + y
def subtract(x:Double, y:Double):Double = x - y
```




    add: (x: Int, y: Int)Int
    subtract: (x: Double, y: Double)Double
    



We can no longer use the same invocation


```scala
add(subtract(10, 3), subtract(100, 22))
```


    <console>:29: error: type mismatch;

     found   : Double

     required: Int

           add(subtract(10, 3), subtract(100, 22))

                       ^

    <console>:29: error: type mismatch;

     found   : Double

     required: Int

           add(subtract(10, 3), subtract(100, 22))

                                        ^

    


We would just need to make sure that that type matches:


```scala
add(subtract(10.0, 3.0).round.toInt, subtract(100.0, 22.0).round.toInt)
```




    res4: Int = 85
    



NOTE: This is going to be one of the key distinctions between a language like Scala and Python. Static types heavily matter in a language like Scala


## Primitives Are Objects

* In Scala we treat everything like an object
* Therefore, you can treat all numbers, boolean, and characters as types
* Every primitive is a member of `AnyVal` 

## `java.lang.Object` has been demoted

* `java.lang.Object` is called AnyRef in Scala
* `AnyRef` will be the super type of all object references
* This includes:
  * Scala API Classes
  * Java API Classes
  * Your custom classes

## `Any` is the new leader

* Super type of:
  * `AnyVal` (Primitive Wrappers)
  * `AnyRef` (Object References)

## Scala Family Tree

![scalahierarchy.png](../images/scalahierarchy.png)

## **Lab:** Polymorphism

**Step 1:** Try various combinations of references and see how everything is match together. Try some of these combinations that may or may not work to be sure:


```scala
val a:AnyVal = 40
```




    a: AnyVal = 40
    




```scala
val b:Any = a
```




    b: Any = 40
    




```scala
val c:AnyRef = "A String"
```




    c: AnyRef = A String
    




```scala
val d:Any = c
```




    d: Any = A String
    




```scala
val e:AnyRef = 40
```


    <console>:24: error: the result type of an implicit conversion must be more specific than AnyRef

           val e:AnyRef = 40

                          ^

    



```scala
val f:AnyVal = "A String"
```


    <console>:24: error: the result type of an implicit conversion must be more specific than AnyVal

           val f:AnyVal = "A String"

                          ^

    


**Step 2:** Discuss and ask any questions.

## The Complete Hierarchy

![scalaclasshierarchy.png](../images/scalaclasshierarchy.png)

## `Nothing`

* `Nothing` is the sub type of everything
* It is used for collections and other containers with no defined parameterized type
* Used to represent a "bottom", a type that covers among other things, methods, that only throw `Throwable`

## `Nothing` in List

Given: A `List` with no parameterized type…​


```scala
val list = List()
```




    list: List[Nothing] = List()
    



Given: A List with a type, and note the difference


```scala
val list2 = List[Int]()
```

## Nothing and throwing an Exception
Given an exception, thrown from a method, and that’s all that is thrown:


```scala
def ohoh(i:Int):Nothing = { throw new Exception() }
```




    ohoh: (i: Int)Nothing
    



It shouldn’t matter if it is an `Exception` or a `RuntimeException`
Link: http://www.scala-lang.org/api/current/scala/Nothing.html

## `Null`

* `Null` is a type in Scala that represents a null
* `Null` is the sub type of all object references
* `null` is not used in pure Scala applications, but only for those that interop with Java

## `Null` in use


```scala
val f = null
```




    f: Null = null
    



NOTE: The type returned

## `Null` when a type is established


```scala
val x:String = null
```




    x: String = null
    



NOTE: The type is `String`. The reason the about is that null is the subtype of all `AnyRef`. This is just pure polymorphism

## `???`

The triple question mark `???` is a way to mark that a method, `val`, `var` is unimplemented

The signature:

```
/** <code>???</code> can be used for marking methods
  *  that remain to be implemented.
  *  @throws NotImplementedError
  *  @group utilities
  */
def ??? : Nothing = throw new NotImplementedError
```

* It is of a type `Nothing`
* That also means that it is type safe
* Perfect for compilation without an actual implementation
* Perfect for Test Driven Development

## Example of using `???`

* Compilation works
* `???` throws a `NotImplementedError`
* Therefore, the type returns `Nothing`
* Nothing is the subtype of everything, including, `Int`


```scala
def add(x:Int, y:Int):Int = ???
add(10, 12) + 3
```


    scala.NotImplementedError: an implementation is missing

      at scala.Predef$.$qmark$qmark$qmark(Predef.scala:230)

      at add(<console>:26)

      ... 36 elided

    


## Type Hierarchy Conclusion

* Types are templates that make up an object.
* Primitives are also types, Int, Short, Byte, Char, Boolean, etc.
* Types need to matched up like a puzzle. If it isn’t the type system at compile time will tell you there is a type mismatch
* Every type is in a relationship.
  * `Any` is the parent for all types
  * `AnyVal` is the parent for all primitives
  * `AnyRef` is the parent for all Scala, Java, and custom classes that you create
  * `Nothing` is the subtype for everything
  * `Null` is the subtype of all references
* `???` is a method that returns `NotImplementedError`
* `???` returns `Nothing` and therefore can be used in nearly any method as a placeholder until you have an implementation
* It can be used for Test Driven Development
