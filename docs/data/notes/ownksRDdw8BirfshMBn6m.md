
# FILTER
- def - adds a filter to an aggregate function
- ex. if we are making a json array with `json_agg`, we can filter on anything that would appear in that json
	- ex. `filter (where "bucket".id is not null)`
