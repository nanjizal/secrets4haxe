## main
( part2 )

### Tracing using abstract type.

With the latest Haxe here is a rather strange approach using an 'abstract type', abstract types only exist at compile they are like virtual types often used to add funtionality without full cost of inheritance.  Covered here to understand they can be an alternative to a Class, they disappear at runtime so not so useful if using Haxe as a Javascript library from Javascript rather than Haxe.
  
```Haxe
function main() {
  var str = new Test("Haxe is Great!");
  str.trace();
}
abstract Test( String ) from String to String {
  public inline function new( str: String ){
		this = str;
  }
  public inline function trace(){
    trace( abstract );
  }
}
```
  
A more convoluted example using also an '@:initStruct' class allows Object style initialization of the class.

```Haxe
function main() {
  var test: Test = cast { message: "Haxe is Great!"};
  test.trace();
}
@:initStruct
class Test_{
  public var message: String;
  public function new( message: String ){
    this.message = message;
  }
}
abstract Test( Test_ ) from Test_ {
  public inline function trace(){
    trace( this.message );
  }
}
```
  
these are often a faster implementation of a simple typedef. Typedef are much like a label ( pointer to ) for an annonymous object structure, class or function. 
  
```Haxe
function main() {
  var test: Test = cast { message: "Haxe is Great!"};
  test.trace();
}
typedef Test_ = {
  public var message: String;
}
abstract Test( Test_ ) from Test_ {
  public inline function trace(){
    trace( this.message );
  }
}
```
  
'cast' is used to tell the compiler to trust you and that the compiled types will work so it is rather risky.  Often with some more variables you can reduce many cast uses related to abstracts. Typedef are often used like a wrapper to a method, or class, they can be used as a polyfill but normally when used for say a vector point he compiler may be able to optimise them away dependending on use, such as when numerical values can be easily extracted.  
   
We can't extend a type with a class so using an abstract can be powerful. But we can write the abstract type class example with inheritance as two classes, using 'extends' keyword.   
  
```Haxe
function main() {
  var test: Test = cast { message: "Haxe is Great!"};
  test.trace();
}
@:initStruct
class Test_{
  public var message: String;
  public function new( message: String ){
    this.message = message;
  }
}
class Test extends Test_ {
  public inline function trace(){
    trace( this.message );
  }
}
```
  
We can use interfaces ( several if needed ) with classes.  
  
```Haxe
function main() {
  var test: CanTrace = new Test( "Haxe is Great!" );
  test.trace();
  test = new Test2();
  test.trace();
}
interface CanTrace {
   public function trace():Void;
}
class Test implements CanTrace {
  public var message: String;
  public function new( message: String ){
    this.message = message;
  }
  public inline function trace(){
    trace( this.message );
  }
}
class Test2 implements CanTrace {
  public function new(){}
  public function trace(){
    trace( 'test2' );
  }
}
