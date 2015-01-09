OCaml Guidelines
================

First see, and follow, The *Mothership*
[Guidelines](https://ocaml.org/learn/tutorials/guidelines.html) (at
[ocaml.org](http://ocaml.org)).

### More Rules

Always try to add typing information to `_` like `let (_ : unit list) = …`, or
`>>= fun (_ : unit list) ->`.

Avoid catch-all cases in pattern matching (`| _ ->`).

A type should be called `t` inside its module. Most functions inside that
module should have the value of type `t` as first argument, and use labels for
(all) the other ones.


Typography:

- `function_names`, `Module_names`, `Variant_names`, `` `Polymorphic_variant_names``
⇒ **No CamelCase, ever**.
- 80 characters lines, no tabs.
- Settings for [`OCamlPro/ocp-indent`](https://github.com/OCamlPro/ocp-indent):
    - `-c strict_with=always,with=0,strict_comments=false`

Avoid exceptions as much as possible:

- Return the types `option`, or ``[ `Ok of 'a | `Error of 'b ]``.
- If you do use the exceptions, tag your function with the `_exn` suffix and
  **document** them.

Don't *haskell*:

- Avoid defining infix operators as much as possible (and try to delimit them
  with `Meaning_full_module_name.( … )`).
- The number of characters in an identifier should match its scope; it's
  programming not a math paper.
- Type hackery is cool when it brings safety/readability but not just for fun.

### A Project

Remember that in OCaml, doing **huge refactorings is very easy**!

Start with one single file like there:
[`smondet/build-docs-workflow`](https://github.com/smondet/build-docs-workflow).

Then, the projects grows:

- Everything should be in a **library**; without module initializations (most
  important: no side effects or C-stub calls when a module loads; prefer `init`
  functions if needed).
- An application is a `main.ml` file that just parses the command line (and/or
  environment variables) and calls library functions (and that's all).

Bigg-ish projects should look like:

```
\__ README.md
\__ LICENSE
\__ CONTRIBUTING.md
\__ src/
 |  \__ app/
 |   |  \__ main.ml
 |  \__ doc/
 |  \__ lib/
 |  \__ test/
```

Smaller projects (like one-module-libraries) can of course be more “flat.”

When a module can be extracted from a library, it should become it's own
library (at the “package level”).

### Good Stuff

For reading, and using:

- Everything [by D. Bünzli](http://erratique.ch/software) (except sometimes the
  naming of the libraries/modules …).
- The
  [Cohttp](https://github.com/mirage/ocaml-cohttp/)/[Conduit](https://github.com/mirage/ocaml-conduit)
  family (with Lwt).
- [`mirleft/ocaml-tls`](https://github.com/mirleft/ocaml-tls),
  [`c-cube/cconv`](https://github.com/c-cube/cconv),
  [`ocamllabs/ocaml-ctypes`](https://github.com/ocamllabs/ocaml-ctypes).

### Toxic Stuff

- Big non-portable blobs like `Core_kernel` and its dependencies.
- Camlp4.
- `Obj.magic`, or `%identity` used as `extern` (which are the same).


