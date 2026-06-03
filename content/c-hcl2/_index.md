---
title: "c-hcl2"
---

A from-scratch C implementation of **HCL2** — the heavyweight companion to
[c-hcl](../c-hcl/) (which parses only the declarative subset). Use c-hcl for a
tiny config reader; use c-hcl2 when you need the **expression language**.

> **Goal: a 100 %-HCL2-compatible C library.** The native syntax is essentially
> complete; the remaining work is the deeper `cty` type system, big-number
> precision, and the JSON *body* profile — tracked openly in
> [ROADMAP.md](https://github.com/libhcl/c-hcl2/blob/main/ROADMAP.md).

```c
#include "hcl2.h"

hcl2_ctx *ctx = hcl2_ctx_new();
hcl2_ctx_set_var(ctx, "port", hcl2_number(8080));

char err[256];
hcl2_value *v = hcl2_eval("\"api:${port + 1}\"", 17, ctx, err, sizeof err);
puts(hcl2_value_as_string(v));   // "api:8081"
```

## What works today

- **Expressions:** numbers, booleans, `null`, quoted-string templates with
  `${ }` interpolation, tuples, objects, unary `- !`, binary `+ - * / %`,
  comparison, logical `&& ||`, the conditional `?:`, parentheses, variable
  references with `.attr` / `[index]` traversal, and function calls.
- **Collections & templates:** `for`-expressions (tuple & object, with `if`
  filters and grouping `=> v...`), splat (`xs[*]`, legacy `xs.*`), heredocs
  (`<<EOF` / `<<-EOF`), template directives (`%{ if }` / `%{ for }`) with `~`
  whitespace trimming, variadic call spread `f(xs...)`, and `try` / `can`.
- **Configuration bodies:** attributes + labeled blocks, decoded lazily against
  a context.
- **cty-lite values:** type constraints + conversion (`hcl2_convert`), and
  cty-style **unknown** values with propagation.
- **JSON:** a value reader (`hcl2_parse_json`) plus `jsonencode` / `jsondecode`
  builtins.
- **Diagnostics:** line/column on both syntax and evaluation errors.
- A standard library of 21 builtins; full `\uNNNN` / `\UNNNNNNNN` string escapes.

## Quality

- **100 % function coverage**, ~99 % lines, clean under AddressSanitizer.
- Allocation fault-injection (`HCL2_FAULT_INJECT`) and a deterministic fuzzer
  (`make fuzz`).
- BSD-3-Clause, C99, zero dependencies (9 translation units).

## Links

- **Repository:** [github.com/libhcl/c-hcl2](https://github.com/libhcl/c-hcl2)
- **Grammar:** [GRAMMAR.md](https://github.com/libhcl/c-hcl2/blob/main/GRAMMAR.md)
- **Roadmap / compliance:** [ROADMAP.md](https://github.com/libhcl/c-hcl2/blob/main/ROADMAP.md)
