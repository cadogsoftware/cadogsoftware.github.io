# Making Non Breaking Changes to our API

When we make changes to our API we need to make sure, where possible that these do not break existing clients. This section describes a way of doing this.

## Change the model

When you need to add a new field to a model you should still support the old model.
For example, if you have a Book entity that has a 'name', and then you have a requirement to support 'firstName' and 'lastName' instead, then you should support BOTH ways of creating a book. In this way you do not break the old API and you still support the new API.

*** BEST PRACTICE : Don't remove old fields in your model, support them. ***

As well as accepting both ways of creating a Book, you need to make sure that both ways allow you to perform CRUD operations, i.e. you can Create, Read, Update and Delete Books in either format.

As for all of this project, the full source code is available here: [API Best Practices](https://github.com/cadogsoftware/APIBestPractices) but here are some samples showing how a change to the Book is implemented and done in a non-breaking way:

### The BookDTO

```java

package uk.co.cadogsoftware.api.dtos;

import jakarta.validation.constraints.NotEmpty;
import lombok.EqualsAndHashCode;
import lombok.Getter;
import lombok.RequiredArgsConstructor;
import org.springframework.util.StringUtils;
import uk.co.cadogsoftware.api.validators.BookValidation;

/**
 * Details of the book(s) exposed to the end clients.
 */
@RequiredArgsConstructor
@BookValidation
@EqualsAndHashCode
public class BookDTO {

    private final String author; // Left here to support backwards compatibility.

    private final String authorFirstName; // Introduced in version 2 of the API.

    private final String authorLastName; // Introduced in version 2 of the API.

    @Getter
    @NotEmpty(message = "isbn must be provided")
    private final String isbn;

    @Getter
    @NotEmpty(message = "title must be provided")
    private final String title;

    /**
     * If we have an author return it, otherwise get if from first and last names.
     * Added for backwards compatibility.
     * @return the author.
     */
    public String getAuthor() {
        String authorToReturn = "";
        if (StringUtils.hasText(author)) {
            authorToReturn = author;
        } else if (StringUtils.hasText(authorFirstName) && StringUtils.hasText(authorLastName)) {
            authorToReturn = authorFirstName + " " + authorLastName;
        }
        return authorToReturn;
    }

    /**
     * If we have an author first name return it, otherwise get if from author.
     * Added for backwards compatibility.
     * @return the author first name.
     */
    public String getAuthorFirstName() {
        String authorFirstNameToReturn = "";
        if (StringUtils.hasText(authorFirstName)) {
            authorFirstNameToReturn = authorFirstName;
        } else if (StringUtils.hasText(author)) {
            String[] parts = author.split(" ");
            if (parts.length > 0) {
                authorFirstNameToReturn = parts[0];
            }
        }
        return authorFirstNameToReturn;
    }

    /**
     * If we have an author last name return it, otherwise get if from author.
     * Added for backwards compatibility.
     * @return the author last name.
     */
    public String getAuthorLastName() {
        String authorLastNameToReturn = "";
        if (StringUtils.hasText(authorLastName)) {
            authorLastNameToReturn = authorLastName;
        } else if (StringUtils.hasText(author)) {
            String[] parts = author.split(" ");
            if (parts.length > 1) {
                authorLastNameToReturn = parts[1];
            }
        }
        return authorLastNameToReturn;
    }

}

```

Then whenever a BookDTO is used we convert it to the new format using the BookConverter:

```java
package uk.co.cadogsoftware.api.converters;

import java.util.List;
import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;
import uk.co.cadogsoftware.api.database.entities.Book;
import uk.co.cadogsoftware.api.dtos.BookDTO;

/**
 * Converts {@link BookDTO}s to {@link Book}s and vice versa.
 *
 */
@Service
public class BookConverter {

  public Book convertToBook(BookDTO bookDTO) {
    // Create the book from the new structure.
    return new Book(bookDTO.getAuthorFirstName(), bookDTO.getAuthorLastName(),
        bookDTO.getIsbn(), bookDTO.getTitle());
  }

  public BookDTO convertToBookDTO(Book book) {
    return new BookDTO(book.getAuthor(), book.getAuthorFirstName(), book.getAuthorLastName(),
        book.getIsbn(), book.getTitle());
  }

  public List<BookDTO> convertToBookDTOList(List<Book> books) {
    return books.stream().map(this::convertToBookDTO).toList();
  }

}

```

Using this method a change has been made to the Book, but clients using the old structure still work - i.e. we have introduced a non-breaking change.
Of course, not every change is as simple as splitting the name into first and last names but it should be possible to cover many scenarios by transforming the data as done above.

## Summary

This project has shown how to develop REST based services with Java and Spring.
We used the HATOAS principles to return links in our REST responses, to ensure that we weren't just making RPC style services.
We also showed how to support making non-breaking changes to our APIs.

As mentioned at the start of this article, the basis for what has been written here was taken from the excellent Spring guide: [Building REST services with Spring](https://spring.io/guides/tutorials/rest/)

Full source code of this service can be found here: [API Best Practices](https://github.com/cadogsoftware/APIBestPractices)

We hoped you enjoyed reading this and found it beneficial!
