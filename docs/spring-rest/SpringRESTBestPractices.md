# Building a Good REST Service with Spring

## Introduction
We have developed many so called 'REST' services with Spring, but what makes a good REST service as opposed to just an RPC style service. Read on for more.

This guide is rightly based on Spring's recommendations, with our own comments added in. The Spring resources that were used in putting this guide together are here:

* [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
* [Building REST services with Spring](https://spring.io/guides/tutorials/rest/)

We have built a project containing services to perform CRUD operations on our domain object, in this case Books, but it
could be anything.

The full source code can be found here: [API Best Practices](https://github.com/cadogsoftware/APIBestPractices)

### Summary of Best Practices used here

This section has been provided for the reader who just wants a quick summary of the best practices we have shown in what follows

TODO: add to this as we go along

- In our controller(s) use RequestParam for filtering/sorting/searching.
- Perform minimal processing in Controllers. Use a service layer to help separate logic.
- Separate the client's view of the data from that stored in the database.
-



## Sample code

This is in progress! There are some 'TODOs' in the code samples at the moment. Hopefully these are self-explanatory. They will be resolved soon.

Some best practices that are worth highlighting are shown in bold and start with *** BEST PRACTICE ***.

### The Controller layer

```java
package uk.co.cadogsoftware.api.controllers;

import java.util.List;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import uk.co.cadogsoftware.api.dtos.BookDTO;
import uk.co.cadogsoftware.api.services.BookService;

/**
 * Handles REST requests for {@link BookDTO}s.
 *
 * @author Richard Morgan
 */
@RestController
@Slf4j
@RequiredArgsConstructor
public class BookController {

  private final BookService bookService;

  /**
   * Get all of the {@link BookDTO}s.
   * <p>
   * Note use of the {@link RequestParam} annotation here. This is set to required = false so that
   * we can return all books or books with the given title.
   * </p>
   * <p>
   * The link {@link RequestParam} annotation is not used to identify the resource (a book), but it
   * is used for filtering of the results. If I wanted to identify the resource I would use a
   * {@link PathVariable} instead, as in the other method in this class that gets a single
   * {@link BookDTO}. Another use of {@link RequestParam} would be for sorting of the results.
   * </p>
   *
   * @param title - an optional title to filter the results by.
   * @return - all books or books containing the given title.
   */
  @GetMapping("/books")
  public List<BookDTO> findBooks(@RequestParam(required = false) String title) {
    log.debug("Getting all books with a title that contains: {} ", title);
    return bookService.findBooks(title);
  }

  /**
   * Gets a single book. Note that as we are getting a resource we use {@link PathVariable}, not
   * {@link RequestParam}, which should generally be used for sorting or filtering the results.
   *
   * @param isbn - the ISBN of the Book.
   * @return - the Book with the given ISBN.
   */
  @GetMapping("/books/{isbn}")
  public BookDTO getOneBook(@PathVariable(value = "isbn") String isbn) {
    // TODO: validate input
    return bookService.getBook(isbn);
  }

  @PostMapping("/books")
  public BookDTO addBook(@RequestBody BookDTO bookDTO) {
    // TODO: validate input
    return bookService.addBook(bookDTO);
  }

  @DeleteMapping("books/{isbn}")
  public void deleteBook(@PathVariable String isbn) {
    // TODO: validate isbn here.
    bookService.removeBook(isbn);
  }

}


```
Here is the simple DTO used:

```java
package uk.co.cadogsoftware.api.dtos;

/**
 * Details of the book(s) exposed to the end clients.
 */
public record BookDTO(String author, String isbn, String title) {

}
```

### When should I use @PathVariable and @RequestParam

Use @PathVariable to identify a resource e.g.

```java
@GetMapping("/books/{isbn}")
public BookDTO getOneBook(@PathVariable(value = "isbn") int isbn) { ... }
```

*** BEST PRACTICE : Use RequestParam for filtering/sorting/searching the resources we want e.g.***

```java
@GetMapping("/books")
public List<BookDTO> getBooks(@RequestParam(required = false) String title) {
  // Use the title to filter the results here if it is provided.
  ... }
```

Note that here the title is not the resource we are returning (i.e. a BookDTO), but it is used to filter the list of resources we are returning.

Both @PathVariable and @RequestParam are required by default but can be made optional as in the case of the @RequestParam example above, but be careful when making @PathVariable optional, to avoid conflicts in paths.

### Data objects used

The clinet view of the Book is the BookDTO, but the database stores a slightly differnt object, in our case a Book. We have decoupled the client's view of the Book from the Book stored in the database so that we can expose only the fileds we want to to the clinet. In this case our Book contains a generated id that the client does not need to know about.

*** BEST PRACTICE : Separate the client's view of the data from that stored in the database ***

The BookDTO is shown above. Here is the Book Entity that we will store in the database:

```java
package uk.co.cadogsoftware.api.database.entities;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.Id;
import lombok.EqualsAndHashCode;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.ToString;

/**
 * Represents the Book in the database.
 */
@Entity
@EqualsAndHashCode
@ToString
@NoArgsConstructor
public class Book {

  @Getter
  private @Id @GeneratedValue Long id;
  @Getter
  private String author;
  @Getter
  private String isbn;
  @Getter
  private String title;

  public Book(String author, String isbn, String title) {
    this.author = author;
    this.isbn = isbn;
    this.title = title;
  }

}

```

### The Serivce Layer

The service layer does two things. Firstly it transforms BookDTOs to Books and vice versa via the use of a dedicated class (the BookConverter) and controls interaction with the database, via the repository class.

Here is the BookService:

```java
package uk.co.cadogsoftware.api.services;

import java.util.List;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;
import uk.co.cadogsoftware.api.converters.BookConverter;
import uk.co.cadogsoftware.api.database.entities.Book;
import uk.co.cadogsoftware.api.database.repositories.BookRepository;
import uk.co.cadogsoftware.api.dtos.BookDTO;
import uk.co.cadogsoftware.api.exceptions.BookAlreadyExistsException;
import uk.co.cadogsoftware.api.exceptions.BookNotFoundException;

/**
 * A class that controls interactions with books.
 * <p>
 * Handles the conversion of {@link BookDTO}s to {@link Book}s and vice versa with use of the
 * {@link BookConverter}.
 * </p>
 */
@Service
@RequiredArgsConstructor
@Slf4j
public class BookService {

  private final BookRepository bookRepository;

  private final BookConverter bookConverter;

  public BookDTO getBook(String isbn) {
    Book book = bookRepository.findByIsbn(isbn);
    if (book == null) {
      throw new BookNotFoundException(isbn);
    }
    return bookConverter.convertToBookDTO(book);
  }

  public List<BookDTO> findBooks(String titleFilter) {

    if (StringUtils.hasText(titleFilter)) {
      return findBooksMatchingTitle(titleFilter);
    } else {
      return getAllBooks();
    }

  }

  public void removeBook(String isbn) {
    Book book = bookRepository.findByIsbn(isbn);
    if (book != null) {
      bookRepository.deleteById(book.getId());
    } else {
      log.warn("Book requested for deletion but was not found for ISBN: {}", isbn);
    }

  }

  public BookDTO addBook(BookDTO bookDto) {

    if (doesBookExistByIsbn(bookDto)) {
      throw new BookAlreadyExistsException("Book already exists for ISBN: " + bookDto.isbn());
    }

    if (doesBookExistByTitleAndAuthor(bookDto)) {
      throw new BookAlreadyExistsException(
          "Book already exists for title: " + bookDto.title() + " and author: " + bookDto.author());
    }

    Book book = bookConverter.convertToBook(bookDto);

    bookRepository.save(book);
    return bookDto;
  }

  private boolean doesBookExistByTitleAndAuthor(BookDTO bookDtoToLookFor) {
    List<Book> allMatchingBooksByTitleAndAuthor = bookRepository.findByTitleAndAuthor(
        bookDtoToLookFor.title().trim(), bookDtoToLookFor.author().trim());
    return !allMatchingBooksByTitleAndAuthor.isEmpty();
  }

  private boolean doesBookExistByIsbn(BookDTO bookDTO) {
    Book allMatchingBooksByIsbn = bookRepository.findByIsbn(bookDTO.isbn());
    return allMatchingBooksByIsbn != null;
  }

  private List<BookDTO> getAllBooks() {
    List<Book> books = bookRepository.findAll();
    return bookConverter.convertToBookDTOList(books);
  }

  private List<BookDTO> findBooksMatchingTitle(String titleFilter) {
    List<Book> books = bookRepository.findByTitleContaining(titleFilter);
    return bookConverter.convertToBookDTOList(books);
  }
}

```

### The Data Access layer

For simplicity we are using an embedded H2 database, and populate that on start-up of the application.
This would not be used in a production environment as the data is not persisted when the application is stopped, but is useful for development.

Spring JPA is used to access the database. Here is our repository class:

```java
package uk.co.cadogsoftware.api.database.repositories;

import java.util.List;
import org.springframework.data.jpa.repository.JpaRepository;
import uk.co.cadogsoftware.api.database.entities.Book;

/**
 * Used to interact with Book entries in the database.
 */
public interface BookRepository extends JpaRepository<Book, Long> {

  Book findByIsbn(String isbn);
  List<Book> findByTitleContaining(String titleToMatch);

  void deleteByIsbn(String isbn);

  List<Book> findByTitleAndAuthor(String title, String author);

}
```

As you can see we have added a few methods that we need. Spring JPA makes it particularly easy to add your own custom queries such as 'findByTitleAndAuthor'. Hopefully it is obvious what this does!

### Start up the application

Once you have checked out the code from here [API Best Practices](https://github.com/cadogsoftware/APIBestPractices), you can build the code:

```
mvn clean install

```
And then start the application:

```
./mvnw clean spring-boot:run
```

### Testing

There are unit tests within the code base, but to perform intgration tests (i.e. end to end tests) you can import the Postman file from the project into your Postman app and run the requests in that. The Postman file is here https://github.com/cadogsoftware/APIBestPractices/blob/main/integration-tests/API_Best_Practices.postman_collection.json

Alternatively you can run some curl commands like this:

```
curl -v -X GET localhost:8080/books

curl -v -X GET localhost:8080/books/1-2-3

curl -X DELETE localhost:8080/books/1-2-3

curl -X POST localhost:8080/books -H 'Content-type:application/json' -d '{"isbn" : 9-9-9, "title": "Test Book", "author": "Sandy Orchid Else"}'

```

### Where are we so far?

So far, we have created a CRUD application. Whilst this is all very good and follows a lot of best practices, it is not really RESTful. This is because the end user of our service(s) does not know how to interact with it. We would have to provide documentation that describes how the services should be used.

So, as in the Spring guide https://spring.io/guides/tutorials/rest/, the next step is to make this application RESTful.

Roy Fielding describes what makes something RESTful far better than we can, in his guide from 2008 https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven

Follow the link here to see these next steps: [Making it RESTful](./MakingItRestful.md)
