
whenever rows in the parent (referenced) table are deleted/updated, the respective rows in the child (referencing) table with matching FK will be deleted/updated as well
- put another way, not only do we delete an object (ex. a schema), but we delete everything contained within that object (ex. tables)

- ex. if we mark the `nugget_id` of `nugget_bucket` table as CASCADE, when a row in the `nugget` table is deleted, all rows in `nugget_bucket` table that reference that nugget will also be deleted
