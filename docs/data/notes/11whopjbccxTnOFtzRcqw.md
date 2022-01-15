
# Glob Patterns
`*` is not the only globbing primitive. Other globbing primitives are:
- `?` - matches any single character
- `[abd]` - matches any character from a, b or d
- `[a-d]` - matches any character from a, b, c or d

- `.` has no meaning as a glob.

## Globbing vs Regex
- spec: Globbing is interpreted by the shell, while regex is interpreted by a program (like `rename`)
- commands surrounded by single-quotes are not interpreted by the shell.
