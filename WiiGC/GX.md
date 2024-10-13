# Wii/GameCube GX

## Textures

devkitPro already comes with gxtexconv to handle converting images to the GX's native formats.<br>
https://github.com/devkitPro/gamecube-tools<br>

Texture data must be tiled, aligned to 32 bytes and be in big endian format.

### 16-bit
Texture tiling is a similar process to the 3DS for 16-bit textures:<br>
- Re-arrange the pixels from horizontal lines to 4x4 tiles.<br>
- Reverse the byte order for each pixel as well since the PPC is big endian<br>

### 32-bit
For 32-bit textures it's more complicated, the AR and GB components must be 32 bytes apart from an offset.<br>
To calculate the offset see GRRLIB's source as example:<br>
https://github.com/GRRLIB/GRRLIB/blob/master/GRRLIB/GRRLIB/grrlib/GRRLIB_pixel.h<br>
```c
void  GRRLIB_SetPixelTotexImg (const int x, const int y,
                               GRRLIB_texImg *tex, const u32 color) {
    u32  offs;
    u8*  bp = (u8*)tex->data;

    offs = (((y&(~3))<<2)*tex->w) + ((x&(~3))<<4) + ((((y&3)<<2) + (x&3)) <<1);

    *((u16*)(bp+offs   )) = (u16)((color <<8) | (color >>24));
    *((u16*)(bp+offs+32)) = (u16) (color >>8);
}
```
A loop can be used to copy each RGBA pixel from your image to the texture with that function.
