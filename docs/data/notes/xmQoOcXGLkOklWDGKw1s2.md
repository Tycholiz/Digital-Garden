
Direnv is an environment variable manager for your shell
- ex. maybe you want to have `NPM_CONFIG` variable set to something else depending on what directory you are in.
- It does this by [[hooking|general.patterns.hook]] into the shell to load/unload env variables, depending on current directory.

Direnv allows us to have project-specific env variables, preventing us from having to clutter a `/.profile` file.

Before each prompt, existence of `.envrc` file is checked in current and parent directories.
- If exists, it is loaded into a sub shell and all exported variables and then captured by Direnv and made available to the current shell.

spec: You could also use Direnv to load the correct version of node with `nvm`.

### Procedure
1. create an `.envrc` file with the contents of what will get run when entering the directory:
```sh
# .envrc
export NPM_TOKEN=glpat-638g1hnfxn32nf2
```

2. hook direnv into the shell: https://direnv.net/docs/hook.html

3. run `direnv allow` in the directory to source in the file, and mark it as safe so it will be sourced automatically in the future.
