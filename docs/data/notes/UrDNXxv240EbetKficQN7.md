
### Use `while(true)` and `break`
- we can make a while loop that will continuously execute code until `break` is reached.
```js
while(true) {
	try {
		await client.query('select true as "Connection test";')
		break //if the promise above resolves, then break will be run and we will exit the while-block 
	} catch(e) {
		await sleep(1000)
	}
}
```
