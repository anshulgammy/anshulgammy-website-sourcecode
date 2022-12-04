---
title: "Reactive Programming using CompletableFuture in Java"
date: 2022-04-07T19:49:38+05:30
draft: false
categories: [tech, blog]
tags: [reactive, programming, java, completablefuture]
description: "Practical use case that discusses nested operation on CompletableFuture in Java"
author: "Anshul Gautam"
---

Java 8 introduced **CompletableFuture** as part of it's concurrency API. This is very helpful when we want to nest operations around asynchronous computations, using the native Java API only.

Prior to introducing CompletableFuture, we had **Future** which provided asynchronous callbacks. But there were few problems associated with Future. CompletableFuture addresses them well.

Let's start with understanding what were the issues with Future, that paved the way for CompletableFuture.

## 🎯 Drawbacks with Future
- We cannot manually complete the Future object
- Lack of proper Exception Handling options available
- We cannot chain multiple Future objects together

In the below example, I am creating a simple Future, and trying to get the result from it.

```
public class FuturesDemo {

  public static void main(String[] args) {

    Random random = new Random();

    FutureTask[] futureTaskArray = new FutureTask[5];

    for (int i = 0; i < 5; i++) {
      futureTaskArray[i] = new FutureTask<Integer>(() -> {
        Thread.sleep(2000);
        return random.nextInt(1, 9);
      });
      Thread thread = new Thread(futureTaskArray[i]);
      thread.start();
    }

    for (int i = 0; i < 5; i++) {
      try {
        System.out.println(
            "Output obtained for FutureTask number " + i + " : " + futureTaskArray[i].get());
      } catch (InterruptedException e) {
        e.printStackTrace();
      } catch (ExecutionException e) {
        e.printStackTrace();
      }
    }
  }
}
``` 
There are five `FutureTask` objects being created here. Each `FutureTask` has the logic to produce a random number. Output from all these future objects are being obtained in the second for loop where we are doing `.get` on each of the FutureTask.

Now, what if we had to club the operation on `futureTaskArray[1]` and `futureTaskArray[2]` together? Get the output from one, and then supply that to another. This kind of clubbing together of multiple Future is not possible.

Also, see Future do not provide any manual way to end the future. There is no method provided for it. So if there is any `FutureTask` taking more time, the main thread will stay blocked.

## 🎯 CompletableFuture to the rescue
CompletableFuture helps us in solving the issues that we had with Future. It provides more robust way to do asynchronous computations. There are lots of helpful method operations provided to us out of the box.

Let's have a look on some key methods from CompletableFuture

- `runAsync()`: This is helpful when we want some instructions to run asynchronously and we do not expect any output.
This method takes Runnable, and there is one more overloaded method which takes Runnable and Executor.
If you are not providing Executor to runAsync(), then threads will be picked up from common fork join pool. If you have provided the Executor, then threads will be picked from the Executor's thread pool.

- `supplyAsync()`: This is helpful when we want to execute some instructions asychronously and also expect some output. This method takes Supplier type, and there is one more overloaded method which takes Supplier and Executor.
If you are not providing Executor to supplyAsync(), then threads will be picked up from common fork join pool. If you have provided the Executor, then threads will be picked from the Executor's thread pool.

## 🎯 Hands on example

Let's create a CompletableFuture object and play around it.
```
CompletableFuture<List<Employee>> getNewJoinerEmployees(List<Employee> employees,
      ExecutorService executorService)
      throws ExecutionException, InterruptedException {
    return CompletableFuture.supplyAsync(() -> {
      System.out.println("getNewJoinerEmployees on thread: " + Thread.currentThread().getName());
      List<Employee> employeesList = employees.stream().filter(
              employee -> ChronoUnit.MONTHS.between(employee.getDateOfJoining(), LocalDate.now()) <= 6)
          .collect(Collectors.toList());
      System.out.println(employeesList);
      return employeesList;
    }, executorService);
  }
``` 
In this example we are working on `List<Employee>` and filtering out those Employees who have not yet completed more than six months of tenure in their offices.
This `List<Employee>` is being returned as CompletableFuture.

Now let's see how can we apply nested operation on this returned CompletableFuture object, `thenApply()`, and `thenRun()`.

In this example, we want to get list of employees who have not completed the 6 months in their offices, and for those employees we want to send an email to complete mandatory trainings. After having sent the email, we want this application to shut down.
```
try {
      // Fetching all the employees present in the company's database.
      // (Tried to simulate such a situation, there is no DB though in this example)
      employeesService
          .getAllEmployeesCompletableFuture(executorService)
          .thenApply(
              // Fetching those employees who are new joiners,
              // whose System Current Date - DateOfJoining <= 6 months
              employees -> {
                try {
                  return employeesService.getNewJoinerEmployees(employees, executorService);
                } catch (ExecutionException ex) {
                  ex.printStackTrace();
                  return null;
                } catch (InterruptedException ex) {
                  ex.printStackTrace();
                  return null;
                }
              })
          .thenApply(
              // Fetching those employees who are new joiners,
              // and whose trainings are not yet completed.
              employees -> {
                try {
                  return employeesService.getTrainingsNotCompletedEmployees(employees.get(),
                      executorService);
                } catch (InterruptedException e) {
                  e.printStackTrace();
                  return null;
                } catch (ExecutionException e) {
                  e.printStackTrace();
                  return null;
                }
              })
          .thenAccept(
              // Collecting List<String> emailIds of those new joiner employees who have not
              // completed their trainings, and sending reminder email on those email ids.
              employees -> {
            try {
              employeesService.sendEmailsToEmployeesForTraining(employees.get(),
                  executorService);
            } catch (InterruptedException e) {
              e.printStackTrace();
            } catch (ExecutionException e) {
              e.printStackTrace();
            }
          })
          // After all the CompletableFuture are done, doing System.exit() to terminate
          // the application.
          .thenRun(() -> {
            System.exit(0);
          });
    } catch (InterruptedException e) {
      e.printStackTrace();
    } catch (ExecutionException e) {
      e.printStackTrace();
    }
``` 
`thenApply()` takes in Function type. It accepts a Function instance and uses it to process the result and then returns a Future object result.

`thenAccept()` takes in Consumer type.

`thenRun()` takes in the Runnable type.

Like a DSL, we have nested the operation on different CompletableFuture in the example above.

## 🎯 Github Repository
Code that I have used in this article is available at my [Github](https://github.com/anshulgammy/utopian-nerd/tree/main/usecases/java/completable-futures-demo) repository. Please check that out as well.

## 🎯 Conclusion
In this article we discussed with very basic examples that how can we implement CompletableFuture, and what were the drawbacks with Future.