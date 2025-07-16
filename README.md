# Python-style Object Allocator in Rust (obmalloc)

This project implements a Python-inspired object allocator in Rust, focusing on memory pool management and custom allocation strategies similar to CPython's obmalloc.

## Features

- **Custom memory allocator** implementing `GlobalAlloc` trait
- **Pool-based memory management** with linked list structure
- **Reference counting** using `Arc<Mutex<u32>>` for safe concurrent access
- **Memory pool headers** tracking allocation metadata
- **Thread-safe operations** through mutex-protected reference counts
- **Comprehensive error handling** for allocation failures

## Implementation Details

The allocator features:

- `PoolHeader` struct tracking:
  - Reference counts
  - Free block pointers
  - Pool linking (next/previous)
  - Allocation metadata (size index, arena index, offsets)
- `PoolHeaderRef` union for efficient memory usage
- Custom allocation functions with proper alignment handling
- Testable interface with panic-safe execution

## Getting Started

### Prerequisites
- Rust 1.60+ (edition 2021)
- Cargo package manager

### Building
```bash
git clone https://github.com/yourusername/obmalloc.git
cd obmalloc
cargo build
```

### Running Tests
```bash
cargo test
```

### As a Static Library
Configured in `Cargo.toml` for integration into other projects:
```toml
[lib]
crate-type = ["staticlib"]
```

## Usage Example
```rust
use obmalloc::{allocate_pool, manage_memory_pools};

fn main() {
    // Allocate raw memory block
    let block = unsafe { allocate_pool(1024).unwrap() };
    
    // Manage memory pools
    manage_memory_pools().expect("Memory management failed");
    
    // ... use allocated memory ...
}
```

## Performance Considerations
The implementation balances safety and performance by:
1. Using `ManuallyDrop` for precise control over ARC drops
2. Leveraging Rust's ownership system for memory safety
3. Implementing mutex guards only when modifying reference counts
4. Providing direct memory access through raw pointers where appropriate

## License
MIT License - see [LICENSE](LICENSE) file for details

## Contribution
Contributions are welcome! Please submit issues and pull requests through GitHub.
