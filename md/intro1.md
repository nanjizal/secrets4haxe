## Secrets 4 Haxe

It is simple to test some basic Haxe in the browser. I will not explain everything but jump straight in and get a feel.

Go to https://try.haxe.org/

```Haxe
class Test {
  static function main() {
    trace("Haxe is great!");
  }
}
```
press the 'build' button to see the javascript output. Haxe can be compiled to many targets and this code will do similar accross them all.
  
  
Recent haxe allows even simpler creation of a Haxe project.

```Haxe
function main() {
  trace("Haxe is great!");
}
```

Haxe expects a 'main' function method as it's starting point.  So 'main' is a special case, just related to a programs setup and is not needed in other classes.
Class are created with 'new' and have a new constuctor where any defined parameters will be initialised. 
  
```Haxe
function main(){
  var test: Test = new Test();
}
class Test {
  private var message = "Haxe is Great!";
  public function new(){
    trace(message); 
  }
}
```
  
Haxe is 'Strongly typed' but has a smart compiler and so declaring type is not always needed.
Notice below how type information has been added and removed but with no change to how the compiler infers the code.
Haxe will always try to type your code even when you leave out type information. Consider to add type information when you want to provide clarity to another programmer or to resolve your type intent properly to the compiler. Notice how all functions return although if they return nothing then we can Omit the Void typing and the return statement.

```Haxe
function main(): Void{
  var test = new Test();
  return;
}
class Test {
  private var message: String = "Haxe is Great!";
  public function new(): Void{
    trace(message);
    return;
  }
}
```
  
With class based code we often keep constructor code to a minimum and move functionality into methods.  Object Orientated programming is a way to split code into many small ideas that action on an Object ( Class ).  
  
```Haxe
function main(){
  var test = new Test();
  test.haxeGreat();
  test.haxeBye();
}
class Test {
  public function new(){}
  public function haxeGreat(){
    trace("Haxe is great!"); 
  }
  public function haxeBye(){
    trace("Goodbye from Haxe");  
  }
}
```
  
With haxe your methods are either public or private. Private limits easy access outside the class ( with inheritance you can still access ).
  
```Haxe
function main(){
  var test = new Test();
  test.haxeGreat();
  // Can't call haxeBye here !
}
class Test {
  public function new(){}
  public function haxeGreat(){
    trace("Haxe is great!");
    /*
    can call haxeBye from within the class.
    as you can see two ways to add comments in code
    */
    haxeBye();
  }
 private function haxeBye(){
    trace("Goodbye from Haxe");  
  }
}
```

Class parameters just use the 'var', variable keyword. Properties ( getter/setters ) are unusual. If method does not say it's public or private it is private, so message here is private and only accessible from methods in Test class.
   
```Haxe
function main() ( new Test() ).haxeMessage();
class Test {
  var message = "Haxe is great!";
  public function new(){}
  public function haxeMessage(){
    trace(message);
  }
}
```
  
Getters and setters also use the 'var' keyword, normally 'var' is used public and the accessors are private, they need to use the 'get_' and 'set_' preface, and stard 'get' and 'set' var paramaters.  The structure is an unusual aspect of Haxe.  
  
```Haxe
function main() ( new Test() ).haxeMessage();
class Test {
  var messageHolder = '';
  public var message( get, set ):String;
  function get_message(): String {
    return messageHolder;
  }
  function set_message( str: String ): String {
    messageHolder = str;
    return str;
  }
  public function new(){ 
    message = "Haxe getter setters are unusual";
  }
  public function haxeMessage(){
    trace(message);
  }
}
```
  
Function calls for properties can reduce runtime speed as each function call adds time, by 'inline' the functions overhead dissappear at runtime, inlining is powerful, but can result in large generated code so use with caution especially on Javascript target. The compiler treats the inline statement as an instruction to copy the methods contents where it's called but retain scope to avoid the function call. When lots of methods are inline and used a lot and sometimes can be semi recursive, then the generated runtime code can be fast but very large with lots of repitition.  Inline requires the function only returns at the end so no early returns can be used.  Inlining a method.  
   
```Haxe
function main() ( new Test() ).haxeMessage();
class Test {
  var messageHolder = '';
  public var message( get, set ):String;
  inline function get_message(): String {
    return messageHolder;
  }
  inline function set_message( str: String ): String {
    messageHolder = str;
    return str;
  }
  public function new(){ 
    message = "Haxe getter setters are unusual";
  }
  public function haxeMessage(){
    trace(message);
  }
}
```
  
we can inline methods, but we can't use them with the var property part ```public var message( get, set ): String;```, but just on the getter setter part will achieve this aim.
There are some other options if we only want to allow get or only set, we can use 'never'.  
'default' can be used to save declaring some of the methods.  
'null' and '@:isVar' are other options used, covered in haxe manual.  https://haxe.org/manual/class-field-property-rules.html
  
With the latest Haxe here is a rather strange approach using an 'abstract type', abstract types only exist at compile they are like virtual types often used to add funtionality without full cost of inheritance.  Covered here to understand they can be an alternative to a Classs, they disappear at runtime so not so useful if using Haxe as a Javascript library.
  
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
```


