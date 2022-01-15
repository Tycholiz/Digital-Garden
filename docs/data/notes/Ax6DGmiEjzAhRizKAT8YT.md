
# Runtime path
- vim looks for scripts in various directories. The directories that vim will look in is known as the `runtime path`
- to see it, type `:set runtimepath?`

# Runtime directory
Assuming that you're using some flavor of Unix, your personal runtime directory is ~/.vim. This is where you should put any plugin used only by you.
You should not install any plugins into the $VIMRUNTIME directory. That directory is intended for plugins distributed with Vim.
