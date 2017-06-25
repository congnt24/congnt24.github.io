---
layout: post
title: "ReactiveX: Observable and Observer"
categories: reactive
image: "https://farm5.staticflickr.com/4280/35347983512_8a315ffeea_o_d.jpg"
tags: [android, java, kotlin, swift, rx]
---

Observer and Observable are the main objects in RX.
## Observable
Observable like a publisher, It's role is to push data/event to all it's subscriber(observer). RX support 3 type of event to push: 

- onNext(data) for emitting data.
- onError(error) for emmiting error event.
- onComplete() for emmiting complete function with no data. 

<!--more-->
## Observer
Observer like a subscriber, It will listen for the event that publisher(observable) pushed. Your observer have to implement onNext, onError and onComplete event. So, whenever it receive an event from observable, it will execute some code compatible with your observable implementation.

- In RxJava

```java
// Observable emit numbers (1->6), Observer receive numbers in onNext().
// After receive all number, onComplete will be call.
// You can use lambda expression for concise code by using java8 or retrolambda

Observable.just(1,2,3,4,5,6).map(new Function<Integer, Integer>() {
            @Override
            public Integer apply(Integer integer) throws Exception {
                return integer * 2;
            }
        }).subscribe(new Observer<Integer>() {
            @Override
            public void onSubscribe(Disposable d) {

            }

            @Override
            public void onNext(Integer integer) {
                System.out.println(integer);
            }

            @Override
            public void onError(Throwable e) {

            }

            @Override
            public void onComplete() {
                System.out.println("complete");
            }
        });
```
- In RxKotlin

```kotlin
//it stand for iterator, if function have only 1 param, you can use it instead of declare a param name.
val disposable = Observable.just(1,2,3,4,5,6).map { x -> x * 2 }.subscribeBy(
                onNext = { println(it) },
                onError = { println(it.message) },
                onComplete = { println("complete")})
//disposable.dispose() // call this when you don't need to subscribe
```

- In RxSwift

```swift
//$0 stand for the 1st param
let disposable: Disposable = Observable.from([1,2,3,4,5,6]).map{ $0 * 2 }.subscribe(
    onNext: { print($0) },
    onError: { print($0) },
    onCompleted: { print("complete") })
```
The result for 3 statement above is:
```
2
4
6
8
10
12
complete
```

After subscribe an observable, it will return an **Disposable** and we will use this for unsubscribe when system deallocate.
