# Output-only Code Questions (Dart)

For each snippet below, state exactly what is printed to the console (no explanation required). Do not run the code — reason about output. These are intended for quick assessment of understanding of Dart/Flutter language semantics (mixins, inheritance, spread, equality, async, BigInt, operator overloading, etc.).

## Table of Contents

- [Mixins & Inheritance](#mixins--inheritance) (Q1-3)
- [Collections & Operators](#collections--operators) (Q4-5, Q9-11)
- [Async & Event Loop](#async--event-loop) (Q6, Q17-20)
- [Algorithms & Recursion](#algorithms--recursion) (Q7, Q12-14)
- [Widget Lifecycle](#widget-lifecycle) (Q15, Q21)
- [Advanced Async Patterns](#advanced-async-patterns) (Q16, Q18-19)

---

## Mixins & Inheritance

### Question 1 — mixin method resolution

```dart
mixin A {
  String foo() => 'A';
}

mixin B {
  String foo() => 'B';
}

class C with A, B {}

void main() {
  print(C().foo());
}
```

---

### Question 2 — `mixin on` constraint and runtime type

```dart
class Base {
  String name = 'base';
}

mixin M on Base {
  String who() => name + ':' + runtimeType.toString();
}

class Impl extends Base with M {}

void main() => print(Impl().who());
```

---

### Question 3 — inheritance + operator override

```dart
class V {
  final int x;
  V(this.x);
  V operator +(V other) => V(x + other.x);
  @override
  String toString() => 'V($x)';
}

void main() {
  final a = V(1);
  final b = V(2);
  print(a + b);
}
```

---

## Collections & Operators

### Question 4 — spread operator with null-aware

```dart
void main() {
  final a = [1, 2];
  final List<int>? b = null;
  final res = [...a, ...?b, 3];
  print(res);
}
```

---

### Question 5 — `identical` vs `==`

```dart
class X {
  final int v;
  X(this.v);
  @override
  bool operator ==(Object o) => o is X && o.v == v;
  @override
  int get hashCode => v.hashCode;
}

void main() {
  final a = X(1000);
  final b = X(1000);
  print(a == b);
  print(identical(a, b));
}
```

---

## Async & Event Loop

### Question 6 — async ordering

```dart
import 'dart:async';

void main() {
  print('start');
  Future(() => print('future')); // schedules microtask-like with Event queue
  scheduleMicrotask(() => print('microtask'));
  print('end');
}
```

---

## Algorithms & Recursion

### Question 7 — BigInt and factorial-ish

```dart
import 'dart:math';

BigInt factorial(int n) {
  BigInt r = BigInt.one;
  for (var i = 2; i <= n; i++) r *= BigInt.from(i);
  return r;
}

void main() {
  print(factorial(5));
}
```

---

### Question 8 — const canonicalization and identical

```dart
void main() {
  const a = 'hello';
  const b = 'he' + 'llo';
  print(identical(a, b));
}
```

---

### Question 9 — map merging and duplicate keys order

```dart
void main() {
  final a = {'x': 1, 'y': 2};
  final b = {'y': 20, 'z': 3};
  final merged = {...a, ...b};
  print(merged);
}
```

---

### Question 10 — tricky closure capture in loops

```dart
void main() {
  final fns = <Function>[];
  for (int i = 0; i < 3; i++) {
    fns.add(() => print(i));
  }
  fns.forEach((f) => f());
}
```

---

### Question 11 — cascade vs method chaining

```dart
class Builder {
  int a = 0;
  Builder setA(int v) { a = v; return this; }
  void printA() => print(a);
}

void main() {
  final b = Builder()..setA(1)..setA(2);
  b.printA();
  final c = Builder().setA(1).setA(2);
  c.printA();
}
```

---

File purpose: quick output-only Dart code questions for interview screens. Provide only the outputs for each snippet.

---

### Question 12 — polymorphism: overridden methods

```dart
abstract class Animal {
  String speak() => '...';
}

class Dog extends Animal {
  @override
  String speak() => 'woof';
}

class Cat extends Animal {
  @override
  String speak() => 'meow';
}

void main() {
  final List<Animal> animals = [Dog(), Cat()];
  animals.forEach((a) => print(a.speak()));
}
```

---

### Question 13 — recursion: fibonacci (recursive)

```dart
int fib(int n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}

void main() {
  print(fib(6));
}
```

---

### Question 14 — recursion with prints (order)

```dart
void recurse(int n) {
  if (n == 0) return;
  print(n);
  recurse(n - 1);
  print(n);
}

void main() => recurse(2);
}
```

---

## Widget Lifecycle

### Question 15 — StatefulWidget lifecycle order

```dart
import 'package:flutter/material.dart';

class LifecycleDemo extends StatefulWidget {
  @override
  State<LifecycleDemo> createState() {
    print('createState');
    return _LifecycleDemoState();
  }
}

class _LifecycleDemoState extends State<LifecycleDemo> {
  @override
  void initState() {
    super.initState();
    print('initState');
  }

  @override
  Widget build(BuildContext context) {
    print('build');
    return Container();
  }

  @override
  void didUpdateWidget(LifecycleDemo old) {
    super.didUpdateWidget(old);
    print('didUpdateWidget');
  }

  @override
  void dispose() {
    print('dispose');
    super.dispose();
  }
}

void main() {
  runApp(MaterialApp(home: LifecycleDemo()));
}
```

Note: Assume the widget is mounted once, built, and then disposed. What prints in order?

---

## Advanced Async Patterns

### Question 16 — Future.wait vs sequential await

```dart
import 'dart:async';

Future<void> task(String name, int ms) async {
  print('$name start');
  await Future.delayed(Duration(milliseconds: ms));
  print('$name end');
}

void main() async {
  print('begin');
  await Future.wait([task('A', 10), task('B', 5)]);
  print('done');
}
```

Note: What is the order of prints (assume tasks run concurrently)?

---

### Question 17 — microtask vs event queue ordering

```dart
import 'dart:async';

void main() {
  print('1');
  scheduleMicrotask(() => print('microtask1'));
  Future(() => print('future1'));
  scheduleMicrotask(() => print('microtask2'));
  Future(() => print('future2'));
  print('2');
}
```

---

### Question 18 — then chaining and execution order

```dart
import 'dart:async';

void main() {
  print('start');
  Future(() {
    print('future');
    return 42;
  }).then((val) {
    print('then: $val');
  });
  print('end');
}
```

---

### Question 19 — async/await with synchronous code

```dart
void main() async {
  print('A');
  await Future.value();
  print('B');
  print('C');
}
```

---

### Question 20 — nested Future.then and microtask

```dart
import 'dart:async';

void main() {
  Future(() => print('F1'))
    .then((_) => scheduleMicrotask(() => print('M1')))
    .then((_) => print('T1'));
  scheduleMicrotask(() => print('M2'));
  print('sync');
}
```

---

### Question 21 — App lifecycle state transitions

```dart
import 'package:flutter/material.dart';

class LifecycleObserver extends WidgetsBindingObserver {
  @override
  void didChangeAppLifecycleState(AppLifecycleState state) {
    print('Lifecycle: ${state.name}');
  }
}

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  final observer = LifecycleObserver();
  WidgetsBinding.instance.addObserver(observer);

  // Simulate state transitions (in order):
  // 1. App starts (resumed)
  // 2. User switches away (inactive → paused)
  // 3. User returns (resumed)
  // 4. App sent to background for a long time (detached)

  // What prints in each transition?
}
```

Note: State the lifecycle state names printed when transitioning from:

- App launch → active use → backgrounded → returned to foreground → fully detached.
  Expected sequence: resumed → inactive → paused → resumed → detached

---
