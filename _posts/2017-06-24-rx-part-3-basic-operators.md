---
layout: post
title: "ReactiveX: Basic Operators"
categories: reactive
image: "https://cdn-images-1.medium.com/max/2000/1*iVjIzql9k7PpwEUdb0phwQ.png"
tags: [android, java, kotlin, swift, rx]
---

## Filter
Emit only those items from an Observable that pass a predicate test

The Filter operator filters an Observable by only allowing items through that pass a test that you specify in the form of a predicate function.

<!--more-->

<img class="post-image" src="https://qiita-image-store.s3.amazonaws.com/0/59803/ea70f5e1-567b-75cf-6043-71fd27d7387e.png" alt="Filter"/>

Example:

```java
//filter even number
Observable.just(1,2,3,4,5,6).filter { it % 2 == 0 } // == Observable.just(2,4,6)
```

## Map
Transform the items emitted by an Observable by applying a function to each item

<img class="post-image" src="https://i.stack.imgur.com/P6C2t.png" alt="Map"/>

**Example:**

```
Observable.just(1,2,3,4,5,6).map { it * 2 } //RxKotlin
Observable.from([1,2,3,4,5,6]).map{ $0 * 2 } //RxSwift

-> It's similar to: Observable.just(2,4,6,8,10,12)
```
It's work perfectly when using with a list single object. But if we have a list of array and you want the result is similar to above example, for example if we have a list of array: 

```
arrayOf(arrayOf(1,2,3), arrayOf(4,5,6)) //Kotlin
[[1,2,3],[4,5,6]] //Swift
```

When you using map operator for this list, you can only transform Array -> Array. So, you can't multiple to 2 for each Number. In this case, instead of map, we use flatMap.
## FlatMap
Transform the items emitted by an Observable into Observables, then flatten the

<img class="post-image" src="http://reactivex.io/documentation/operators/images/flatMap.c.png" alt="Flatmap"/>

RxSwift

```swift
Observable.from([[1,2,3],[4,5,6]]).flatMap({ // if using map, you must return array
    return Observable.from($0).map{$0*2}
})
 --> Similar to: Observable.just(1,2,3,4,5,6).map{ $0 * 2 }
```

RxJava/RxKotlin

```kotlin
Observable.just(arrayOf(1,2), arrayOf(3,4))
.flatMap { Observable.just(it).map { it * 2 } }
```

Inside flatmap, you have to return an Observable instead of Object.


## SwitchMap & FlatMapLatest
SwitchMap in RxJava very similar to FlatMapLatest in RxSwift.
I have an example: I have an SearchBox and using Rx to handle text change event of the SearchBox. Whenever a new char is appended to the box, the observable emit an item(current string in box). Each item received, I using flatMap to request to server and get response. 

Suppose that the items emit every 1s and each request spend 5s to finish. So

1. Observable emit 5 item in 5s 
2. flatMap create 5 request to server and then receive 5 response

But we only need the respone of last item. So, 4 previous request is not need and make application consume more resource (CPU, Network).

That's why SwitchMap/FlatMapLatest was born. If It receive a new item while processing other item, the older item will be cancel and execute newer item.

<img class="post-image" src="https://i.stack.imgur.com/Tn8KA.png" alt="switchMap" />

Example:

```
//RxSwift
Observable.from(1,2,3,4,5,6).flatMapLatest({ fetchData($0) })

//RxKotlin/RxJava
Observable.just(1,2,3,4).switchMap { fetchData(it) }
```
