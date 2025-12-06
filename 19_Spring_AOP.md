# Spring AOP:

## Before Advice, JoinPoint, After Advice:

```
@Component
@Aspect
public class LoggingAspect {
    private static final Logger log = LoggerFactory.getLogger(LoggingAspect.class);

    @Before("execution(* com.example.SpringREST.service.JobService.getJob(..)) || execution(* com.example.SpringREST.service.JobService.updateJob(..))")
    public void logMethodCall(JoinPoint jp) {
        log.info("logMethodCall " + jp.getSignature().getName());
    }

    @After("execution(* com.example.SpringREST.service.JobService.getJob(..)) || execution(* com.example.SpringREST.service.JobService.updateJob(..))")
    public void logMethodExecuted(JoinPoint jp) {
        log.info("logMethodExecuted " + jp.getSignature().getName());
    }

    @AfterThrowing("execution(* com.example.SpringREST.service.JobService.getJob(..)) || execution(* com.example.SpringREST.service.JobService.updateJob(..))")
    public void logMethodCrashed(JoinPoint jp) {
        log.info("logMethodCrashed " + jp.getSignature().getName());
    }

    @AfterReturning("execution(* com.example.SpringREST.service.JobService.getJob(..)) || execution(* com.example.SpringREST.service.JobService.updateJob(..))")
    public void logMethodExecutedSuccess(JoinPoint jp) {
        log.info("logMethodExecutedSuccess " + jp.getSignature().getName());
    }
}

```
## Performance Monitoring the input using Around Advice
```
@Component
@Aspect
public class PerformanceMonitorAspect {

    private static final Logger log = LoggerFactory.getLogger(PerformanceMonitorAspect.class);

    @Around("execution(* com.example.SpringREST.service.JobService.getJob(..))")
    public Object monitorTime(ProceedingJoinPoint pjp) throws Throwable {
        long start= System.currentTimeMillis();
        Object obj=pjp.proceed();
        long end= System.currentTimeMillis();
        long time=end-start;
        System.out.println("Time taken: "+time+ " ms");

        return obj;
    }
}
```

## Validating the input using Around Advice
```
@Component
@Aspect
public class validationAspect {
    private static final Logger log = LoggerFactory.getLogger(validationAspect.class);

    @Around("execution(* com.example.SpringREST.service.JobService.getJob(..)) && args(postId)")
    public Object validationAndUpdate(ProceedingJoinPoint pjp, int postId) throws Throwable {

        if(postId<0){
            postId = -postId;
        }

        Object obj=pjp.proceed(new Object[]{postId});

        return obj;
    }
}
```
---
---

Here is your **Spring AOP explanation rewritten properly and in SIMPLE language**, covering:

* What is AOP?
* Before Advice
* After Advice
* JoinPoint
* Around Advice
* Performance Monitoring
* Input Validation

All with **clear explanations of your code** 👇👇

---

# 🌿 **SPRING AOP — SIMPLE UNDERSTANDING**

---

# ## ⭐ What is AOP?

AOP = **Aspect Oriented Programming**

It allows you to add **extra behavior to your code** without editing your actual classes.

Examples of behavior you can add:

✔ Logging
✔ Security
✔ Validation
✔ Transaction management
✔ Performance measurement
✔ Exception handling

👉 **You write code separately (aspects)** and Spring automatically applies it to your application.

---

# ## ⭐ What is an Aspect?

`@Aspect` = special class where you write extra behavior.

```java
@Component
@Aspect
public class LoggingAspect {
}
```

---

# ## ⭐ What is a JoinPoint?

JoinPoint = **point where the aspect will be applied inside a method call**.

Example:

* Before method runs
* After method runs
* After exception
* After returning successfully
* Around the method

👉 JoinPoint gives method information like:

* method name
* arguments
* target class

---

# ## ⭐ BEFORE ADVICE

**Runs before a method gets executed**

```java
@Before("execution(* JobService.getJob(..)) || execution(* JobService.updateJob(..))")
public void logMethodCall(JoinPoint jp) {
    log.info("logMethodCall " + jp.getSignature().getName());
}
```

### Meaning:

* Before calling `getJob()` or `updateJob()`
* Print log with method name

### Use Case:

✔ Logging
✔ Security check
✔ Input validation

---

# ## ⭐ AFTER ADVICE

**Runs after the method completes**

```java
@After("execution(* JobService.getJob(..)) || execution(* JobService.updateJob(..))")
public void logMethodExecuted(JoinPoint jp) {
    log.info("logMethodExecuted " + jp.getSignature().getName());
}
```

### Meaning:

* Runs after method returns or throws exception
* Good for cleanup or logging

---

# ## ⭐ AFTER THROWING ADVICE

**Runs only when method CRASHES (throws exception)**

```java
@AfterThrowing("execution(* JobService.getJob(..)) || execution(* JobService.updateJob(..))")
public void logMethodCrashed(JoinPoint jp) {
    log.info("logMethodCrashed " + jp.getSignature().getName());
}
```

### Good for:

✔ Error logging
✔ Alerting
✔ Retry logic

---

# ## ⭐ AFTER RETURNING ADVICE

**Runs only when method returns successfully**

```java
@AfterReturning("execution(* JobService.getJob(..)) || execution(* JobService.updateJob(..))")
public void logMethodExecutedSuccess(JoinPoint jp) {
    log.info("logMethodExecutedSuccess " + jp.getSignature().getName());
}
```

### Good for:

✔ Performance tracking
✔ Business logging
✔ Post-processing

---

# ## ⭐ AROUND ADVICE (Most Powerful)

Around Advice wraps the whole method call:

```java
@Around("execution(* JobService.getJob(..))")
public Object monitorTime(ProceedingJoinPoint pjp) throws Throwable {
    long start= System.currentTimeMillis();
    Object obj=pjp.proceed();
    long end= System.currentTimeMillis();
    System.out.println("Time taken: "+(end-start)+" ms");
    return obj;
}
```

### What it does?

* Runs **before** method
* Calls method itself (`pjp.proceed()`)
* Runs **after** method
* Measures total execution time

### Use Case:

✔ Performance measurement
✔ Debugging slow methods
✔ Monitoring

---

# ## ⭐ INPUT VALIDATION USING AROUND ADVICE

```java
@Around("execution(* JobService.getJob(..)) && args(postId)")
public Object validationAndUpdate(ProceedingJoinPoint pjp, int postId) throws Throwable {

    if(postId < 0){
        postId = -postId;
    }

    Object obj = pjp.proceed(new Object[]{postId});
    return obj;
}
```

### Meaning:

* Before calling `getJob(postId)`
* If `postId` is negative, convert to positive
* Then call original method with updated input

### Why it’s powerful?

✔ Modify arguments
✔ Stop execution
✔ Check security
✔ Log input/output
✔ Clean input

---

# 🌟 **DIFFERENCE BETWEEN ALL ADVICES**

| Advice Type         | When It Runs                                    |
| ------------------- | ----------------------------------------------- |
| **@Before**         | Before method execution                         |
| **@After**          | After method execution (success or failure)     |
| **@AfterReturning** | Only after success                              |
| **@AfterThrowing**  | Only if exception occurred                      |
| **@Around**         | Before + After + ability to control method call |

👉 **Around = most powerful (you can measure time, validate input, block call, etc.)**

---

# 🌟 **WHY USE AOP?**

Instead of writing logging/validation logic inside every service method…

You write it once in AOP.

Spring applies it automatically.

---

# ⭐ FINAL SUMMARY (1 line each)

* **Aspect = extra logic separate from business code**
* **JoinPoint = method where AOP applies**
* **Before = runs before**
* **After = runs after**
* **AfterReturning = success**
* **AfterThrowing = failure**
* **Around = full control (before + after + modify input/output)**

---


