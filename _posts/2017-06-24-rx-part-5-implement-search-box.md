---
layout: post
title: "Rx part 5: Debounce, throttle and Implement search box"
categories: reactive
comments: true
image: "https://cdn.css-tricks.com/wp-content/uploads/2016/04/debounce-leading.png"
tags: [android, java, kotlin, swift, rx]
---

## Debounce
Only emit an item from an Observable if a particular timespan has passed without it emitting another item.

**For example:** When the last event is emitted, It have to wait a period of time to forward the event. If the new event is emitted too quick, the old event will be ignored and not be forwarded.
<!--more-->
<img class="post-image" src="http://reactivex.io/documentation/operators/images/debounce.png" alt="Debounce"/>
## Throttle
Only forward an event once the source observable has stopped sending events for the specified period of time.

**For example:** In every 1s, the last event will be forward.

<img class="post-image" src="https://camo.githubusercontent.com/95e62a00ba13ce12ab42f6d41431d5afb3b26e76/68747470733a2f2f7261772e6769746875622e636f6d2f77696b692f5265616374697665582f52784a6176612f696d616765732f72782d6f70657261746f72732f7468726f74746c654c6173742e706e67" alt="ThrottleLast"/>

## Implement search box

To implement Rx, we need to emit the String in search box whenever the text in search box is changed. To do it, we use [RxBinding](https://github.com/JakeWharton/RxBinding) for Android and [RxCocoa](https://github.com/ReactiveX/RxSwift) for iOS. It will help you emit text from search box (text field) when ever the text is changed.

Then, we use **throttle or debounce** to not emit too much event when user type so fast. Then, use **flatMapLatest or switchMap** to request to server. The response will be handle in onNext() and error will be handle in onError() of Observer.

For Android:

```java
//using RxBinding
RxTextView.textChanges(editText)
		.throttleLast(300, TimeUnit.MILLISECONDS)// you can use debounce instead
		.switchMap { fetchData(it) }
		.subscribe({ println(it) })
```

For iOS:

```swift
//using RxCocoa
uitextField.rx.text.asObservable()
            .throttle(0.3, scheduler: MainScheduler.instance)
            .flatMapLatest { self.fetchData(key: $0) }
            .catchErrorJustReturn("")
            .subscribe(onNext: {
                print($0)
            }).addDisposableTo(DisposeBag())
        
```