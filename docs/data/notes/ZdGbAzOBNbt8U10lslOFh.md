
## Should this be a table or not?
- can you imagine the current data being *expanded* in the future?
	- ex. imagine we had a Book table, which included 3 properties: genre1, genre2, genre3. Now if we decided that each book should have 4 potential genres, we would have to change the structure. The solution would be to have a Books table and a Genres table
- Is the data being held in a table "genericable"? If you are tempted to combine different concepts into something that has a common thread, it might be a sign that they should be different tables. Framed another way, if you find yourself in a position where it would be difficult to extend a table, it should give us pause.
	- ex. Imagine we have to keep track of `InvoiceStatus`, `BackOrderStatus`, `ShipViaCarrier`, `CreditStatus`, and `CustomerStatusType`. Each of these concepts has a common thread, which is that aside from their respective PKs, they only have a single text field. We might therefore think it appropriate to combine all 5 concepts into a single generic table. However, this becomes problematic when we start to query information. What results is a necessity to JOIN ON many different fields, and to use aliases to distinguish multiple "versions" of the same table:
- if we were to look at 5 rows of a single table and noticed that 3/5 of the rows have an identical value for one of the columns, that would be a sign that the column should be its own table
	- ex. we have a table called `shoes` and there is a field to describe what type of shoe it is (business, casual etc). If we were to query some records, we would find that 'business' would show up a lot in the 'type' column. This is a sign that we should create a "shoe type" table.

```sql
SELECT *
	FROM Customer
	JOIN GenericDomain as CustomerType
	ON Customer.CustomerTypeId = CustomerType.GenericDomainId
	  and CustomerType.RelatedToTable = "Customer"
	  and  CustomerType.RelatedToColumn = "CustomerTypeId"
	JOIN GenericDomain as CreditStatus
	ON Customer.CreditStatusId = CreditStatus.GenericDomainId
	  and CreditStatus.RelatedToTable = "Customer"
	  and CreditStatus.RelatedToColumn = "CreditStatusId"
```
	- on the other hand, if we separate them all out as seperate tables, then it greatly simplifies our query:
```sql
SELECT * FROM Customer  
	JOIN CustomerType 
		ON Customer.CustomerTypeId = CustomerType.CustomerTypeId  
	JOIN CreditStatus    
		ON Customer.CreditStatusId = CreditStatus.CreditStatusId
```
