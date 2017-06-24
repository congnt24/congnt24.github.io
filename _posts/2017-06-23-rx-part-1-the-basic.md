---
layout: post
title: "Rx part 1: Introduction to Rx"
categories: reactive
---

## 1. What is Rx
Rx is stand for Reactive X. It mean you're ready for an event and when the event happend, you'll react to the event. 

**For example:** In an Chat application, when you send a message you need to do two thing: Save it to local DB and send the request to server.

- **The traditional approach:** You need to call a function for saving data to DB and other function for sending message to server.

- **RX approach:** You only need to call a function to save data in DB. Because we have a subscriber alway listen to the change of publisher(DB), when data in DB is changed, the subscriber will REACT to the change and send message to server.

Reactive X is the combine of **Observable Pattern** and **Iterator Pattern**:

Why Observable Pattern? because It use the concept of Observable, It will have a observable(publisher) and many observer(subscriber) -> It's similar to subscribing on youtube channel.

Why Iterator Pattern? because observable(publisher) emit(publish) an event(object) whenever it has. It's similar to loop for an array.


Now you known exactly what is reactive x and how it work. But you will wonder why you need to learn RX, why RX is better then traditional approach, and what is the advantage and disadvantage of RX. You will see it in the next sextion and you will love it after going through all the article of this serial.

## 2. Why Rx?
Nowaday, ReactiveX become so popular in many programing language suck as: RxJava for Java and Android development, RxSwift, RxJs...
Using Rx help you control your flow/code easier, your code will be more concise, help you create realtime application and applying MVVM architechture.

For example: when you need to execute an async call like: network call

Traditional(Android): Using Asynctask

```java
public class SampleTask extends AsyncTask<Void,Void,List<Users>> {
    private final SampleAdapter mAdapter;

    public SampleTask(SampleAdapter sampleAdapter) {
        mAdapter = sampleAdapater;
    }

    @Override
    protected List<Users> doInBackground(Void... voids) {
    //Call API here
        return fetchUsers();
    }

    @Override
    protected void onPostExecute(List<Users> users) {
        super.onPostExecute(products);
        //Receive data and update UI here
    }
}
```

RxJava: concise, easy to use and understand

```java
fetchUsers()        
		.subscribeOn(Schedulers.io())	//use io thread
        .observeOn(AndroidSchedulers.mainThread()) //use main thread for update ui
        .subscribe(new Subscriber<User>() {
                       @Override
                       public void onCompleted() {
                       }

                       @Override
                       public void onError(Throwable e) {
                           //Handle error
                       }

                       @Override
                       public void onNext(User user) {
                           //Handle success
                       }
                   }
        );
```

This article just an introduce about RX. There are many more useful feature in ReactiveX that will help you a lot in develop application. I will show you in the next article.

Thank your for reading.