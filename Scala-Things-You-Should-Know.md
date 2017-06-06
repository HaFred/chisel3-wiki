### Packages
Scala packages are a way to hierarchically organize code.  Scala source files should have a package name at the beginning something like 
```scala
package mytools
class Tool1 { ... }
```
This name can be used when referencing code defined in this file.  
```scala
import mytools.Tool1
```
Note: The package name  **should** match the directory hierarchy, this is not mandatory but failing to abide by this guideline can produce some unusual and difficult to diagnose problems. Package names by convention are lower case and do not contain separators like underscore.  This sometimes makes good descriptive names difficult.  One approach is too add a layer of hierachy ```package good.tools```.  Do your best.  Chisel itself plays some games with the package names that do not conform to these rules.

### A Simple Class Example
An example of creating a Scala class might be
```scala
class WrapCounter(counterBits: Int) {
  val max: Long = 1 << (counterBits + 1) - 1
  var counter = 0L
  def inc(): Long = {
    counter = counter + 1
    if(counter > max) counter = 0
    counter
  }
  println(s"counter created with max value $max")
}
```
What is here:
* class MyCounter: This is the definition of MyCounter
* (counterBits: Int): Creating MyCounter requires an integer argument, nicely named to suggest it is the bit width of the counter
* braces ({}) delimit a block of code. Most classes use a code block to define variables and constants and methods (functions)
* the class contains a member variable max, declared as ```Long``` and initialized as the class is created to be the maximum value that can be contained in counterBits bits.  Since max was created with _val_ it cannot be changed.
* a variable _counter_ is created and initialized to 0L, the _L_ says that 0 is a long value and from this _counter_ is inferred to be Long.
* a class method inc is defined which takes no arguments and returns a Long value
* the body of the method _inc_ is a code block that:
  * increments the counter
  * tests if it is greater than the max value and sets it back to zero if it is
  * the last line of the code block is important, any value expressed at the last line of a code block is considered to be the return value of that code block, that return value can be used or ignore by the programmer
  * so in this case the function _inc_ returns the value of the counter
* The println prints a string to the standard out.  Because the println is directly in the defining code block it is part of the classes initialization code and is run, i.e. prints out the string, every time an instance of this class is created.
* The string printed in this case is an _interpolated_ string
  * the leading s in front of the first double quote identifies this as an interpolated
  * an interpolated string is processed at run time  
  * the $max is replaced with the value of max
  * if the $ is followed by a code block arbitrary scala can be in that code block
    * for example ${max + max}
    * the return value of this code block will be inserted  in place of ${...}
    * if the return value is not a string it will be converted to one, virtually every class or type in scala has implicit conversion to a string)
  * it should be noted that classes that print something every time an instance is created is darned annoying and should avoided