
#### Update statement to change jsonb value
```sql
UPDATE test
SET data = replace(data::TEXT,': "my-name"',': "my-other-name"')::jsonb
WHERE id = 1;
```

#### Use variable/function within JSON insertion
Easiest way is to just use string concatenation (`||` or `concat()`)

If value is integer:
```sql
('[{"specCode": {"name": "Telephone Number", "text": "TEL_NUM"}, "specValue": {"code": null, "text":' || tel || '}}]')::json
```

If value is not integer (difference in quotes used):
```sql
('[{"specCode": {"name": "Telephone Number", "text": "TEL_NUM"}, "specValue": {"code": null, "text":"' || tel || '"}}]')::json
```

#### Overwriting json value
Here, we move values from the `list_price` and `paid_price` columns into a combined jsonb column:
```sql
update app_public.invoice_items as invoice_items
	set
		amount = concat('{"list_price":', invoice_items.list_price, ',"paid_price":', invoice_items.paid_price, '}')::jsonb
	where id = ii_row.id;
-- result {"list_price": 1465, "paid_price": 817}
```