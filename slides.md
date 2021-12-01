# Clean Architecture

Andrew Cline

---

## Goals

As a listener:

- I am able to describe the Clean Architecture Model.
- I am able to define "Domain" in context of building software.
- I have language to discuss different parts of software achtecture.

---

## What is Software Architecture?

It's a plan for building code.

---

## What is Clean Architecture?

Clean architecture is simply one generic blueprint that we can use to apply to our applications.

It is meant to be a guide in making correct, testable programs and applications that are resilient to change

---

![Diagram of clean architecture](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

---

## Entities

These are the core of the application. They define the players in the application.
For example, a Library Catalog application might have entities like Librarys, Books, and Users.

A Book might have certain properties, like title, author, ISBN.

---

![Diagram of clean architecture](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

---

## Use Cases

Use cases are a set of rules on how entities might interact, between themselves and the outside world.

for example, a user might want to checkout a book.

---

## Domain

Entities and Use Cases make up the core "business logic" of an application.
This could also be described as the domain of a software project.

It is not the software developers job to define this domain.
As a team we should work with the subject matter experts to define the domain of the application we are building.

It is the software developer's job to describe the domain in code.

---

## Describing Domain\*

Developers have a vast array of tools for describing domain specific propblems.
Depending on your favorite progaming language or paradigm you will probably have a different answer for this.

----

My personal preferance is to build entities as types.

```typescript
type Book = {
  title: string;
  author_first_name: string;
  author_last_name: string;
  isbn: string;
};
```
----

and use cases as functions

```
const checkoutBook = (user: User, book: Book): User => {
  // ...
  // this can be whatever logic we choose.
  // I would write this as a User can have many checked out books
}
```

---

![Diagram of clean architecture](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

---

## Adapters, Gateways, and Presentors

These are abstractions of actions that we will need to implement in our applicaitons.

For example, we need to persit the changes we make to our User entity when they checkout a book.

----

So we might create a function called `saveUser()`, and we could use it like this.

```typescript
const user = getUser(id);

const updated_user = checkoutBook(user, book);

saveUser(updated_user);
```

---

![Diagram of clean architecture](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

---

## Everything else

Devices, Servers, APIs, Databases, User Interfaces, Virtual Machines, Hardware.

Vue or React code, MySQL, MongoDB, AWS. all the tools we use to get our applications in the hands of human beings and allow them to interact.

---

![Diagram of clean architecture](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

---

## Flow of dependancy

Everything goes inside out. nothing(\*) comes from the outside in.

---

![Diagram of clean architecture](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

---

## How??...

Dependancy inversion

```typescript
const makeBooksAdapter = (db) => {
  return {
    listBooks: () => db.query(`SELECT * from books WHERE status > 0`),
    getBook: (id) =>
      db.query(`SELECT * from books WHERE status > 0 AND id = ${id}`),
    saveBook: () => db.query(`...`),
    deleteBook: () => db.query(`...`),
  };
};
```

---

## But why???

- Testablity

  - This allows us to "stub" anything into our tests, like an in memory version of sqlite, or a book adapter that is just
  - It makes unit testing so easy. no weird mocks, no complex test setup. Just stub and go.
  - see me for an example.

----

- Resiliance
  - Dependancy inversion also us to adapt to the ever changing climate of Software Development

---

![Diagram of clean architecture](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)
