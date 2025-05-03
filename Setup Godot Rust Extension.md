#development #development/game #godot #rust 
1. Setup lib crate `cargo new {project-name} --lib`
2. I prefer to structure my project like this:
```md
project-name
├── src 
|   ├── custom-node-extension.rs
│   └── lib.rs
├── godot
|   ├── scenes
|   |   ├── level.tscn
|   ├── project-name.gdextension
│   └── project.godot
├── Cargo.toml
└── .gitignore
```
3. Since rust cargo project entry point is at `/src` folder I prefer to put the godot project inside the rust project instead to save nested directory
4. I also need to put this in .gitignore to ignore unnecessary godot files on commit
```
# rust
/target  

# godot
# Godot 4+ specific ignores
godot/.godot/

# Godot-specific ignores
godot/.import/
godot/export.cfg
godot/export_credentials.cfg  

# Imported translations (automatically generated from CSV files)
godot/*.translation

# Mono-specific ignores
godot/.mono/
godot/data_*/
godot/mono_crash.*.json
```
5. This is the gdextension configuration I need:
```toml
[configuration]
entry_symbol = "gdext_rust_init"
compatibility_minimum = 4.1
reloadable = true

[libraries]
linux.debug.x86_64 =     "res://../target/debug/lib{project-name}.so"
linux.release.x86_64 =   "res://../target/release/lib{project-name}.so"
windows.debug.x86_64 =   "res://../target/debug/{project-name}.dll"
windows.release.x86_64 = "res://../target/release/{project-name}.dll"
macos.debug =            "res://../target/debug/lib{project-name}.dylib"
macos.release =          "res://../target/release/lib{project-name}.dylib"
macos.debug.arm64 =      "res://../target/debug/lib{project-name}.dylib"
macos.release.arm64 =    "res://../target/release/lib{project-name}.dylib"

```
6. Basic Cargo.toml configuration
```toml
[package]
name = "godot_rust_template"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib"]
  
[dependencies]
godot = "0.2.4"
```
7. Basic entrypoint lib.rs
```rust
use godot::prelude::*;

// Import modules here
struct GdRust;  

#[gdextension]
unsafe impl ExtensionLibrary for GdRust {}
```

