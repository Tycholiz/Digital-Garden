
# Pagination
## Limit/Offset Pagination
A naive approach to pagination is to think like this: "We want page 3, with a page size of 10, so we should load 10 items, starting after item 20". This might look like this:
```
SELECT * FROM posts ORDER BY created_at LIMIT 10 OFFSET 20;
```
This approach is naive because it has some major downfalls
- If an item was added to the list while the user is switching pages, we will inevitably be skipping over items (ex. product site, chat app)
- If an item was added at the top of the list while switching pages, then we might see the same item twice.
These previous examples show that the mental model of pages in a book is a poor analogy for pagination, since the data set is not static. This goes to show that for certain apps, the very concepts of page1 and page2 don't really make any sense, because the set of data and the boundaries between loaded sections is constantly changing.

## Cursor-Based Pagination
What if we could just specify the place in the list we want to begin, and then specify how many items we want to fetch? Then it doesn’t matter how many items were added to the top of the list in the meanwhile, since we have a constant pointer (cursor) to the specific spot where we left off.

A cursor-based paginator needs 2 things to fetch more data: the current cursor position, and the number of items to fetch.

*"Cursor-based pagination is the most efficient method of paging and should always be used where possible."*
- besides cursor-based pagination, the most
