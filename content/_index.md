---
title: "libhcl"
---

# HCL parsing, in plain C

**libhcl** is a small family of [HashiCorp Configuration Language][hcl] parsers
written in **hand-written, dependency-free C** (BSD-3-Clause). No flex/bison, no
third-party libraries — just `cc *.c`. Each library is designed to be embedded
as a git submodule, instrumented to ~100 % test coverage, and fuzzed.

<ul class="cards">
  <li>
    <h3><a href="c-hcl/">c-hcl</a></h3>
    <p>The declarative <strong>subset</strong> of HCL native syntax: attributes
    and labeled blocks with scalar / list values. Tiny config reader, one
    <code>.c</code>/<code>.h</code>.</p>
  </li>
  <li>
    <h3><a href="c-hcl2/">c-hcl2</a></h3>
    <p>A from-scratch <strong>HCL2</strong> implementation: the expression
    language, configuration bodies, a cty-lite value model with conversions and
    unknowns, templates, and a JSON value reader.</p>
  </li>
</ul>

## Why hand-written?

The reference implementation ([hashicorp/hcl][hcl]) uses a ragel-generated
scanner and a hand-written parser — *not* bison. libhcl follows the same spirit
for concrete reasons:

- **Zero build dependencies.** `cc *.c` is the whole build; consumers vendoring
  a submodule need no extra tooling.
- **Testability.** Every line is reachable by tests — including out-of-memory
  paths via an allocation-budget fault-injection hook — which table-driven
  generated parsers make impractical.
- **Precise errors.** Full control over messages, with line/column positions.
- **Independence.** Being able to compile from source is a guarantee of
  independence.

## Shared design

- **BSD-3-Clause**, C99, no third-party code.
- **Parse once into an immutable AST**, walk it with accessors, then free.
- **Allocation fault-injection** in tests (`*_FAULT_INJECT` budget hook) and a
  deterministic **fuzzer** (`make fuzz`); **100 % function coverage**, ~99 %
  lines, clean under AddressSanitizer.
- Each repo carries a `GRAMMAR.md` derived directly from the lexer/parser.

[hcl]: https://github.com/hashicorp/hcl
