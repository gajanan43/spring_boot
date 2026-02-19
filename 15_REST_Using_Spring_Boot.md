# REST Using Spring Boot:

1) What is REST
2) HTTP Methods
3) Creating a REST Controller
4) Path Variable
5) Sending Data & RequestBody 
6) PUT & DELETE Mapping
7) Content Negotation(In background jackson library convert JAVA Objects into JSON)


```

@RestController
public class JobRestController {

    @Autowired
    private JobSerivce service;

    @GetMapping("/jobPosts")
    public List<JobPost> getJobs() {
        return service.getAllJobs();
    }

    @GetMapping("jobPosts/{postId}")
    public JobPost getJob(@PathVariable int postId) {
        return service.getJob(postId);
    }

    @PostMapping("jobPosts")
    public void addJob(@RequestBody JobPost jobPost) {
        service.addJob(jobPost);
    }

    @PutMapping("jobPosts")
    public JobPost updateJob(@RequestBody JobPost jobPost) {
        service.updateJob(jobPost);
        return jobPost;
    }

    @DeleteMapping("jobPosts/{postId}")
    public void deleteJob(@PathVariable int postId){
        service.deleteJob(postId);
    }
}
```

# application,properties

```
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=root
spring.datasource.password=mysql
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql= true
```
