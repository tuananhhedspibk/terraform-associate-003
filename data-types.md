# Terraform data types

Terraform has two main data types:

1. Simple types:

- string (ex: `"hello"`, `""`)
- number (ex: `100`, `-20`)
- bool (ex: `true`, `false`)

2. Complex types:

- list: (ex: `[1, 2, 3]`), pay attention that all of the elements must have the same data type.
- set: list with unique and sorted elements (ex: `[2, 1, 3, 3]` → `[1, 2, 3]`).
- map: key/value data type, but the value must be the same type (ex: `{string: "string", str: "string"}`)
- object: key/value data type, but the value may have different data types (ex: `{string: "string", bool: true}`)
- tuple: like list but the elements can have different data types (ex: `[0, "string", true]`)

Here are some snip code about all of the upper data types

For simple types: `string`, `number`, `bool`

```terraform
variable "str_ex" {
  type = string
  default = "str_val"
}

variable "num_ex" {
  type = number
  default = 99
}

variable "bool_ex" {
  type = bool
  default = true
}
```

For complex types: `list`, `set`, `map`, `object`, `tuple`

```terraform
variable "list_ex" {
  type = list(string)
  default = ["str1", "str2"]
}

variable "set_ex" {
  type = set(string)
  default = ["a", "b", "c"]
}

variable "map_ex" {
  type = map(any)
  default = {
    name = "test",
    age  =  30
  }
}

variable "object_ex" {
  type = object({
    name = string
    age = number
  })
  default = {
    "name" =  "test",
    "age"  =  30
  }
}

variable "tuple_ex" {
  type = tuple([
    object({
      name = string,
      age = number
    }),
    object({
      name = string,
      age = number
    })
  ])
  default = [{
    name = "test_1",
    age = 29
  }, {
    name = "test_2",
    age = 31
  }]
}
```
