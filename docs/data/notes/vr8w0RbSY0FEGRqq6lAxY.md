
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

