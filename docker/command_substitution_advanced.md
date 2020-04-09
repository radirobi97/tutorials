- `${var}`
  - Substitute the value of var.
-`${var:-word}`
  - If var is null or unset, word is substituted for var. The value of var does not change.
- `${var:=word}`
  - If var is null or unset, var is set to the value of word.
- `${var:?message}`
  - If var is null or unset, message is printed to standard error. This checks that variables are set correctly.
- `${var:+word}`
  - If var is set, word is substituted for var. The value of var does not change.

## Avoid error with `:`
Say we want to set **xx** to **"foo"** but only if it isn't already set. We can use use '${xx:="foo"}' as a shorthand way to avoid "if" or "case" blocks, but that has a side effect:
```bash
${xx:="foo"}
-bash: foo: command not found
```

  You could redirect errout to stop that complaint, but what if 'foo' were a real command? It would be executed, and that might not be what you want. You can avoid that by the leading ":" again: <br/>
  `: ${xx:="foo"}`
