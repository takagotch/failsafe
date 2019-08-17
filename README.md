### failsafe
---
https://github.com/jhalterman/failsafe

```java
RetryPolicy<Object> retryPolicy = new RetryPolicy<>()
  .handle(ConnectException.class)
  .withDelay(Duration.ofSeconds(1))
  .withMaxRetries(3);

Failsafe.with(retryPolicy).run(() -> connect());
Connection connection = Failsafe.with(retryPolicy).get(() -> connect());

CompletableFuture<Void> future = Failsafe.with(retryPolicy).runAsync(() -> connect());
CompletableFuture<Connection> future = Failsafe.with(retryPolicy).getAsync(() -> connect());

CircuitBreaker<Object> circuitBreaker = new CircuitBreaker<>();
Fallback<Object> fallback = Fallback.of(this::connectToBackup);

Failsafe.with(fallback, retryPolicy, circuitbreaker).get(this::connect);

FailsafeExecutor<Object> executor = Failsafe.with(fallback, retryPolicy, circuitBreaker);
executor.run(this::connect);


policy
  .handle(ConnectException.class, SocketException.class)
  .handleIf(failure -> failure instanceof ConnectException);
  
policy
  .handleResult(null)
  .handleResultIf(result -> result == null);


retryPolicy.withMaAttempts(3);
retryPolicy.withDelay(Duration.ofSeconds(1));
retryPolicy.withBackoff(1, 30, ChronoUnit.SECONDS);
retryPolicy.withDelay(1, 10, ChronoUnit.SECONDS);
retryPolicy.withJitter(.1);
retryPolicy.withMaxDuration(Duration.ofMinutes(5));
retryPolicy
  .abortWhen(true)
  .abortOn(NoRouteToHostException.class)
  .abortIf(result -> result == true)
  
Timeout<Object> timeout = Timeout.of(Duration.ofSeconds(10));
timeout.withCancel(shouldInterrupt);

Fallback<Object> fallback = Fallback.of(null);
Fallback<Object> fallback = Fallback.ofException(e -> new CustomException(e.getLastFailure()));

Execution execution = new Execution(retryPolicy);
while(execution.isComplete()) {
  try {
    doSomething();
    execution.complete();
  } catch (ConnectException e) {
    execution.recordFailure(e);
  }
}

Failsafe.with(retryPolicy)
  .getAsyncExecution(execution -> service.connect().whenComplete((result, failure) -> {
    if (execution.complete(result, failure))
      log.info("Connected");
    else if (!execution.retry())
      log.error("Connection attempts failed", failure);
}));

Failsafe.with(retryPolicy, circuitBreaker)
  .onComplete(e -> {
    if (e.getResult() != null)
      log.info("Connected to {}", e.getResult());
    else if (e.getFailure() != null)
      log.error("FAiled to create connection", e.getFailure());
  })
  .get(this::connect);
  
breaker.open();
breaker.halfOpen();
breaker.close();

if (breaker.allowsExecution()) {
  try {
    breaker.preExecutio();
    doSomething();
    breaker.recordSuccess();
  } catch (Exception e) {
    breaker.recordFailure(e);
  }
}

CircuitBreaker<Object> breaker = new CircuitBreaker<>()
  .handle(ConnectException.class)
  .withFailureThreshold(3, 10)
  .withSuccessThreshold(5)
  .withDelay(Duration.ofMinutes(1));
```

```
```

```
```
