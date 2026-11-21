# ddnnife

> A d-DNNF reasoner.

`ddnnife` takes a smooth d-DNNF and can be used to compute the cardinality of single features, all features, or partial configurations.
Furthermore, it can compute SAT queries, core/dead features, atomic sets, enumerate complete valid configurations and produce uniform random samples.

In addition to the Rust API, a [CLI][cli] is provided as well as bindings for Kotlin/Java and [Python][pypi].
See the [project repository][repository] for more details

## Usage

The following loads a d-DNNF from file and calculates the count under assumptions:

```rust
use std::path::Path;
use ddnnife::Ddnnf;
use num::BigUint;

let ddnnf: Ddnnf = Ddnnf::from_file(Path::new("./tests/data/small_ex_c2d.nnf"), None);

assert_eq!(BigUint::from(2u32), ddnnf.count(&[3]));
```

See the documentation for more usage examples.

[cli]: https://crates.io/crates/ddnnife_cli
[pypi]: https://pypi.org/project/ddnnife
[repository]: https://github.com/practical-kc/ddnnife
