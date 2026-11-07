
note: this is Ruby code that is more appropriate for scripting tasks

### Get array of files from a folder path
```rb
downloads_folder = Pathname.new(ENV['HOME']).join('Downloads')
# call .children on a path to get the array
all_entries_without_dotfiles = downloads_folder.children.reject { [p] p.basename.to_path.start_with?('.') }
```
