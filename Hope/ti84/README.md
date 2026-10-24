# /ti84/ — TI-84 Plus CE emulator page

Open at `/ti84/` on Vercel (or via the "TI-84 Plus CE" card in the library). This folder holds only the loader and layout.
It needs two things you supply:

## 1. The emulator build (open source, MIT)
Uses the WebAssembly build of https://github.com/hunterchen7/ti84ce (exposes a `WasmEmu` class). Build it once:

    git clone https://github.com/hunterchen7/ti84ce && cd ti84ce
    rustup target add wasm32-unknown-unknown && cargo install wasm-pack
    wasm-pack build core --target web --release --out-dir ../web/src/emu-core

Copy `emu_core.js` and `emu_core_bg.wasm` from `web/src/emu-core/` into `ti84/emu/` and keep the project's LICENSE next to them.
A different build works if it exports `default` (init) and `WasmEmu` with the same methods (`load_rom`, `send_file`,
`send_file_live`, `power_on`, `run_cycles`, `get_framebuffer_rgba`, `framebuffer_width/height`, `set_key`, `is_lcd_on`).
Change the path in `config.json` if you name it differently (same-origin paths only).

## 2. Your own TI-84 CE ROM
Drop it on the page. It is stored only in your browser (IndexedDB) and never uploaded. The `rom` path in `config.json`
is optional; do not put a ROM in a public deployment, because that publishes it to everyone.

## Programs and key mapping
Drop `.8xp` / `.8xv` files on the page (libraries such as graphx/libload `.8xv` files must be added too).
The key matrix defaults to the CE toolchain's keypad layout (row = key group, col = bit). If a key does nothing on your build,
edit `matrix` in the "Key mapping" box (or drop a `.json` file); the "Last key" readout shows what each press sends.
The ON key is not part of the normal matrix, so its default `[0,0]` may need changing.
