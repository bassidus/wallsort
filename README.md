# wallsort

A Bash script that sorts wallpaper images into a folder hierarchy based on aspect ratio and resolution.

## Output structure

```
target-dir/
├── Ultrawide/        # ratio > 2.3  (e.g. 32:9, 21:9)
│   ├── Ultra_High_Res/   # width ≥ 5120
│   ├── 4K/               # width ≥ 3840
│   ├── QHD/              # width ≥ 2560
│   ├── FHD/              # width ≥ 1920
│   ├── HD/               # width ≥ 1280
│   └── Small_Files/
├── Widescreen/       # ratio > 1.3  (e.g. 16:9, 16:10)
│   └── ...
└── Other/            # everything else (square, portrait, etc.)
    └── ...
```

## Requirements

- `bash` 4+
- [ImageMagick](https://imagemagick.org/) (`identify` must be on `$PATH`)

Install ImageMagick:

```bash
# Arch / CachyOS
sudo pacman -S imagemagick

# Debian / Ubuntu
sudo apt install imagemagick

# macOS
brew install imagemagick
```

## Usage

```bash
./wallsort [-R] <source-dir> <target-dir>
```

| Argument | Description |
|---|---|
| `-R` | Recursively scan `source-dir` for images |
| `source-dir` | Directory containing unsorted images |
| `target-dir` | Directory where sorted subfolders will be created (created if absent) |

Supported formats: `jpg`, `jpeg`, `png`, `webp`, `avif`, `jxl`, `tiff`, `bmp`

## Example

```bash
./wallsort ~/Downloads/walls ~/Pictures/wallpapers
```

To recursively scan subdirectories:

```bash
./wallsort -R ~/Downloads/walls ~/Pictures/wallpapers
```

Progress is shown inline while running:

```
[ 73%] (463/633) my_wallpaper.jpg
```

In recursive mode the relative path from the source directory is shown:

```
[ 73%] (463/633) subdir/my_wallpaper.jpg
```

Files that would overwrite an existing file at the destination are skipped and counted in the final summary.

```
Sorting complete! (633 files processed, 2 skipped due to name conflicts)
  Source: /home/user/Downloads/walls
  Output: /home/user/Pictures/wallpapers
```

## Notes

- The source directory is not modified structurally — only the matched image files are moved out of it.
- Skipped files (name conflicts) are left in the source directory untouched.
- Running the script a second time on a partially sorted source is safe.
- In recursive mode, subdirectory structure of the source is not preserved — all images are sorted flat into the target hierarchy.
