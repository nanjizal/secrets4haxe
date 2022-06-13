## Secrets 4 Haxe

It is simple to test some basic Haxe in the browser.

Go to https://try.haxe.org/

```Haxe
class Test {
  static function main() {
    trace("Haxe is great!");
  }
}
```

Recent haxe allows even simpler

```Haxe
function main() {
  trace("Haxe is great!");
}
```

Haxe expects a 'main' function method as it's starting point.  We can call trace (  js console log for this case ) from a class.

```Haxe
function main(){
  var test: Test = new Test();
}
class Test {
  public function new(){
    trace("Haxe is great!"); 
  }
}
```
Haxe is 'Strongly typed' but has a smart compiler and so declaring type is not always needed, notice how type information has been added and removed but with no change to how the compiler infers the code.
```Haxe
function main():Void{
  var test = new Test();
}
class Test {
  public function new():Void{
    trace("Haxe is great!"); 
  }
}
```
With class based code we often keep constructor code to a minimum and move functionality into methods.  
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
With haxe your methods are either public or private, private limits easy access outside the class ( with inheritance you can still access ).
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
Simple class parameters are simple but properties ( getter/setters ) are unusual. If method does not say it's public or private it is private.
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
Getters and setters, often the var is public and the accessors are private, they need to use the 'get_' and 'set_' preface.
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
Since the added function calls can all add up we can inline them so they dissappear at runtime, inlining is powerful but can result in large generated code so use with caution especially on Javascript target.
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
we can inline methods, but we can't use the keyword on ```public var message( get, set ): String;```.
There are some other options if we only want to allow get or only set, we can use 'never'. 'default' can be used to save declaring some of the methods. 'null' and '@:isVar' are other options used, covered in haxe manual.  https://haxe.org/manual/class-field-property-rules.html
  
With the latest Haxe here is a rather strange approach using an 'abstract type', abstract types only exist at compile they are like virtual types often used to add funtionality without full cost of inheritance.
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
A more convoluted example using also an '@:initStruct' class.
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
these are often a faster implementation of a simple typedef.
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
'cast' is used to tell the compiler to trust you and that the compiled types will work so it is rather risky.  Often with some more variables you can reduce many uses related to abstracts. Typedef are often used like a wrapper to a method or class, they can be used as a polyfill but normally when used for say a vector point often the compiler can optimise them away dependending on use.
  
We can't extend a type with a class so using an abstract can be powerful. But we can write the abstract type class example with inheritance as two classes.
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
We can use interfaces with classes
```Haxe
function main() {
  var temp: Test = new Test( "Haxe is Great!" );
  var test: CanTrace = temp;
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


