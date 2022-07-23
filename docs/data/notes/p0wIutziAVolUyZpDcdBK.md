
```sh
(test -d publishable/.next) && (echo 'updating dendron next...' && cd publishable/.next && git reset --hard && git clean -f && git pull) || (echo 'init dendron next' && npx @dendronhq/dendron-cli publish init --wsRoot publishable)
```
