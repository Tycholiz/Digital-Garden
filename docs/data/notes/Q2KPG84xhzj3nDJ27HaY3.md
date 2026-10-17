
Delete lost or "orphaned" objects (ie. those that are unaccessable by any ref). Any commit that cannot be accessed through a branch or tag is unreachable.
- prune is a garbage collection command, and considered a child command of `git gc` (in other words, `gc` runs `prune`)
