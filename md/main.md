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

Class parameters just use the 'var', variable keyword. Properties ( getter/setters ) are unusual. If method does not say it's public or private it is private, so message here is private and only accessible from methods in Test class.  To call a method on a instance of a class we use dot notation.  ```myInstance.myMethod(myParameter)```, notice this used in the shorted main function, since single line, wiggle brackets can be omitted if preferred.
   
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
  
[next >](main2.md)
