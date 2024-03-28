
A DTO is an object that defines how the data will be sent over the network. 
- Its purpose is to carry data between processes in order to reduce the number of method calls.

The DTO is used to encapsulate data we send from one endpoint to another. Use DTO to define interfaces for input and output for endpoints in your system

When we are working inter-process, each call is expensive. As a result, we want to pack more data in each call.
- we could accomplish this by adding many parameters, but this gets unwieldly fast.
- instead, we can create a DTO to hold all of the data for the call.

Usually an assembler is used on the server side to transfer data between the DTO and any domain objects.

DTOs are normally implemented as POJOs, and are flat data structures that contain no business logic. 

## UE Resources
- https://www.baeldung.com/java-dto-pattern