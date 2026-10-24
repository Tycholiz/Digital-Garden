
#### Loop over regular files in directory
```sh
for f in *; do
  echo "File -> $f"
done
```

#### Loop over all files in directory (inc. dotfiles)
```sh
for f in $(ls -a ~/Documents); do
    echo "Processing $f file.."
done
```
