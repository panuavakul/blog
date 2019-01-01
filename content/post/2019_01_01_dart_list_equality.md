---
title: "Dart's List Equality"
date: 2019-01-01T23:40:00+09:00
tags: [dart, flutter]
categories: [note, howto, programming]
---

I was implementing some `==` operator overload in Dart today and made quite a mistake, which caused me a bunch of debugging time so I might as well note it here.

So let's say we have two lists in something like `Swift` and we do some comparison with `==` operator.

```swift
let a = [1,2,3,4,5,6]
let b = [1,2,3,4,5,6]
a == b // true
```

However, in `Dart` it's seems that list are compared with `identity` (which if I understand correctly is checking if the reference is the same) and not `equality`.

```dart
final a = [1,2,3,4,5,6];
final b = [1,2,3,4,5,6];
a == b // false
```

We can also check the `hashCode` to see what happen between this two list.

```dart
a.hashCode  // 57191515
b.hashCooe  // 264245813
```

Hence, `==` return a `false`. This is cause a problem when overloading an `==`, such as below would not work.

```dart
class A {
    A({
        @required this.name,
        @required this.values,
    });

    final String name;
    final List<int> values;

    @override
    bool operator ==(dynamic other) {
        return identical(this, other) ||
            other.runtimeType == runtimeType &&
                other.name == name &&
                other.values == values // <---- This will not work
    }
}
```

 The solution is to check deep equality of the list. It's possible to just write your own function to loop through and check all values of the list, but Flutter provide a `listEquals(a, b)`, fuction (the source code is just looping to check all values for equality) for us to use. So we can just do

```dart
/// Import this to get the listEquals
import 'package:flutter/foundation.dart';

class A {
    A({
        @required this.name,
        @required this.values,
    });

    final String name;
    final List<int> values;

    @override
    bool operator ==(dynamic other) {
        return identical(this, other) ||
            other.runtimeType == runtimeType &&
                other.name == name &&
                listEquals(other.values, values) // <---- This now should work
    }
}
```

and we should be able to compare `A` with `==` operator now.