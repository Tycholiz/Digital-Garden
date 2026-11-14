
### Controlled Form
```ts
const OuterForm = () => {
  const [inputValue, setInputValue] = useState<string>("");
  const handleChange = (event: React.FormEvent<HTMLInputElement>) => {
    const { value } = event.currentTarget;
    setInputValue(value)
  };
  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    // submit logic
  }
  return (
    <form action="" method="get" onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Enter your name: </label>
        <input
          type="text"
          name="name"
          id="name"
          placeholder="Enter your name"
          value={inputValue}
          onChange={handleChange}
          required
        />
      </div>
      <div>
        <input type="submit" value="Submit!" />
      </div>
    </form>
  )
}
```