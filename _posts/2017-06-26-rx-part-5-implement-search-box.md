---
layout: post
title: "Rx part 5: Debounce, throttle and Implement search box"
categories: reactive
---

## 1. Debounce
Only emit an item from an Observable if a particular timespan has passed without it emitting another item

![Debounce](http://reactivex.io/documentation/operators/images/debounce.png)
## 2. Throttle

![ThrottleLast](https://camo.githubusercontent.com/95e62a00ba13ce12ab42f6d41431d5afb3b26e76/68747470733a2f2f7261772e6769746875622e636f6d2f77696b692f5265616374697665582f52784a6176612f696d616765732f72782d6f70657261746f72732f7468726f74746c654c6173742e706e67)

## 3. Implement search box

```java
//using RxBinding
RxTextView.textChanges(editText)
		.throttleLast(300, TimeUnit.MILLISECONDS)
       .switchMap { fetchData(it) }
       .subscribe({ println(it) })
```

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