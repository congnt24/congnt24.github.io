---
layout: post
title: "ReactiveX: RxBus and Subject in Rx"
categories: reactive
image: "https://raw.githubusercontent.com/greenrobot/EventBus/master/EventBus-Publish-Subscribe.png"
tags: [android, java, kotlin, swift, rx]
---

## Publist Subject
<img class="post-image" src="http://reactivex.io/documentation/operators/images/S.PublishSubject.png" alt="publish subject"/>
## Behavior Subject
<img class="post-image" src="http://reactivex.io/documentation/operators/images/S.BehaviorSubject.png" alt="behavior subject"/>
## RelaySbject
<img class="post-image" src="http://reactivex.io/documentation/operators/images/S.ReplaySubject.png" alt="relay subject"/>
## AsyncSubject
<img class="post-image" src="http://reactivex.io/documentation/operators/images/S.AsyncSubject.png" alt="async subject"/>
## RxBus

In Kotlin

```java
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