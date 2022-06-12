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
Since the added function calls can be heavy we can inline them so they dissappear at runtime.
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


