
*page window* is the amount of items returned per page (ie. per request)

## Limit/Offset Pagination
- `offset` -> with this request, what is our starting point?
- `limit` -> how many records will we be retrieving per request?

A naive approach to pagination is to think like this: "We want page 3, with a page size of 10, so we should load 10 items, starting after item 20". This might look like this:
```sql
SELECT * FROM posts ORDER BY created_at LIMIT 10 OFFSET 20;
```
This approach is naive because it has some major downfalls
- If an item was added to the list while the user is switching pages, we will inevitably be skipping over items
    - this issue can be mitigated with proper ordering. For instance, if we order by `created_at`, then new items will always appear at the end.
- If an item was added at the top of the list while switching pages, then we might see the same item twice.
- The rows skipped by an `OFFSET` clause still have to be computed inside the server; therefore a large `OFFSET` might be inefficient.

These previous examples shows the limitation of the analogy of pagination as pages in a book, since the data set is not static. This goes to show that for certain apps, the very concepts of page1 and page2 don't really make any sense, because the set of data and the boundaries between loaded sections is constantly changing.

## Cursor-Based Pagination
> "Cursor-based pagination is the most efficient method of paging and should always be used where possible."

- see [[db.strategies.cursors]]

What if we could just specify the place in the list we want to begin, and then specify how many items we want to fetch? Then it doesn’t matter how many items were added to the top of the list in the meanwhile, since we have a constant pointer (cursor) to the specific spot where we left off.

An API request that implements cursor-based pagination needs 2 things to fetch more data: 
1. the current cursor position (the cursor parameter), obtained from the previous API response
2. the number of items to fetch

benefits of cursor-based pagination:
- scalable for large datasets because the [[database index|db.strategies.index]] is leveraged to prevent a full table scan
- pagination window is not affected when high-frequency writes to the database occur as the next cursor remains the same
