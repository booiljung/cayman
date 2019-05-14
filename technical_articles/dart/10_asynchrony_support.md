[Up](./index.md)

##  Asynchrony support

Dart libraries are full of functions that return [Future](https://api.dartlang.org/stable/dart-async/Future-class.html) or [Stream](https://api.dartlang.org/stable/dart-async/Stream-class.html) objects. These functions are *asynchronous*: they return after setting up a possibly time-consuming operation (such as I/O), without waiting for that operation to complete.

The `async` and `await` keywords support asynchronous programming, letting you write asynchronous code that looks similar to synchronous code.



###  Handling Futures

When you need the result of a completed Future, you have two options:

- Use `async` and `await`.
- Use the Future API, as described [in the library tour](https://dart.dev/guides/libraries/library-tour#future).

Code that uses `async` and `await` is asynchronous, but it looks a lot like synchronous code. For example, here’s some code that uses `await` to wait for the result of an asynchronous function:

```dart
await lookUpVersion();
```

To use `await`, code must be in an *async function*—a function marked as `async`:

```dart
Future checkVersion() async {
  var version = await lookUpVersion();
  // Do something with version
}
```

**Note:** Although an async function might perform time-consuming operations, it doesn’t wait for those operations. Instead, the async function executes only until it encounters its first `await` expression ([details](https://github.com/dart-lang/sdk/blob/master/docs/newsletter/20170915.md#synchronous-async-start)). Then it returns a Future object, resuming execution only after the `await` expression completes.

Use `try`, `catch`, and `finally` to handle errors and cleanup in code that uses `await`:

```dart
try {
  version = await lookUpVersion();
} catch (e) {
  // React to inability to look up the version
}
```

You can use `await` multiple times in an async function. For example, the following code waits three times for the results of functions:

```dart
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```

In `await *expression*`, the value of `*expression*` is usually a Future; if it isn’t, then the value is automatically wrapped in a Future. This Future object indicates a promise to return an object. The value of `await *expression*` is that returned object. The await expression makes execution pause until that object is available.

**If you get a compile-time error when using await, make sure await is in an async function.** For example, to use `await` in your app’s `main()` function, the body of `main()` must be marked as `async`:

```dart
Future main() async {
  checkVersion();
  print('In main: version is ${await lookUpVersion()}');
}
```



###  Declaring async functions

An *async function* is a function whose body is marked with the `async` modifier.

Adding the `async` keyword to a function makes it return a Future. For example, consider this synchronous function, which returns a String:

```dart
String lookUpVersion() => '1.0.0';
```

If you change it to be an async function—for example, because a future implementation will be time consuming—the returned value is a Future:

```dart
Future<String> lookUpVersion() async => '1.0.0';
```

Note that the function’s body doesn’t need to use the Future API. Dart creates the Future object if necessary.

If your function doesn’t return a useful value, make its return type `Future<void>`.



###  Handling Streams

When you need to get values from a Stream, you have two options:

- Use `async` and an *asynchronous for loop* (`await for`).
- Use the Stream API, as described [in the library tour](https://dart.dev/guides/libraries/library-tour#stream).

**Note:** Before using `await for`, be sure that it makes the code clearer and that you really do want to wait for all of the stream’s results. For example, you usually should **not** use `await for` for UI event listeners, because UI frameworks send endless streams of events.

An asynchronous for loop has the following form:

```dart
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
```

The value of `*expression*` must have type Stream. Execution proceeds as follows:

1. Wait until the stream emits a value.
2. Execute the body of the for loop, with the variable set to that emitted value.
3. Repeat 1 and 2 until the stream is closed.

To stop listening to the stream, you can use a `break` or `return` statement, which breaks out of the for loop and unsubscribes from the stream.

**If you get a compile-time error when implementing an asynchronous for loop, make sure the await for is in an async function.** For example, to use an asynchronous for loop in your app’s `main()` function, the body of `main()` must be marked as `async`:

```dart
Future main() async {
  // ...
  await for (var request in requestServer) {
    handleRequest(request);
  }
  // ...
}
```

For more information about asynchronous programming, in general, see the [dart:async](https://dart.dev/guides/libraries/library-tour#dartasync---asynchronous-programming) section of the library tour.

