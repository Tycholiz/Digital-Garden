
- `:map` - recursive version of mapping command
- `:noremap` - non-recursive version of mapping command
    - "Recursive" means that the mapping is expanded to a result, then the result is expanded to another result, and so on.
- below, `j` will be mapped to `gg`. `Q` will also be mapped to `gg`, because `j` will be expanded for the recursive mapping. `W` will be  mapped to `j` (and not to `gg`) because `j` will not be expanded for the non-recursive mapping.
    - The expansion stops when one of these is true:
        1. the result is no longer mapped to anything else.
        2. a non-recursive mapping has been applied (i.e. the "noremap" [or one of its ilk] is the final expansion).
```vim
:map j gg
:map Q j
:noremap W j
```
- since VIM is a **modal editor**, there are corresponding mapping commands for each mode
    - ***ex.*** - `:imap`, `:nmap`, `:map!` (*i mode* and command line)

- Recursive map - the rhs says to the lhs "if you get triggered, when I get triggered. It stops with me, and nothing past me gets triggered"
- Non-recursive map (nore)- the rhs says "When you get triggered, I may server as the lhs of another mapping that will trigger something else."
- map is the "root" of all recursive mapping commands. 
    - The root form applies to "normal", "visual+select", and "operator-pending" modes. 
- "Recursive" means that the mapping is expanded to a result, then the result is expanded to another result, and so on.
- The expansion stops when one of these is true:
    1. the result is no longer mapped to anything else.
    2. a non-recursive mapping has been applied (i.e. the "noremap" [or one of its ilk] is the final expansion).
- At that point, Vim's default "meaning" of the final result is applied/executed.
- "Non-recursive" means the mapping is only expanded once, and that result is applied/executed.
    - Example:
```vim
nmap K H
nnoremap H G
nnoremap G gg
```
- The above causes K to expand to H, then H to expand to G and stop. It stops because of the nnoremap, which expands and stops immediately. The meaning of G will be executed (i.e. "jump to last line"). At most one non-recursive mapping will ever be applied in an expansion chain (it would be the last expansion to happen).
- The mapping of G to gg only applies if you press G, but not if you press K. This mapping doesn't affect pressing K regardless of whether G was mapped recursively or not, since it's line 2 that causes the expansion of K to stop, so line 3 wouldn't be used.
