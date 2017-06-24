---
layout: post
title: "Rx part 4: Creat Observable"
categories: reactive
---

## 1. Observable.just(element)
In RxJava, Observable.just() will emit each parameters. The max params is 10

```
Observable.just(1,2,3) // Will emit onNext(1), onNext(2), onNext(3), onComplete()
```

In RxSwift, Observable.just() only emit one time

```
Onservable.just(1,2,3) // Will emit onNext(1,2,3) and onComplete()
```

## 2. Observable.from(array)
Using it when you have an array and you want to emit each item in the array

```
Observale.from([1,2,3]) // Will emit onNext(1), onNext(2), onNext(3), onComplete()
```
## 3. Observable.create

## 4. Observable.interval
create an Observable that emits a sequence of integers spaced by a given time interval
## 5. Observable.defer
do not create the Observable until a Subscriber subscribes; create a fresh Observable on each subscription

## 6. [Other](https://github.com/ReactiveX/RxJava/wiki/Creating-Observables)
