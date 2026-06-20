---
title: "c-hcl"
---

A small, dependency-free C parser for the **declarative subset of HCL native
syntax** — bodies of attributes and (optionally labeled) blocks whose values are
strings, numbers, booleans, `null`, or lists thereof.

It is meant for **configuration**, not computation: no expressions,
interpolation (`${...}`), functions, heredocs, or object-value literals. That
keeps it tiny (one `.c`/`.h`, no third-party code) and easy to embed.

```hcl
name     = "demo"
replicas = 3
tags     = ["web", "edge"]

service "api" "primary" {
  port = 8080
  upstream { host = "10.0.0.1" }
}
```

```c
#include "hcl.h"

char err[256];
hcl_doc *doc = hcl_parse(src, len, err, sizeof err);
if (!doc) { fprintf(stderr, "%s\n", err); return 1; }

const hcl_body *root = hcl_doc_root(doc);
puts(hcl_value_string(hcl_body_attr(root, "name")));   // "demo"
hcl_free(doc);
```

## Highlights

- **One translation unit** (`hcl.c` + `ast.c`), zero dependencies.
- Recursive-descent lexer + parser; `#`, `//`, `/* */` comments.
- ~99 % line coverage, OOM fault-injection, BSD-3-Clause.

## Links

- **Repository:** [github.com/libhcl/c-hcl](https://github.com/libhcl/c-hcl)
- **Grammar:** [GRAMMAR.md](https://github.com/libhcl/c-hcl/blob/main/GRAMMAR.md)
- Need expressions, `for`, templates, `cty` values? Use
  [c-hcl2](../c-hcl2/).
