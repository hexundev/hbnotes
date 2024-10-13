# Textures

The PSP supports many pixel formats for textures: <br>
- 32-bit
- 16-bit
- 8-bit with palettes
- 4-bit with palettes
- Compressed with DXT1, DXT3, DXT5

## Tiling/swizzling

The PSP supports linear and tiled textures, however there is a massive performance penalty for un-tiled textures so it should not be used.
Tiles are up to 128 bytes in size, and Nx8 bytes per tile depending on the bit depth.
Ex: for 16-bit textures a tile is (16/2)x8, for 32-bit (16/4)x8 and so on
