# 2.0.0

Updating proptest to version 4.1 and quickcheck to 6.0.0.
This is a breaking change wrt. public dependencies, so therefore the
major version bump.

# 1.0.5

- Loosened bounds on `QCValueTree`. This should have zero effect on client code.

# 1.0.4

## Fixes

- Removed `impl Clone for QCValueTree<Vec<u32>>` from public API, this was
  previously noted as forever unstable, but it was moved to `mod tests` to
  make this clearer and not subject to accidental breakage.

# 1.0.3

## Fixes

- Fixed README.

# 1.0.2

## Fixes

- More documentation fixes.

# 1.0.1

## Fixes

- Documentation fixes.

# 1.0.0

## New Additions

This is the initial version. This version provides:

- `from_qc::<T>()` - a function to generate a `Strategy` so long as `T`
  implements `quickcheck::Arbitrary`.

- `from_qc_sized::<T>()` - allows the user to control the size of inputs in
  quickcheck.

- `QCStrategy`, a proptest `Strategy` produced by `from_qc` and `from_qc_sized`.

- `QCValueTree`, the proptest `ValueTree` used for `QCStrategy`.