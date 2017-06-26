---
layout: post
title: "ReactiveX: RxBus and Subject in Rx"
categories: reactive
image: "https://raw.githubusercontent.com/greenrobot/EventBus/master/EventBus-Publish-Subscribe.png"
tags: [android, java, kotlin, swift, rx]
---

## Publist Subject
PublishSubject emits to an observer only those items that are emitted by the source Observable(s) subsequent to the time of the subscription.
That mean items were emitted before subscribe won't be emitted.

Example:

```java
//In Kotlin
val ps = PublishSubject.create<Int>()
ps.subscribe({ println(it) })
ps.onNext(1)
ps.onNext(2)
ps.onNext(3)
// result is: 1 2 3
```

<!--more-->

```swift
//In Swift
let ps = PublishSubject<Int>()
ps.subscribe(onNext: {
    print($0)
})
ps.onNext(1)
ps.onNext(2)
ps.onNext(3)
```

<img class="post-image" src="http://reactivex.io/documentation/operators/images/S.PublishSubject.png" alt="publish subject"/>
## Behavior Subject
When an observer subscribes to a BehaviorSubject, it begins by emitting the item most recently emitted by the source Observable (or a seed/default value if none has yet been emitted) and then continues to emit any other items emitted later by the source Observable(s).

However, if the source Observable terminates with an error, the BehaviorSubject will not emit any items to subsequent observers, but will simply pass along the error notification from the source Observable.

<img class="post-image" src="http://reactivex.io/documentation/operators/images/S.BehaviorSubject.png" alt="behavior subject"/>

Example:

```swift
let ps = BehaviorSubject<Int>(value: 1)
ps.subscribe(onNext: {
    print($0)
})
ps.onNext(2)
ps.onNext(3)
//3 will be emit
ps.subscribe(onNext: {
    print($0)
})
//result is: 1 2 3 3
```

## RelaySbject

ReplaySubject<T> provides the feature of caching values and then replaying them for any late subscriptions. It's similar to BehaviorSubject but BehaviorSubject only cache last event and must have initial value.

<img class="post-image" src="http://reactivex.io/documentation/operators/images/S.ReplaySubject.png" alt="relay subject"/>

By default, RelaySubject will cache all the items had emitted. This could create unnecessary memory pressure on the application. RelaySubject allow you to control this memory issue by set timeout (be removed after period of time) for each event or set buffer size (max event cached).

Example:

```java
//In Kotlin
val rs = ReplaySubject.createWithTime<Int>(1000, TimeUnit.MILLISECONDS, Schedulers.io())
rs.onNext(1)
rs.onNext(2)
Thread.sleep(1000)
rs.onNext(3)
rs.subscribe({println(it)})
// result id: 3
```


```swift
//In Swift
let rs = ReplaySubject<Int>.create(bufferSize: 2)
rs.onNext(1)
rs.onNext(2)
rs.onNext(3)
rs.subscribe(onNext: {
    print($0)
})
//result id: 3
```

## AsyncSubject

AsyncSubject<T> is similar to the Replay and Behavior subjects in the way that it caches values, however it will only store the last value, and only publish it when the sequence is completed. The general usage of the AsyncSubject<T> is to only ever publish one value then immediately complete.

<img class="post-image" src="http://reactivex.io/documentation/operators/images/S.AsyncSubject.png" alt="async subject"/>
## RxBus

In android development, when you need to pass data from an activity to other activity or service, fragment... you should use EventBus(3rd library) instead of using Binder, Bundle...

But, you can create your own eventbus using Rx by using PublishSubject.

```java
//In Kotlin
object RxBus {
    private var mapBus: MutableMap<String, Any> = HashMap()

    fun <E: Any> events(type: KClass<E>) : Observable<E> {
        if (mapBus[type.javaObjectType.name] == null) {
            mapBus[type.javaObjectType.name] = PublishSubject.create<E>()
        }
        return (mapBus[type.javaObjectType.name] as PublishSubject<E>).ofType(type.javaObjectType).share()
    }

    fun <E: Any> post(event: E){
        (mapBus[event.javaClass.name] as PublishSubject<E>).onNext(event)
    }

    fun <E: Any> release(type: KClass<E>){
        mapBus.forEach{
            if (it.key.equals(type.javaObjectType.name)){
                (it.value as PublishSubject<Any>).onComplete()
                return
            }
        }
    }

    fun release(){
        mapBus.forEach { (it.value as PublishSubject<Any>).onComplete() }
    }
}
```