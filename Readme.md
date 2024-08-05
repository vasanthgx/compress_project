## Code Explanation

```rust
extern crate flate2;
```
This line declares that we're using an external crate (library) called `flate2`, which provides compression and decompression functionality.

```rust
use flate2::write::GzEncoder;
use flate2::Compression;
use std::env::args;
use std::fs::File;
use std::io::copy;
use std::io::BufReader;
use std::time::Instant;
```
These lines import specific items from the `flate2` crate and the Rust standard library that we'll use in our program.

```rust
fn main() {
```
This declares the main function, the entry point of our Rust program.

```rust
if args().len() != 3 {
    eprintln!("Usage: `source` `target`");
    return;
}
```
This checks if the program was called with exactly two arguments (the program name counts as the first argument). If not, it prints an error message to stderr and exits the program.

```rust
let mut input = BufReader::new(File::open(args().nth(1).unwrap()).unwrap());
```
This line opens the source file (specified by the first command-line argument) and wraps it in a `BufReader` for efficient reading.

```rust
let output = File::create(args().nth(2).unwrap()).unwrap();
```
This creates the target file (specified by the second command-line argument).

```rust
let mut encoder = GzEncoder::new(output, Compression::default());
```
This creates a new `GzEncoder` that will write compressed data to the output file.

```rust
let start = Instant::now();
```
This records the start time of the compression operation.

```rust
copy(&mut input, &mut encoder).unwrap();
```
This copies all data from the input file to the encoder, which compresses it and writes to the output file.

```rust
let output = encoder.finish().unwrap();
```
This finishes the compression and returns the completed output file.

```rust
println!(
    "Source len: {:?}",
    input.get_ref().metadata().unwrap().len()
);
```
This prints the length of the source file.

```rust
println!(
    "Target len: {:?}", 
    output.metadata().unwrap().len()
);
```
This prints the length of the compressed target file.

```rust
println!(
    "Elapsed: {:?}", start.elapsed()
);
```
This prints the time taken for the compression operation.

```rust
}
```
