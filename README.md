scratchpad
==========

A Rust library providing a stack-like dynamic memory pool with double-ended
allocation support.

Features include:

- User-defined backing storage of data (static arrays, boxed slices, or
  mutable slice references).
- Allocation of any data type from any scratchpad instance.
- Double-ended allocation support (allocations from the "front" are separate
  from the "back", but share the same memory pool).
- Use of lifetimes to prevent dangling references to allocated data.
- Low runtime overhead.
- Support for `no_std` usage.

## Usage

Add this to your `Cargo.toml`:

```toml
[dependencies]
scratchpad = "0.1"
```

and this to your crate root:

```rust
#[macro_use]
extern crate scratchpad;
```

## Rust Version Support

The minimum supported Rust version is 1.25 due to use of the `repr(align)`
attribute.

## `no_std` Support

`scratchpad` doesn't require the Rust standard library, although it makes use
of it by default (via the `std` crate feature) to provide support for using
`Box` as backing memory. For `no_std` support, the `std` feature must be
disabled in your `Cargo.toml`:

```toml
[dependencies]
scratchpad = { version = "0.1", default-features = false }
```

`Box` support is still available for `no_std` builds when using a nightly
toolchain by enabling the `unstable` crate feature.

## Unstable Features

The `unstable` crate feature provides some additional functionality when using
a nightly toolchain:

- `ByteData` trait implementations for `u128`/`i128`.
- Declaration of the function `Scratchpad::new()` as `const`, allowing it to
  be used directly to initialize static variables (`lazy_static` crate or
  similar workaround needed otherwise).
- Support for using `Box` as the storage type for allocations and marker
  tracking, regardless of whether the `std` feature is enabled (`alloc`
  library is used if `std` is disabled).

Simply add the `unstable` feature to your `Cargo.toml` dependency:

```toml
[dependencies]
scratchpad = { version = "0.1", features = ["unstable"] }
```
