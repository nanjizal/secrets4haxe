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

Haxe expects a 'main' function method as it's starting point.  We can mix functions and classes.

```Haxe
function main(){
  var test: Test = new Test();
}
class Test(){
  function new(){
    trace("Haxe is great!"); 
  }
}
```
