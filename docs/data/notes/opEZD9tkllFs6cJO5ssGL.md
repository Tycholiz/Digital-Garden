
### TinyURL example
Imagine a database where the main data entry is a shortened URL. In this structure, the shortform of the URL is matched with the longform. At some point, it will make sense to delete entries. For instance, the user may choose to have the URL only last for 24 hours, after which point it will no longer be a valid URL. In this scenario, all we need to do is introduce an `expired` property/column to the object/table, and check it any time a user tries to access the shortform URL. Now this leads to the principle issue of cleaning: when do we go about deleting items from the database?
- If we chose to continuously search for expired links to remove them, it would put a lot of pressure on our database. Instead, we can slowly remove expired links and do a lazy cleanup.
- additionally, 
    - Whenever a user tries to access an expired link, we can delete the link and return an error to the user.
    - A separate Cleanup service can run periodically to remove expired links from our storage and cache. This service should be very lightweight and scheduled to run only when the user traffic is expected to be low.
    - We can have a default expiration time for each link (e.g., two years).
    - After removing an expired link, we can put the key back in the key-DB to be reused.
    - Should we remove links that haven’t been visited in some length of time, say six months? This could be tricky. Since storage is getting cheap, we can decide to keep links forever.
