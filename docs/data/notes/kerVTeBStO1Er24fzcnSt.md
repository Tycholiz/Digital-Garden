
### Primary Key
- a foreign key of a table can also be made into its PK
	- ex. Imagine we had a `public.user` table that held more public info about a user like `username`, `interests`, and a `private.user_account` which held sensitive info like `password` and `email`. Since it is logical to think of the `user_account` table as just an extension of the `user` table, we can simply use the already existing `id` from the `user` table, and make `user_id` the primary key of `user_account`, which references the `user` table.
- PK is a type of candidate key.

#### Surrogate Key
- A surrogate key is a (likely automatically generated) primary key that is chosen because there is nothing else that uniquely identifies a row. Put another way, the only reason it exists is for the purposes of our database.
	- By contrast, a key that actually means something outside our database (ex. an item SKU)
- The danger in just relying on surrogate keys is if we have a situation where there are other columns that should uniquely identify the row, then it leaves us open to developer error
	- ex. what if we have an `items` table, and 2 rows are `id` and `SKU`. There's nothing stopping us from inputting the same SKU twice, which result in a duplicated row with different ids.

### Foreign Key
- for foreign keys, we definitely want one column to uniquely identify a row (likely a primary key), otherwise we would have to reference multiple columns in the other table.
- foreign keys can reference tables or columns
- the foreign key is on the many side of a 1:many relationship
- foreign keys are not unique
- foreign keys can be null

### Candidate key
- a column or combination of columns that uniquely identifies a row.
	- ex. if the rules of our application dictated that emails in the db must be unique, then email could qualify as a candidate key 
- Any column or column combination that can guarantee uniqueness is referred to as a *candidate key*
	- if this uniqueness is achieved by 2 or more columns, then it is referred to as a composite key. Therefore, composite key is a type of candidate key.

#### Differences with PK
- a table can have multiple candidate keys.
- candidate key column(s) can have null values

#### Composite key
- If a table does not have a single column that qualifies for a Candidate key, then you have to select 2 or more columns to make a row unique
	- ex. if no EmployeeID or SocialSecurityNumber that could qualify as a candidate key, then you can make FullName + DoB as composite primary key (though there is small risk of duplication still)
- Using composite keys could save us from having to perform lookups before making insertions.

### Super key
- if you add any column to a PK, then it becomes a super key
	- ex. EmployeeID + FullName

## Constraining tables
- foreign key constraints ensure that there is another table in existence that has the same column-value
	- ex. if we have an attribute `product_no` in an Order table, then we should expect that attribute to reference an `id` from a Product table
	- maintains the referential integrity between two related tables
- when we create a foreign key, we essentially are saying "a row cannot be entered into this table, unless we have an established reference to the other table"
	- in other words, if the id in the other table (the *referenced table*) doesn't exist, then neither can the row in the *referencing table* (the table with the FK) 

## Constraining columns
- below, we reference columns `c1` and `c2` from `table2`, calling the `b` and `c` in our table (`table1`)
```sql
CREATE TABLE table` (
  a integer PRIMARY KEY,
  b integer,
  c integer,
  FOREIGN KEY (b, c) REFERENCES table2 (c1, c2)
);
```

1:1 and 1:many relationships are achieved by the use of key contraints (foreign keys)
- ex. User table with an `id` (PK) property can be used as the value for the `owner_id` (FK) property on the Nugget table


### Shoestore example
Lets start with a  sensible table: `product` (ex. Air, Jordans). Since we are going to keeping inventory in the store, we will need actual instances of those shoes, which we can call the `item` table. For instance, this table will hold the information about the shoe's size, its particular color, its price etc. (All of this information is specific to a shoe instance, and doesn't make sense to be included in the product table). Next, we might be tempted to add a column to the `product` table indicating what type of shoe it is (business, running etc). What we would find when querying data however, is that 'business' would be repeated many times, which is a sign that it should be its own table. So we create a table called `product_type` and relate it to the `product` table with ids. Next, we will have to consider that we will have to store data on the actual sale of the item (called `sales_item`). There is a distinct but important difference between difference between the `item` table and the `sales_item`. `item` is a table that includes the actual instances of the shoe in inventory. `sales_item` is the item that is being sold. Therefore, it is going to be part of another table `sales_order`. In `sales_item`, we will include information like the specific tax rate that was used (since different people are subject to different tax rates), the discount that was applied, the quantity bought etc. Next, in our `sales_order` table, we will include the form of payment (credit, cash), what time the order was placed etc. 
![8da5a5b7c2d3633de044d693884219ee.png](:/7d7f11b898814196afe51643fe2a6f8d)
