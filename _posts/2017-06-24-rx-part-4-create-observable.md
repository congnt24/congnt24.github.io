---
layout: post
title: "ReactiveX: Create Observable"
categories: reactive
image: "https://farm5.staticflickr.com/4280/35347983512_8a315ffeea_o_d.jpg"
tags: [android, java, kotlin, swift, rx]
---

## Observable.just(element)
In RxJava, Observable.just() will emit each parameters. The max params is 10

```java
Observable.just(1,2,3) // Will emit onNext(1), onNext(2), onNext(3), onComplete()
```

In RxSwift, Observable.just() only emit one time

```swift
Onservable.just(1,2,3) // Will emit onNext(1,2,3) and onComplete()
```
<!--more-->
## Observable.from(array)
Using it when you have an array and you want to emit each item in the array

```swift
Observale.from([1,2,3]) // Will emit onNext(1), onNext(2), onNext(3), onComplete()
```

## Observable.interval
Create an Observable that emits a sequence of integers spaced by a given time interval.
The below statement will emit data each 1s.

RxKotlin

```java
Observable.interval(0, 1000, TimeUnit.MILLISECONDS)
	.subscribe( { println("Hello world") }) // == onNext
Thread.sleep(10000)
```

RxSwift

```swift
Observable<Int>.interval(1, scheduler: MainScheduler.instance)
    .subscribe(onNext: { i in print("Hello world") })
```

## Observable.defer
Do not create the Observable until a Subscriber subscribes; create a fresh Observable on each subscription


<img class="post-image" src="http://reactivex.io/documentation/operators/images/defer.c.png" alt="Defer"/>

The Defer operator waits until an observer subscribes to it, and then it generates an Observable, typically with an Observable factory function. It does this afresh for each subscriber, so although each subscriber may think it is subscribing to the same Observable, in fact each subscriber gets its own individual sequence.

In some circumstances, waiting until the last minute (that is, until subscription time) to generate the Observable can ensure that this Observable contains the freshest data.

Example:

RxKotlin / RxJava

```kotlin
val ob = Observable.defer { Observable.just(1, 2, 3) }
ob.subscribe({ println(it) }) // emit onNext(1), onNext(2), onNext(3), onComplete() 
ob.subscribe({ println(it) }) // emit onNext(1), onNext(2), onNext(3), onComplete()
```

RxSwift

```swift
let ob = Observable.deferred { Observable.from([1,2,3]) }
ob.subscribe(onNext: { print($0) })
ob.subscribe(onNext: { print($0) })

```

## Observable.create
You can create an Observable from scratch by using the Create operator. You pass this operator a function that accepts the observer as its parameter. Write this function so that it behaves as an Observable — by calling the observer’s onNext, onError, and onCompleted methods appropriately.

A well-formed finite Observable must attempt to call either the observer’s onCompleted method exactly once or its onError method exactly once, and must not thereafter attempt to call any of the observer’s other methods.

**Example:**

In RxJava / RxKotlin

```kotlin
val observable = Observable.create<Int> {
			  doSomething()
            it.onNext(1)
            it.onNext(2)
            it.onNext(3)
            it.onError(IllegalArgumentException())
            it.onComplete()
        }
observable.subscribe({ println(it) })

```

In RxSwift is similar.


## Other
Here are some other way to create observable:

- [Observable.never()](http://reactivex.io/RxJava/javadoc/rx/Observable.html#never())
- [Observable.empty()](http://reactivex.io/RxJava/javadoc/rx/Observable.html#empty())
- [Observable.range()](http://reactivex.io/RxJava/javadoc/rx/Observable.html#range(int,%20int))
- [Observable.timer()](http://reactivex.io/RxJava/javadoc/rx/Observable.html#timer(long,%20java.util.concurrent.TimeUnit))
- [Observable.error()](http://reactivex.io/RxJava/javadoc/rx/Observable.html#error(java.lang.Throwable))
