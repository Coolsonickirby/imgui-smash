# imgui-smash: A modified fork of imgui-xeno to get Dear ImGui backend working for Smash

## Current ImGUI Version: 1.92.7 WIP

A [Dear ImGui](https://github.com/ocornut/imgui) backend that leverages NVN to support and target Super Smash Bros. Ultimate specifically (will probably also work with other games that imgui-xeno doesn't work with.)

This is specifically the main host plugin build that exports API functions which other plugins can then use to create their own ImGUI windows.

## Installing (Plugin User)
To install the imgui-smash host plugin, ensure you have [skyline](https://github.com/skyline-dev/skyline/releases/tag/beta) set up, then do the following:
- Download `libimgui_smash.nro` from the releases
- Place the plugin in `sd:/atmosphere/contents/<Title ID>/romfs/skyline/plugins` (where `<Title ID>` is the game you want to install it for, for example, Smash Ultimate would be `01006A800016E000`)

Once installed, if no other plugins use imgui-smash, then nothing noticable will happen in-game.

## Usage (Plugin Developer)
To use imgui-smash in your plugin, do the following:
- Download `libimgui_smash.a` from the releases
- Create a new folder at the root of your plugin folder called `lib`, and put the downloaded file in there
- Create a new file called `build.rs`, and paste the following in:
```rust
fn main() {
    let proj_dir = env!("CARGO_MANIFEST_DIR");

    println!("cargo:rerun-if-changed={}/lib/", proj_dir);
    println!("cargo:rustc-link-search={}/lib/", proj_dir);
}
```
- Add the following to your `lib.rs` file:
```rust
#[link(name = "imgui_smash")]
extern "C" {}
```
- Add the following to your crate dependencies
```toml
imgui-api = { git = "https://github.com/Coolsonickirby/imgui-api" }
```
- Lastly, add the following to your `lib.rs` file:
```rust
use imgui_api::bindings::*;

unsafe extern "C" fn setup_imgui_context(imgui_ctx: *mut u64){
    igSetCurrentContext(imgui_ctx as _);
}

pub fn main() { // Make sure to just add these lines in YOUR main function instead of overwriting your main function
    unsafe {
        imgui_api::imgui_setup_context(setup_imgui_context);
    }
}
```

This will set up your plugin to allow you to create ImGUI windows. You can check out an example on the [Gist](https://gist.github.com/Coolsonickirby/89f2a895b0e06177cbbfd21523b0ac3e).

**If you're planning on making a chainloaded plugin that uses ImGUI, then you can check out this example on [Gist](https://gist.github.com/Coolsonickirby/0ff7a7bb58fe6bd9456196cc0803f9ef).**

## Compiling
(Recommend using Linux directly or WSL for compiling this plugin)

To compile, install:
- [devkitpro](https://devkitpro.org/wiki/Getting_Started)
- the switch toolchain (switch-dev)
- CMake
- [Rust & Cargo](https://rust-lang.org/tools/install/)
- cargo-skyline (`cargo install cargo-skyline`)
- cargo-skyline-std (`cargo skyline update-std`)

After all that, run the `make` command. The host plugin will be built to `./target/aarch64-skyline-switch/release`.

## Credits
- [Coolsonickirby](https://github.com/Coolsonickirby) - Modified imgui-xeno to work with Smash & Created Host Plugin
- [blujay](https://github.com/blu-dev) - Input Visualizer mod was used as reference for understanding nvn and nvn rendering implementation
- [imgui](https://github.com/ocornut/imgui) - The backbone of this whole thing
- [cimgui](https://github.com/cimgui/cimgui) - Used to create the C API for current ImGUI version
- [bindgenrs](https://github.com/rust-lang/rust-bindgen) - Used to create the bindings from the API

## Original repo credits

- [CraftyBoss](https://github.com/CraftyBoss/MP1R-Exlaunch-Base) - thanks for the base <3
- [exlaunch](https://github.com/shadowninja108/exlaunch/)
- [exlaunch-cmake](https://github.com/EngineLessCC/exlaunch-cmake/)
- [BDSP](https://github.com/Martmists-GH/BDSP)
- [Sanae](https://github.com/Sanae6)
- [Atmosphère](https://github.com/Atmosphere-NX/Atmosphere)
- [oss-rtld](https://github.com/Thog/oss-rtld)
- [roccodev](https://github.com/roccodev)
- [BlockBuilder57](https://github.com/BlockBuilder57)
- [AlexCSDev](https://github.com/AlexCSDev)