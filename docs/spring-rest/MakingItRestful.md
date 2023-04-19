# Making our application RESTful

Here is a quote from Roy Fielding, the author of REST:

> I am getting frustrated by the number of people calling any HTTP-based interface a REST API. Today’s example is the SocialSite REST API. That is RPC. It screams RPC. There is so much coupling on display that it should be given an X rating.

>What needs to be done to make the REST architectural style clear on the notion that hypertext is a constraint? In other words, if the engine of application state (and hence the API) is not being driven by hypertext, then it cannot be RESTful and cannot be a REST API. Period. Is there some broken manual somewhere that needs to be fixed?

— Roy Fielding
https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven

## So what does that mean?

Essentially, the responses we return from our services need to contain information about what the client can do next. For example, if the first call to our service is to get all Books, then we need to provide information about what can be done next with those books. Maybe one can be deleted but another can't, or one can be updated but another is read only. This information will be sent in the form of hypertext in the json response(s).

This principle is called HATEOAS (Hypermedia as the Engine of Application State) and is what we will be implementing in the following sections.

The example given on the [HATEOAS Wikipedia page](https://en.wikipedia.org/wiki/HATEOAS) gives a nice example of this principle in action.

## Adding in HATEOAS to our application

Here is our modified code with HATEOAS added in:

The Controller:

```java

// imports omitted for brevity

@RestController
@Slf4j
@RequiredArgsConstructor
public class BookController {

  private final BookService bookService;

  private final BookModelAssembler bookModelAssembler;

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
  public CollectionModel<EntityModel<BookDTO>> getBooks(
      @RequestParam(required = false) String title) {
    log.debug("Getting all books with a title that contains: {} ", title);

    List<BookDTO> books = bookService.findBooks(title);
    if (CollectionUtils.isEmpty(books)) {
      throw new BookNotFoundException("No books found for title " + title);
    }

    List<EntityModel<BookDTO>> booksModels = books.stream()
        .map(bookModelAssembler::toModel)
        .toList();

    return CollectionModel.of(booksModels,
        linkTo(methodOn(BookController.class).getBooks("")).withSelfRel());
  }

  /**
   * Gets a single book. Note that as we are getting a resource we use {@link PathVariable}, not
   * {@link RequestParam}, which should generally be used for sorting or filtering the results.
   *
   * @param isbn - the ISBN of the Book.
   * @return - the Book with the given ISBN.
   */
  @GetMapping("/books/{isbn}")
  public EntityModel<BookDTO> getOneBook(@PathVariable(value = "isbn") String isbn) {
    // Note that we do not need to check that the isbn is not empty here as it is
    // a path variable. If it were empty then the request would get all books.
    return bookModelAssembler.toModel(bookService.getBook(isbn));
  }

  @PostMapping("/books")
  public EntityModel<BookDTO> addBook(@Valid @RequestBody BookDTO bookDTO) {
    return bookModelAssembler.toModel(bookService.addBook(bookDTO));
  }

  @DeleteMapping("books/{isbn}")
  public void deleteBook(@PathVariable String isbn) {
    // Note that we do not need to check that the isbn is not empty here as it is
    // a path variable. If it were empty then the request would be rejected.
    bookService.removeBook(isbn);
  }

}

```

And the BookModelAssembler it uses:

```java
package uk.co.cadogsoftware.api.assemblers;

import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.linkTo;
import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.methodOn;

import org.springframework.hateoas.EntityModel;
import org.springframework.hateoas.server.RepresentationModelAssembler;
import org.springframework.stereotype.Component;
import uk.co.cadogsoftware.api.controllers.BookController;
import uk.co.cadogsoftware.api.dtos.BookDTO;

@Component
public class BookModelAssembler implements RepresentationModelAssembler<BookDTO, EntityModel<BookDTO>> {

  @Override
  public EntityModel<BookDTO> toModel(BookDTO bookDTO) {

    return EntityModel.of(bookDTO,
        linkTo(methodOn(BookController.class).getOneBook(bookDTO.isbn())).withSelfRel(),
        linkTo(methodOn(BookController.class).getBooks("")).withRel("books"));
  }
}
```

Here are some sample responses from calling the controller methods:

Get a single book (GET http://localhost:8080/books/4-5-6):

Response:

```
{
    "author": "J. R. R. Tolkien",
    "isbn": "4-5-6",
    "title": "The Lord of the Rings",
    "_links": {
        "self": {
            "href": "http://localhost:8080/books/4-5-6"
        },
        "books": {
            "href": "http://localhost:8080/books?title="
        }
    }
}
```

Get all books (GET http://localhost:8080/books):

Response:

```
{
    "_embedded": {
        "bookDTOList": [
            {
                "author": "George Orwell",
                "isbn": "1-2-3",
                "title": "Animal Farm",
                "_links": {
                    "self": {
                        "href": "http://localhost:8080/books/1-2-3"
                    },
                    "books": {
                        "href": "http://localhost:8080/books?title="
                    }
                }
            },
            {
                "author": "J. R. R. Tolkien",
                "isbn": "4-5-6",
                "title": "The Lord of the Rings",
                "_links": {
                    "self": {
                        "href": "http://localhost:8080/books/4-5-6"
                    },
                    "books": {
                        "href": "http://localhost:8080/books?title="
                    }
                }
            },
            {
                "author": "Harper Lee",
                "isbn": "7-8-9",
                "title": "To Kill a Mockingbird",
                "_links": {
                    "self": {
                        "href": "http://localhost:8080/books/7-8-9"
                    },
                    "books": {
                        "href": "http://localhost:8080/books?title="
                    }
                }
            }
        ]
    },
    "_links": {
        "self": {
            "href": "http://localhost:8080/books?title="
        }
    }
}
```

Notice how the responses now contain a "_links" section containing the HATEOAS links. Using these, any client can determine what to do next.
