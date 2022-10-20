# Callable and Future

## Learning Goals

- Explain a Callable.
- Explain a Future.
- Use Callable and Future to write and run asynchronous code.

## Introduction

We have been using tasks which implement the `Runnable` interface. But sometimes
we want the tasks to return a result. The `Callable` interface along with the
`Future` interface provides a way for getting results from tasks.

## Callable

`Callable` is a functional interface with a `call` method that returns a value.

```java
public interface Callable<V> {
		V call() throws Exception;
}
```

## Future

We can submit tasks which implement the `Callable` interface to an executor but
the `submit` method doesn’t return the result of the task since it is executed
asynchronously. The `submit` method instead returns a `Future` object.

The `Future` interface has the following methods:

- `get`: A blocking call that waits for a task to complete and gets the result.
  It also accepts a timeout which defines how long it will wait for task
  completion.
- `cancel`: Cancels the task if possible.
- `isDone`: It returns `true` if the task ran successfully.
- `isCancelled`: It returns `true` if the `cancel` method was called on the task
  before it finished running.

## Example  

```java
import java.util.concurrent.*;

class Example {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newFixedThreadPool(5);

        Future<Integer> num1 = executor.submit(()-> {
            try {
                Thread.sleep(3000);
                return 1000;
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        });

        Future<Integer> num2 = executor.submit(()-> {
            try {
                Thread.sleep(500);
                return 2000;
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        });

        int result = num1.get() * num2.get();
        System.out.println(result); // 2000000

				executor.shutdown();
    }
}
```

We have created two `Callable` objects using lambda expressions and passed them
to the `submit` method. When we call the `get` method on `num1` and `num2` the
program waits for the tasks to complete and return a result before continuing.

## Conclusion

We learned how to use `Callable` and `Future` the interfaces in this lesson.
They make it easier to handle results from asynchronous computation by providing
simple methods for checking task status and accessing their return values.


## Resources

[Java 11 Callable](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Callable.html)    
[Java 11 Future](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Future.html)  
