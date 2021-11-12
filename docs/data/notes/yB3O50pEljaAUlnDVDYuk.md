```
*[_type == "post" && slug.current == $slug][0]
```
Means:
`*` 
- select all documents

`[_type == 'post' && slug.current == $slug]` 
- filter the selection down to documents with the type "post" and those of them who have the same slug to that we have in the parameters

[0] 
- select the first and only one in that list

## Breakdown
### Filter
the first set of `[]` is the filer. We return data the type of data we are seeking, based on what parameters etc.
ex. return data that are of either type `Movie` or `Person`, and `popularity > 15`, or `releaseDate > 2016-04-25`

### Slice
the second set of `[]`
Allows us to determine which items we will get back from the query
ex. first 5

### Information returned
what's found between `{}`
ex. return `title`, `author`, and `publishedDate` from the query for all blog posts.

### Ordering
what's found after the ` | `

# Resources
[Cheat sheet](https://www.sanity.io/docs/query-cheat-sheet)