# tar-wasi

[Documentation](https://docs.rs/tar-wasi)

A tar archive reading/writing library for Rust that supports the wasm32-wasi target

```toml
# Cargo.toml
[dependencies]
tar-wasi = "0.4"
```

## Reading an archive

```rust,no_run
extern crate tar_wasi;

use std::io::prelude::*;
use std::fs::File;
use tar_wasi::Archive;

fn main() {
    let file = File::open("foo.tar").unwrap();
    let mut a = Archive::new(file);

    for file in a.entries().unwrap() {
        // Make sure there wasn't an I/O error
        let mut file = file.unwrap();

        // Inspect metadata about the file
        println!("{:?}", file.header().path().unwrap());
        println!("{}", file.header().size().unwrap());

        // files implement the Read trait
        let mut s = String::new();
        file.read_to_string(&mut s).unwrap();
        println!("{}", s);
    }
}

```

## Writing an archive

```rust,no_run
extern crate tar_wasi;

use std::io::prelude::*;
use std::fs::File;
use tar_wasi::Builder;

fn main() {
    let file = File::create("foo.tar").unwrap();
    let mut a = Builder::new(file);

    a.append_path("file1.txt").unwrap();
    a.append_file("file2.txt", &mut File::open("file3.txt").unwrap()).unwrap();
}
```

# License

This project is licensed under either of

 * Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
   http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or
   http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in this project by you, as defined in the Apache-2.0 license,
shall be dual licensed as above, without any additional terms or conditions.
