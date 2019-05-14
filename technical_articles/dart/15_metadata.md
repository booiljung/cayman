[Up](./index.md)

##  Metadata

Use metadata to give additional information about your code. A metadata annotation begins with the character `@`, followed by either a reference to a compile-time constant (such as `deprecated`) or a call to a constant constructor.

Two annotations are available to all Dart code: `@deprecated` and `@override`. For examples of using `@override`, see [Extending a class](https://dart.dev/guides/language/language-tour#extending-a-class). Here’s an example of using the `@deprecated` annotation:

```dart
class Television {
  /// _Deprecated: Use [turnOn] instead._
  @deprecated
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {...}
}
```

You can define your own metadata annotations. Here’s an example of defining a @todo annotation that takes two arguments:

```dart
library todo;

class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}
```

And here’s an example of using that @todo annotation:

```dart
import 'todo.dart';

@Todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}
```

Metadata can appear before a library, class, typedef, type parameter, constructor, factory, function, field, parameter, or variable declaration and before an import or export directive. You can retrieve metadata at runtime using reflection.

