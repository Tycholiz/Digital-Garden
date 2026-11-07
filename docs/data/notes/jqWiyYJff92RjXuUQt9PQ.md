
An api is really an interesting thing, because it allows you to do the work once, and have its effect (result) be replicated ad infinitum. Consider the way the cursor works in WoW. The cursor points over something, and somehow it knows what it is hovering over. Well, For a mouse to able to locate something in that sense, it has to have the coordinates of the item (ex. a chest). If the coordinates that you are hovering over match the coordinates of the chest, then you know you are hovering over the chest. The function to determine this is complex, but it is at the end of the day reducible to a function. We can still walk away with an atomic piece that can be transported anywhere. So imagine we take the functionality of determining where the coorinate of the cursor is and wrap it up into a function. Now all we need to do is run that function every time the mouse moves. We have just created an API that easily replicable across the entire application. 

we can make a simplifying assumption in the case of dataflow through an API: it is reasonable to assume that all the servers will be updated first, and all the clients second. Thus, you only need backward compatibility on requests, and forward compatibility on responses.

## Best Practices
- follow the single responsibility principle to build the API and [[paginate|api.pagination]] the API response by default
- simplify the API by reducing the number of API parameters and define default parameter values on heavily used APIs
- provide consistent API endpoints through naming conventions, input parameters, and output responses
- include meaningful error messages in the API response for the ease of troubleshooting
- rate limit the API and design for scale
- avoid breaking changes to the API

### Robustness principle (Postel's law)
"be conservative (strict) in what you send, be liberal in what you accept"
- programs that send messages to other machines (or to other programs on the same machine) should conform completely to the specifications, but programs that receive messages should accept non-conformant input as long as the meaning is clear.

# UE Resources
[difference between API and SDK](https://nordicapis.com/what-is-the-difference-between-an-api-and-an-sdk/#:~:text=By%20definition%2C%20an%20SDK%20is,to%20allow%20communication%20between%20applications.)
