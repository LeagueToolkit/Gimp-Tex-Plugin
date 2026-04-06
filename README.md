# GIMP TEX Plugin

Load and export League of Legends `.tex` texture files in GIMP.

Supports **GIMP 2.x** and **GIMP 3.x**.

## Installation

Download from [Releases](https://github.com/LeagueToolkit/Gimp-Tex-Plugin/releases):

| File | Description |
|------|-------------|
| `GIMP_TEX_Plugin_Setup.exe` | Installer — auto-detects GIMP version(s) |
| `GIMP3_TEX_Plugin.zip` | Manual install for GIMP 3.x |
| `GIMP2_TEX_Plugin.zip` | Manual install for GIMP 2.x |

### Installer

Run the `.exe` and follow the prompts. Restart GIMP.

### Manual Install (GIMP 3.x)

Extract `GIMP3_TEX_Plugin.zip` into a folder called `gimp3_tex_plugin` inside your GIMP plug-ins directory:

```
%APPDATA%\GIMP\<version>\plug-ins\gimp3_tex_plugin\
```

### Manual Install (GIMP 2.x)

Extract `GIMP2_TEX_Plugin.zip` into your GIMP plug-ins directory:

```
%APPDATA%\GIMP\<version>\plug-ins\
```

This places `gimp2_tex_plugin.py` directly in `plug-ins/` and the shared files in `plug-ins/gimp2_tex_libs/`.

After manual install, restart GIMP.

---

## Uninstallation

**If installed via the installer:** Open Windows Settings > Apps > Installed Apps, find "GIMP TEX Plugin", and click Uninstall.

**If installed manually:** Delete the plugin files:
- GIMP 3.x: delete `%APPDATA%\GIMP\<version>\plug-ins\gimp3_tex_plugin\`
- GIMP 2.x: delete `gimp2_tex_plugin.py` and the `gimp2_tex_libs\` folder from `plug-ins\`

Restart GIMP.

---

## Usage

### Opening TEX files

**File > Open** and select a `.tex` file, or drag and drop.

### Exporting TEX files

**Quick export** — File > Export As, set the filename to `.tex`, hit Export. Uses your last-used settings (defaults to DXT5 with dithering).

**Export with options** — File > Export as .tex (options)... Opens a dialog where you can configure compression settings before exporting.

---

## Export Options

### Compression Format

| Format | Description |
|--------|-------------|
| **DXT1 (BC1)** | 4:1 compression. No alpha channel. Best file size for opaque textures. |
| **DXT5 (BC3)** | 4:1 compression. Full 8-bit alpha channel. Use for textures with transparency. |
| **BGRA8** | Uncompressed. 32-bit BGRA. Largest file size, no quality loss. |

DXT1 and DXT5 require image dimensions divisible by 4. If your image isn't, the plugin will tell you the nearest valid size.

### Error Diffusion Dithering

Applies [Floyd-Steinberg dithering](https://en.wikipedia.org/wiki/Floyd%E2%80%93Steinberg_dithering) during DXT compression. This reduces visible banding on gradients and smooth color transitions by spreading quantization error across neighboring pixels within each 4x4 block.

**Recommended: On.** The quality improvement is significant on most textures with minimal performance cost.

Only available for DXT1 and DXT5 formats (BGRA8 is uncompressed and doesn't need dithering).

### Error Metric

Controls how color differences are measured during DXT compression:

| Metric | Description |
|--------|-------------|
| **Perceptual** | Weights color channels by human visual sensitivity (green > red > blue). Produces results that look better to the human eye. Based on BT.709 luminance coefficients. |
| **Uniform** | Treats all color channels equally. May be preferable for non-photographic textures. |

**Recommended: Perceptual** for most textures. Use Uniform only for textures where mathematical accuracy matters more than visual quality.

Only available when dithering is enabled.

### Generate Mipmaps

Generates a full mipmap chain using Lanczos3 resampling. Each mipmap level is half the resolution of the previous one, down to 1x1.

Mipmaps are used by the game engine for level-of-detail rendering. When enabled, the file size increases by roughly 33%.

**Recommended: Off** unless you know the texture needs mipmaps. Most League of Legends textures get resized in real time.

---

## Supported Formats

| Format | Load | Export |
|--------|------|--------|
| DXT1 (BC1) | Yes | Yes |
| DXT5 (BC3) | Yes | Yes |
| BGRA8 | Yes | Yes |
| Mipmaps | Yes | Yes |

---

## Credits

- TEX format handling based on work by [LtMAO](https://github.com/tarngaina/LtMAO)
- DXT compression ported from [Microsoft DirectXTex](https://github.com/microsoft/DirectXTex) (MIT License)
- Built for the League of Legends modding community
