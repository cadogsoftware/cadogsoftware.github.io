# Building a Good REST Service with Spring

## Introduction
We have developed many so called 'REST' services with Spring, but what makes a good REST service as opposed to just an RPC style service. Read on for more.

This guide is rightly based on Spring's recommendations, with our own comments added in. The Spring resources that were used in putting this guide together are here:


* [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
* [Building REST services with Spring](https://spring.io/guides/tutorials/rest/)

## Sample code

This is in progress! More to come soon!

### A simple controller and DTO to get us going

```java
package uk.co.cadogsoftware.api.controllers;

import java.util.List;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import uk.co.cadogsoftware.api.dtos.BookDTO;

@RestController
public class BookController {

  @GetMapping("/books")
  public List<BookDTO> getBooks() {
    // TODO: look up the books here!
    return List.of(new BookDTO(1, "Animal Farm", "George Orwell"),
        new BookDTO(2, "1984", "George Orwell"));
  }

  @GetMapping("/books/{id}")
  public BookDTO getBookById(@RequestParam(value = "id", defaultValue = "1") int id) {
    // TODO: look up the book here!
    return new BookDTO(1, "Animal Farm", "George Orwell");
  }

}

```

```java
package uk.co.cadogsoftware.api.dtos;

public record BookDTO(int id, String title, String author) {

}
```
