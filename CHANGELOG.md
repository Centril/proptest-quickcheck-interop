# 0.1.0

## New Additions

This is the initial version. This version provides:

- `from_qc::<T>()` - a function to generate a `Strategy` so long as `T`
  implements `quickcheck::Arbitrary`.

- `from_qc_sized::<T>()` - allows the user to control the size of inputs in
  quickcheck.

- `QCStrategy`, a proptest `Strategy` produced by `from_qc` and `from_qc_sized`.

- `QCValueTree`, the proptest `ValueTree` used for `QCStrategy`.