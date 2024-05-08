---
acl_categories:
- '@write'
- '@hash'
- '@fast'
arguments:
- display_text: key
  key_spec_index: 0
  name: key
  type: key
- display_text: milliseconds
  name: milliseconds
  type: integer
- arguments:
  - display_text: nx
    name: nx
    token: NX
    type: pure-token
  - display_text: xx
    name: xx
    token: XX
    type: pure-token
  - display_text: gt
    name: gt
    token: GT
    type: pure-token
  - display_text: lt
    name: lt
    token: LT
    type: pure-token
  name: condition
  optional: true
  type: oneof
- display_text: numfields
  name: numfields
  type: integer
- display_text: field
  multiple: true
  name: field
  type: string
arity: -5
categories:
- docs
- develop
- stack
- oss
- rs
- rc
- oss
- kubernetes
- clients
command_flags:
- write
- denyoom
- fast
complexity: O(N) where N is the number of arguments to the command
description: Set expiration for hash fields using relative time to expire (milliseconds)
group: hash
hidden: false
key_specs:
- RW: true
  begin_search:
    spec:
      index: 1
    type: index
  find_keys:
    spec:
      keystep: 1
      lastkey: 0
      limit: 0
    type: range
  update: true
linkTitle: HPEXPIRE
since: 8.0.0
summary: Set expiration for hash fields using relative time to expire (milliseconds)
syntax_fmt: "HPEXPIRE key milliseconds [NX | XX | GT | LT] numfields field [field\n\
  \  ...]"
syntax_str: milliseconds [NX | XX | GT | LT] numfields field [field ...]
title: HPEXPIRE
---
This command works exactly like [`HEXPIRE`]({{< relref "/commands/hexpire" >}}) but the expiration of the key is
specified in milliseconds instead of seconds.

## Options

The `HPEXPIRE` command supports a set of options:

* `NX` -- For each specified field, set expiration only when the key has no expiration.
* `XX` -- For each specified field, set expiration only when the key has an existing expiration.
* `GT` -- For each specified field, set expiration only when the new expiration is greater than current one.
* `LT` -- For each specified field, set expiration only when the new expiration is less than current one.

A non-volatile key is treated as an infinite TTL for the purpose of `GT` and `LT`.
The `GT`, `LT` and `NX` options are mutually exclusive.

## Example

```
TODO: to be provided.
```

## RESP2/RESP3 replies

One of the following:",
* [Array reply](../../develop/reference/protocol-spec#arrays). For each field:
    - [Integer reply](../../develop/reference/protocol-spec#integers): `-2` if no such field(s) exist in the provided hash key.
    - [Integer reply](../../develop/reference/protocol-spec#integers): `0` if the specified NX | XX | GT | LT condition has not been met.
    - [Integer reply](../../develop/reference/protocol-spec#integers): `1` if the expiration time was set/updated.
    - [Integer reply](../../develop/reference/protocol-spec#integers): `2` if the field(s) were deleted because of previously set expiration(s). This reply may also be returned when `HEXPIRE`/`HPEXPIRE` is called with 0 seconds/milliseconds or when `HEXPIREAT`/`HPEXPIREAT` is called with a past unix-time in seconds/milliseconds.
* [Simple error reply](../../develop/reference/protocol-spec#simple-errors):
    - if parsing failed, mandatory arguments are missing, unknown arguments are specified, or argument values are of the wrong type or out of range.
    - if the provided key exists but is not a hash.
* [Null reply](../../develop/reference/protocol-spec#nulls) if the provided key does not exist.