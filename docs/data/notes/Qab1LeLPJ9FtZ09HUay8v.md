
#### Append to a pathname
```rb
downloads_dir = Pathname.new(ENV['HOME']).join('Downloads')
```

#### Get files/folders of a directory as an array
```rb
Pathname.new(ENV['HOME']).join('Downloads').children
```

#### Get basename (ie. all leaf nodes of a path)
```rb
Pathname.new(ENV['HOME']).basename
```
