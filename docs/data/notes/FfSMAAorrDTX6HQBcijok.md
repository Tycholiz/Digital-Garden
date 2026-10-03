
`=~` allows us to do a Regex check in Ruby:
```rb
# don't throw if Order equals modified, modified_reverse, added, or added_reverse
abort 'Unrecognized order type.' unless Order =~ /(modified|added).*/
```
