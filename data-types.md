# Terraform data types

Terraform has two main data types:

1. Simple types:

- string (ex: `"hello"`, `""`)
- number (ex: `100`, `-20`)
- boolean (ex: `true`, `false`)

2. Complex types:

- list: (ex: `[1, 2, 3]`), pay attention that all of the elements must have the same data type.
- set: list with unique and sorted elements (ex: `[2, 1, 3, 3]` → `[1, 2, 3]`).
- map: key/value data type, but the value must be the same type (ex: `{string: "string", str: "string"}`)
- object: key/value data type, but the value may have different data types (ex: `{string: "string", bool: true}`)
- tuple: like list but the elements can have different data types (ex: `[0, "string", true]`)
