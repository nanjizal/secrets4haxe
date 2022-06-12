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

Haxe expects a 'main' function method as it's starting point.  We can call the js console log from a class.

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
Haxe is 'Strongly typed' but has a smart compiler and so declaring type is not always needed.
```Haxe
function main(){
  var test = new Test();
}
class Test {
  public function new(){
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
 function haxeBye(){
    trace("Goodbye from Haxe");  
  }
}
```
Properties in Haxe are unusual.

