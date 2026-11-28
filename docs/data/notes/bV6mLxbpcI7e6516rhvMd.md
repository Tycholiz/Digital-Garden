
The last parameter of a method may be preceded by an asterisk(*), which is sometimes called the 'splat' operator. This indicates that more parameters may be passed to the function. Those parameters are collected up and an array is created.
```rb
def by_added(dir, entries)
  Open3.capture2(
    'mdls',
    '-name', 'kMDItemFSName',
    '-name', 'kMDItemDateAdded',
    '-raw',
    *entries.map(&:to_path)
  )
end
```
