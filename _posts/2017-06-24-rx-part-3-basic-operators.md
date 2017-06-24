---
layout: post
title: "Rx part 3: Basic Operators"
categories: reactive
---

## 1. Map
Transform the items emitted by an Observable by applying a function to each item
![Map](https://i.stack.imgur.com/P6C2t.png)

Example:

```
Observable.just(1,2,3,4,5,6).map { it * 2 } //RxKotlin
Observable.from([1,2,3,4,5,6]).map{ $0 * 2 } //RxSwift

-> It's similar to: Observable.just(2,4,6,8,10,12)
```
It's work perfectly when using with a list single object. But if we have a list of array and you want the result is similar to above example, for example if we have a list of array: 

```
{{1,2,3}, {4,5,6}} //Java
[[1,2,3],[4,5,6]] //Swift
```

When you using map operator for this list, you can only transform Array -> Array. So, you can't multiple to 2 for each Number. In this case, instead of map, we use flatMap.
## 2. FlatMap
Transform the items emitted by an Observable into Observables, then flatten the emissions from those into a single Observable
![FlatMap](http://reactivex.io/documentation/operators/images/flatMap.c.png)

```swift
Observable.from([[1,2,3],[4,5,6]]).flatMap({
    return Observable.from($0).map{$0*2}
}).subscribe(
    onNext: { print($0) },
    onError: { print($0) },
    onCompleted: { print("complete") })
```


## 3. SwitchMap & FlatMapLatest
![SwitchMap](https://i.stack.imgur.com/Tn8KA.png)

