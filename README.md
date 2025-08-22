# ASCII Camera Preview

This project provides a lightweight C library for rendering grayscale camera frames
as ASCII art directly in the terminal.

## Files

- `ascii_camera_preview.h` — Public header with API declarations
- `ascii_camera_preview.c` — Implementation of ASCII rendering

## API Overview

### Initialization
```c
int ascii_camera_init(size_t width_chars, size_t height_chars);
```
Initialize the preview with the target ASCII dimensions.

### Rendering
```c
void ascii_camera_render(const uint8_t *frame,
                         size_t width_px,
                         size_t height_px,
                         size_t stride_bytes);
```
Render a grayscale frame (0–255 values) to ASCII.  
The frame is sampled down to fit the ASCII grid.

### Clearing
```c
void ascii_camera_clear(void);
```
Clear the terminal using ANSI escape codes.

### Shutdown
```c
void ascii_camera_shutdown(void);
```
Reset the preview state.

## Example Usage

```c
#include "ascii_camera_preview.h"
#include <stdint.h>
#include <stdlib.h>

int main() {
    const size_t width = 64, height = 32;
    uint8_t *frame = malloc(width * height);
    for (size_t i = 0; i < width * height; i++) {
        frame[i] = (i % width < width / 2) ? 50 : 200; // simple pattern
    }

    ascii_camera_init(64, 32);
    ascii_camera_clear();
    ascii_camera_render(frame, width, height, width);
    ascii_camera_shutdown();

    free(frame);
    return 0;
}
```

## Build

To build a simple demo:
```bash
gcc main.c ascii_camera_preview.c -o ascii_demo
./ascii_demo
```

