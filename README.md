Spring Security Resource Server
===============================
This project is a simple Spring WebFlux API that uses Spring Security's Resource
Server to enable authorization via an OAuth2 access token.

The API exposes three endpoints:

 - GET /books - Lists all books in the database. Does not require authorization.
 - GET /books/{id} - Lists a specific book by its ID. Does not require authorization.
 - POST /books - Adds a book. Requires a JWT token with "data.write" scope.

The token can be obtained from the 
[essential authorization server](https://github.com/habuma/spring-auth-server).
The authorization server must also be running when using this API so that tokens
presented to the API can be verified.

How to use
----------

 - Start the application:
 
~~~
% ./mvnw spring-boot:run
~~~

 - Make requests to the GET endpoints. Using (HTTPie)[https://httpie.io/] :

~~~
% http :8081/books
% http :8081/books/1
~~~

 - Try to add a book without a token (you should get "HTTP/1.1 401 Unauthorized"):
 
~~~
% http :8081/books title="Spring in Action" author="Craig Walls" publisher="Manning Publications"
~~~

 - Obtain an access token from the 
[authorization server](https://github.com/habuma/spring-auth-server) following
the directions in that project's README, being sure to approve "data.write" scope.

 - Make a request to add a book with the access token obtained from the previous
 step (this time it should work):
 
~~~
% http :8081/books title="Spring in Action" author="Craig Walls" publisher="Manning Publications" -A bearer -a <<ACCESS TOKEN>>
~~~

 - Request the list of books again to prove that the book was added.

