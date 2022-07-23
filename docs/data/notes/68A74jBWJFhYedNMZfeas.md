
#### Create new file
```rb
File.new("out.txt", "r")
```
r - Read only. The file must exist.
w - Create an empty file for writing.
a - Append to a file.The file is created if it does not exist.
r+ - Open a file for update both reading and writing. The file must exist.
w+ - Create an empty file for both reading and writing.
a+ - Open a file for reading and appending. The file is created if it does not exist.

#### Open + Read file
```rb
file = File.open("users.txt")
```

Read whole file:
```rb
file_data = file.read
# "user1\nuser2\nuser3\n"
```

Iterate over each line
- `chomp` will remove newline (`\n`) characters
```rb
file_data = file.readlines.map(&:chomp)
# ["user1", "user2", "user3"]
```
