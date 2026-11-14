
#### Run shell command
```yml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: run a script
      run: |
        echo "here it is boys!"
        ls main
```

#### `run` a command in a specific directory
```yml
- name: Clean temp directory
  run: rm -rf *
  working-directory: ./temp
```