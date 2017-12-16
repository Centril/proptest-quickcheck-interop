# proptest-quickcheck-interop

[![Build Status](https://travis-ci.org/Centril/proptest-quickcheck-interop.svg?branch=master)](https://travis-ci.org/Centril/proptest-quickcheck-interop)
[![](http://meritbadge.herokuapp.com/proptest-quickcheck-interop)](https://crates.io/crates/proptest-quickcheck-interop)

[`proptest`] is a property testing framework (i.e., the [`QuickCheck`] family)
inspired by the [Hypothesis](http://hypothesis.works/) framework for
Python.

[`quickcheck`] is another property testing framework for Rust which uses
a shrinking logic similar to Haskell's [`QuickCheck`].

**This crate provides interoperability between quickcheck and proptest.**
Currently, this is one way - if you've implemented quickcheck's
[`Arbitrary`] trait as well as [`Debug`], which is a temporary requirement,
then you may get back the equivalent [`Strategy`] in proptest.

## Status of this crate

This crate is unlikely to see major changes. Any breaking changes
are only likely to come as a result of changes in the dependencies used by
this crate, in particular proptest or quickcheck. When any of those crates
make breaking changes that affect this crate, then the major version of
this crate will be bumped.

See the [changelog] for a full list of substantial historical changes,
breaking and otherwise.

## Using the interoperability layer

Assuming that you already have a `Cargo.toml` file in your project with,
among other things, the following:

```toml 
[dependencies]

quickcheck = "0.5.0"
proptest   = "0.3.2"
```

Now add this crate to your dependencies:

```toml
[dependencies]

quickcheck = "0.5.0"
proptest   = "0.3.2"
proptest_quickcheck_interop = "1.0.3"
```

Let's now assume that `usize` is a complex type for which you have
implemented `quickcheck::Arbitrary`. You wish you reuse this in proptest
or if you simply prefer the implementation provided by quickcheck.
To do so, you can use [`from_qc`]:

```rust
// Import crates:
#[macro_use] extern crate proptest;
extern crate proptest_quickcheck_interop as pqci;

// And what we need into our scope:
use proptest::strategy::Strategy;
use pqci::from_qc;

/// Given a usize returns the nearest usize that is also even.
fn make_even(x: usize) -> usize {
    if x % 2 == 1 { x - 1 } else { x }
}

proptest! {
   /// A property asserting that make_even always produces an even usize.
   fn always_even(ref x in from_qc::<usize>().prop_map(make_even)) {
       prop_assert!(x % 2 == 0);
   }
}

fn main() {
    always_even();
}
```

If you want to control the `size` of the input generated by quickcheck
you may instead use [`from_qc_sized(size)`][`from_qc_sized`]. If you use,
[`from_qc`], then the default size used by quickcheck is used.

[`from_qc`]: https://docs.rs/proptest-quickcheck-interop/1.0.3/proptest_quickcheck_interop/fn.from_qc.html
[`from_qc_sized`]: https://docs.rs/proptest-quickcheck-interop/1.0.3/proptest_quickcheck_interop/fn.from_qc_sized.html

[changelog]:
https://github.com/Centril/proptest-quickcheck-interop/blob/master/CHANGELOG.md

[`Debug`]: https://doc.rust-lang.org/nightly/std/fmt/trait.Debug.html

[`Arbitrary`]: https://docs.rs/quickcheck/0.5.0/quickcheck/trait.Arbitrary.html

[`proptest`]: https://crates.io/crates/proptest

[`quickcheck`]: https://crates.io/crates/quickcheck

[`Strategy`]: https://docs.rs/proptest/0.3.2/proptest/strategy/trait.Strategy.html
## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.
