
GROQ (Graph-Relational Object Queries) is a declarative query language that shares a lot of similarities with [[GraphQL|graphql]].
- It is used by...
    - describing exactly what information your application needs
    - potentially joining together information from several sets of documents
    - then stitching together a very specific response with only the exact fields you need.

```
*[_type == "post" && slug.current == $slug] | order(publishedDate, title) {
    title,
    author,
    publishedDate
}[0..5]
```
Means:
`*` 
- select all documents

`[_type == 'post' && slug.current == $slug]` 
- filter the selection down to documents with the type "post" and those of them who have the same slug to that we have in the parameters

[0] 
- select the first and only one in that list

## Breakdown
We think of GROQ statements as describing a data flow from left to right.

### Filter
the first set of `[]` is the filer. We return data the type of data we are seeking, based on what parameters etc.
- ex. return data that are of either type `Movie` or `Person`, and `popularity > 15`, or `releaseDate > 2016-04-25`

### Slice
the second set of `[]`
Allows us to determine which items we will get back from the query
ex. first 5

### projection
what's found between `{}`
ex. return `title`, `author`, and `publishedDate` from the query for all blog posts.

### Ordering
what's found after the ` | `
We can order by secondary properties, if the first property is the same (e.g. in cases where there are many items with the same `year` value)

# Resources
[Cheat sheet](https://www.sanity.io/docs/query-cheat-sheet)
