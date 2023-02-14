# REST API Versioning

## Setup
I set up a service that exposes two GET and two POST REST endpoints. This was written in Java using Spring Boot, backed by a PostgreSQL database.

If you need to do this yourself I'd recommend this site as a good tutorial:
```bash
https://www.callicoder.com/spring-boot-jpa-hibernate-postgresql-restful-crud-api-example/
```
which in turn links to the source code here
```bash
https://github.com/callicoder/spring-boot-postgresql-jpa-hibernate-rest-api-demo
```

At this point I could hit my 2 POST endpoints:

POST, http://localhost:8080/questions with body:
```bash
{
	"title": "rich test 2",
	"description": "some test desc 2"
}
```

POST, http://localhost:8080/questions/1000/answers with body:
```bash
{
	"text": "answer one to question 1000"
}
```

And my two GET endpoints:

GET http://localhost:8080/questions?page=0&size=2&sort=createdAt,desc

GET http://localhost:8080/questions/1000/answers

So now I'm ready to get going on trying to introduce a breaking change for which I'll need API versioning.....

