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

