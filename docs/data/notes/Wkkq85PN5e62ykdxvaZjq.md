
Implicitly, every object in postgres gets operated on as a path. running `...into nuggets`, postgres runs under the hood `...into neverforget.public.nuggets`.
- We can run `SHOW search_path;` to get the path
	- the first element of the output `$user` indicates that a schema with the same name as current user (ex. a schema called `kyletycholiz`) should be searched first
	- the first schema listed (default `public`) is the default location for creating new objects 
		- when objects are referenced without qualifying a schema (ex. `...into nuggets`), the search_path is traversed, starting with public, and onto the next one (by default only 1, so we need to specify more) 
- to modify the `search_path`, we can run `SET search_path TO myschema,public;`, which will inform postgres that `myschema` should be searched first when there is no specified schema.
