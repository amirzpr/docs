---
acl_categories:
- '@read'
- '@hash'
- '@fast'
arguments:
- display_text: key
  key_spec_index: 0
  name: key
  type: key
- display_text: numfields
  name: numfields
  type: integer
- display_text: field
  multiple: true
  name: field
  type: string
arity: -4
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
- readonly
- fast
complexity: O(N) where N is the number of arguments to the command
description: Returns the TTL in milliseconds of a hash field.
group: hash
hidden: false
key_specs:
- RO: true
  access: true
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
linkTitle: HPTTL
since: 8.0.0
summary: Returns the TTL in milliseconds of a hash field.
syntax_fmt: HPTTL key numfields field [field ...]
syntax_str: numfields field [field ...]
title: HPTTL
---
Like [`HTTL`]({{< relref "/commands/httl" >}}) this command returns the remaining TTL (time-to-live) of a key that has an
expiration set, but in milliseconds instead of seconds.

The return value in case of error is one of:

* The command returns `-2` if the hash key field does not exist.
* The command returns `-1` if the hash key field exists but has no associated expiration.

## Example

```
TODO: to be provided.
```

## RESP2/RESP3 replies

One of the following:
* [Null reply]({{< relref "/develop/reference/protocol-spec" >}}#nulls) if the provided key does not exists.
* [Array reply]({{< relref "/develop/reference/protocol-spec" >}}#arrays). For each field:
    - [Integer reply]({{< relref "/develop/reference/protocol-spec" >}}#integers): `-2` if no such field exists in the provided hash key.
    - [Integer reply]({{< relref "/develop/reference/protocol-spec" >}}#integers): `-1` if the field exists but has no associated expiration set.
    - [Integer reply]({{< relref "/develop/reference/protocol-spec" >}}#integers): the TTL in milliseconds.