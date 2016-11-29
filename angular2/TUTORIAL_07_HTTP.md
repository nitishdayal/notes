# Angular 2: Tour of Heroes

## HTTP

As one should expect at this point, Angular 2 provides an internal way of handling
  HTTP requests via the **Angular HTTP library**. Responses are returned in the form
  of _Observables_, though they can be converted to _Promises_.

There is very little in this chapter of the tutorial that is direclty involving HTTP
  requests; the bulk of the information is about how to create standard CRUD operations,
  and the role RxJS plays in the Angular HTTP library.

## Key Take-Aways:

- Observables > Promises (for the most part), and Angular 2 defaults to Observables
- Server communication should be handled _exclusively_ by a service.
- Header content of HTTP requests and server/API URL should be stored
  as _private properties_ of the service.
- CORS credentials are stored in the request header.
- A deep understanding of RxJS == Smooth & predictable communication w/ API