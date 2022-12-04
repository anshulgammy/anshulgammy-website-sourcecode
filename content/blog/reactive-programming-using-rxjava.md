---
title: "Reactive Programming using RxJava"
date: 2022-03-25T18:19:05+05:30
draft: false
categories: [tech, blog]
tags: [reactive, programming, java, rxjava]
description: "Learn basics of Reactive Programming using RxJava with simpler examples"
author: "Anshul Gautam"
---

![Reactive Programming using RxJava Banner](/blog/reactive-programming-using-rxjava.jpg "Reactive Programming using RxJava")

## 🎯 What is RxJava?

The official documentation defines it as:
> It is a Java VM implementation of Reactive Extensions. Reactive Extensions is a library for composing asynchronous and event-based programs by using observable sequences.

In simpler terms, **RxJava** incorporates **Reactive Programming** paradigm in **Java**, and helps you consume data in motion, **asynchronously**.

You don't need to scan through the data in motion(**pull based blocking calls**), in fact the each data element in the motion will come to you(**push based non-blocking calls**). Now as the data keeps coming to you, you can work with it the way you want.

## 🎯 But why do we need Reactive Programming in Java?

Let's consider an example of an ECommerce website where user purchases some items from the website, and after the payment receives a notification in form of email and sms. Here, we have two services present:
- Payment Service
- Notification Service

Without reactive programming, we would be coding for this requirement something like below:


```
public class PaymentService {
    /
    .
    .
    .
    /

    public void success(User user) {
        notificationService.sendEmail(user);
        notificationService.sendSms(user);
    }
}
``` 
In this approach, you can see that PaymentService is behaving pro-actively, and NotificationService is sort of behaving irresponsible. Why is that so?

This is so because PaymentService is doing more than what it should do.
It is processing the payment, and also triggering the notification. NotificationService is waiting for PaymentService to trigger it. It is not doing anything on its own.
Isn't it violation of Single Responsibility Principle?

With Reactive Programming, we make both of the service responsible, reactive to events, and make them work for what they are intended to. They do their job, and then raise an event marking success or failure at their end. Based on the event raised, other services would start their work.

The implementation with Reactive Programming will look something like below.

```
public class PaymentService {
    /
    .
    .
    .
    /

    public void success(User user) {
        Message message = new Message("Payment Done", user);

        broadcastService.broadcast(message);
    }
}

public class NotificationService {
    /
    .
    .
    .
    /

    public void listen(Message message) {
        sendEmail(message.getUser());
        sendSms(message.getUser());
    }
}
``` 
I think the above code snippets give you an idea on how Reactive Programming helps us. This is not the complete code, but at least it gives you sense of Reactive Programming.

So, in simpler terms, we can say that Reactive Programming is programming with asynchronous data streams.

But wait, Java has in built support for Future and CompletableFuture. Right?

Reactive Programming is by default asynchronous. It gives you DSL to make your life easier, allowing you to write too little code to make your logic work asynchronously. The amount of code that you will write with vanilla Java Future, and the complexity involved in it, these all get over simplified when you use Reactive Programming with RxJava.

RxJava is by default single threaded. It has a component called Scheduler, which helps in scheduling of work to different threads.

## 🎯 Components in RxJava

In RxJava, few important concepts involved here are: **Observable**, **Observer**, **Scheduler** and **Operator**.

**Observable** -> (**pushes data**) -> **Observer**

- **Observable** is the entity which pushes/emits the data stream towards the Observer.

- **Observer** keeps listening to the Observable. It is the entity which will consume the data elements, and work with them or take any action.

- **Operator** is the entity which can transform, merge, split or process the data pushed by the Observable before it reaches Observer. It provides an elegant declarative solution to complex asynchronous tasks. You can chain different operators together.

- **Scheduler** manage threads. It is the component in Rx that tells observable and observers on which thread they should run.

Like I mentioned before, RxJava is by default works on single threaded model. This doesn't mean that every task in your application is going to run on a single thread only. 

For example consider that in an application there is a heavy resource intensive task, and for that a heavy thread is needed, call it *Thread-Elephant*. Along with this, there are several light weight tasks, which can safely run on a lighter thread, call it *Thread-Feather*. 

Scheduler is the component in RxJava which can switch between threads, and schedule the heavy task to be computed on the *Thread-Elephant*, and can switch to *Thread-Feather* for other light weight tasks in the application. By doing this, the main thread never gets blocked.

## 🎯 Integrating RxJava in Java Applications

Maven and Gradle projects can make use of dependency which enables the RxJava support for your Java applications.

Below dependency needs to be added in your maven project.


```
<dependency>
    <groupId>io.reactivex.rxjava2</groupId>
    <artifactId>rxjava</artifactId>
    <version>2.2.21</version>
</dependency>
``` 
While working with RxJava, it is advisable to have knowledge on Java Lambdas and Method Reference concepts. It makes life easier.

## 🎯 Let's play with RxJava

Let's create a very simple Observable.

```
/**
 * Creating Observable from '*just*', and subscribing to it.
 */
private static void observableFromJust() {
    Observable.just(1, 2, 3, 4, 5)
            .subscribe(HelloRxJava::print);
}
``` 
The Observable is created here using '*just*', and it emits integers. Our Observer subscribes to it, and prints integers emitted by the Observable.

The most frequently way to create Observable is by using '*create*'.

```
/**
 * Creating Observable from ObservableOnSubscribe<T> using create, and subscribing to it.
 */
private static void observableFromCreate() {
    Observable<Integer> observable = Observable.create(observableEmitter -> {
        observableEmitter.onNext(1);
        observableEmitter.onNext(2);
        observableEmitter.onNext(3);
        observableEmitter.onNext(4);
        observableEmitter.onNext(5);
    });
    observable.subscribe(System.out::println);
}
``` 
You can create Observables from Iterable, Callable and Future also, using self explanatory methods like *Observable.fromIterable()*, *Observable.fromCallable()* and *Observable.fromFuture()*.

Okay, now let's try to employ Operators on our Observable. 

Like I mentioned, Operator will help you do transformation to your data elements in the stream of data being emitted. 

I have used *filter()* to do the filter operation, and then *map()* to transform the elements.
One more thing you will notice in the below example is that I have used *Observable.range()* to create Observable to emit integers from 1 to 10; even more simpler way to create Observable.
Well, this creates Observable of Integers present in the range defined.

```
Observable<Integer> observable = Observable.range(1, 10);

observable
    // filtering the element on the basis of odd and even. Only even numbers will be considered.
    .filter(element -> element % 2 == 0)
    // multiplying the element with 10
    .map(element -> element * 10)
    .subscribe(System.out::println);
``` 
Let's work around Schedulers now. Like I mentioned, Scheduler in RxJava helps to switch between threads.

In the example code below, I am trying to find out which thread our subscriber is working on. This example shows that by default, all the operation is going on in the main thread.

```
Observable<Integer> observable = Observable.range(1, 10);

observable
    // filtering the element on the basis of odd and even. Only even numbers will be considered.
    .filter(element -> element % 2 == 0)
    // multiplying the element with 10
    .map(element -> element * 10)
    .subscribe(element -> {
        System.out.println(Thread.currentThread().getName());
        print(element);
    });
``` 
Output:

```
main
Printing number: 20
main
Printing number: 40
main
Printing number: 60
main
Printing number: 80
main
Printing number: 100
``` 
Let's try to switch to a different thread now using *observeOn()* and static methods from Schedulers class.

```
Observable.range(1, 10)
    .filter(element -> element % 2 == 0)
    .map(element -> element * 10)
    .observeOn(Schedulers.computation())
    .subscribe(element -> {
        System.out.println(element);
        System.out.println(Thread.currentThread().getName());
    });
``` 
Output:

```
20
RxComputationThreadPool-1
40
RxComputationThreadPool-1
60
RxComputationThreadPool-1
80
RxComputationThreadPool-1
100
RxComputationThreadPool-1

Process finished with exit code 0
``` 

We also have *subscribeOn()* operator available as well which helps to switch to a different thread. 

The difference between *observeOn()* and *subscribeOn()* is that the former makes sure that chained operators till the point where you have used *observeOn()* will work on the separate thread. With *subscribeOn()*, the entire observable works on a separate thread.

This is really helpful when we have lots of operators chained and we want to segregate the work load on different threads.

One important thing to note. In Java, if you try running the code like above which executes on a separate thread, you might not see the expected output. The reason is, the Java main thread just exists. You will need to provide something to stop the main thread from exiting. I used a very simple *Thread.sleep()* to check the output.

```
Observable.range(1, 10)
    .filter(element -> element % 2 == 0)
    .map(element -> element * 10)
    .observeOn(Schedulers.computation())
    .subscribe(element -> {
        System.out.println(element);
        System.out.println(Thread.currentThread().getName());
    });

try {
    Thread.sleep(2000);
} catch (InterruptedException e) {
    e.printStackTrace();
}
``` 
## 🎯 Error Handling in RxJava

You can handle error at the observer/subscribe level, or at the operator level. 

In the below example I have shown handling error when I am doing the subscribe. You can see below that there is one null getting emitted by the Observable.

Observable doesn't allow to emit null, since RxJava 2.x. I have added that on purpose so that Exception gets raised.

```
Observable<Integer> observable = Observable.create(emitter -> {
    emitter.onNext(1);
    emitter.onNext(2);
    emitter.onNext(3);
    emitter.onNext(4);
    emitter.onNext(5);
    emitter.onNext(null);
});

observable
    .filter(element -> element % 2 == 0)
    .map(element -> element * 10)
    // the second argument is for Consumer<? super Throwable> onError
    .subscribe(System.out::println, (error) -> {
        System.out.println("Error happened: " + error.getMessage());
    });
``` 
Output:

```
20
40
Error happened: onNext called with null. Null values are generally not allowed in 2.x operators and sources.

Process finished with exit code 0
``` 
Handling error at the operator level on Observable is shown in the code below.

```
Observable<Integer> observable = Observable.create(emitter -> {
    emitter.onNext(1);
    emitter.onNext(2);
    emitter.onNext(3);
    emitter.onNext(4);
    emitter.onNext(5);
    emitter.onNext(null);
});

observable
    .filter(element -> element % 2 == 0)
    .map(element -> element * 10)
    // if there is an error, then -1 will be returned
    .onErrorReturnItem(-1)
    .subscribe(System.out::println);
``` 
Output:

```
20
40
-1

Process finished with exit code 0
``` 
There are several other error related options available to us which can help to handle errors, like *onErrorReturn*, *onErrorResumeNext*, *doOnError* etc.

We can also use retry in the chain of operators. The intent of using retry is to retry the operation in case some error occurs. We can configure how the retry should be performed. Below is one simple example to make use of retry.

```
Observable<Integer> observable = Observable.create(emitter -> {
    emitter.onNext(1);
    emitter.onNext(2);
    emitter.onNext(3);
    emitter.onNext(4);
    emitter.onNext(5);
    emitter.onNext(null);
});

observable
    .filter(element -> element % 2 == 0)
    .map(element -> element * 10)
    // configured the retry here
    .retryWhen(element -> {
        return element.take(10).delay(2, TimeUnit.SECONDS);
    })
    .subscribe(System.out::println);
``` 
Here retry will be attempted for every 2 seconds, 10 times.

Now that we have got a fair idea on RxJava components, and have got our hand dirty with the basic offerings, I believe we should go ahead and learn something about types of Observable also in this article.

## 🎯 Types of Observable

- **Hot Observable**: These are the observable who do not wait for the observer to subscribe to them before they emit their data. Observers which subscribed late, they miss out the data previously emitted by this type of observable.

- **Cold Observable**: These are the observable which emit their data from start till end to all the observers who have subscribed to them. It is analogous to a Movie DVD, which an observer can play from start till end any time they want.

- **Connectable Observable**: These are the Cold Observable which are converted into Hot Observable. Suppose you have decided to host a movie party using a movie DVD to your friends at 9 PM. Those friends who join you late after 9 PM, they will miss out some part of the movie.

Observable that we created so far in examples above, they are all Cold Observable.

Let's see an example of how we can create a Hot Observable.

```
ConnectableObservable<Integer> observable = 
    Observable.range(1, 3).publish();

observable.subscribe(element -> {
    System.out.println("Observer 1 printing: " + element);
});
observable.subscribe(element -> {
    System.out.println("Observer 2 printing: " + element);
});
observable.connect();
``` 
Output:

```
Observer 1 printing: 1
Observer 2 printing: 1
Observer 1 printing: 2
Observer 2 printing: 2
Observer 1 printing: 3
Observer 2 printing: 3

Process finished with exit code 0
``` 
Both the observers are reading the emitted data, and in the same order.

Also notice that I have used *.publish()* on the observable. As soon as we do *.publish()* on the observable, it returns *ConnectableObservable*. 

*ConnectableObservable* is a hot observable.

I hope you can relate to the explanation that I had given above. This was a cold observable only, but we did *.publish()* on it and converted it to a hot observable.

The hot observable starts emitting data once we do *.connect()* on them.

Let's try changing the order of the observer listening to the observable, and see how it behaves.

```
ConnectableObservable<Integer> observable = 
    Observable.range(1, 3).publish();

observable.subscribe(element -> {
    System.out.println("Observer 1 printing: " + element);
});

observable.connect();

observable.subscribe(element -> {
    System.out.println("Observer 2 printing: " + element);
});
``` 
Output:

```
Observer 1 printing: 1
Observer 1 printing: 2
Observer 1 printing: 3

Process finished with exit code 0
``` 
The observer 2 is not able to get any data because before it could subscribe to the observable, all the data was emitted. Only observer 1 was subscribed beforehand, and that it is why it received all the emitted data.

## 🎯 Ending Notes

I hope this post gives you an overview to the basics of Reactive Programming and RxJava.

The source code for all the snippets used in this post are present at my [GitHub](https://github.com/anshulgammy/utopian-nerd/tree/main/usecases/rxjava-basics) repository. Please check this out. There are few more scenarios covered in RxJava in this repository that you can also play around with.