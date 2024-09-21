# Texture Swizzling

When using textures stored in RAM, the pixels must be re-arranged in a nested Z pattern.<br>
More info on this: <br>
[GBATEK 3DS](https://problemkaputt.de/gbatek-3ds-video-texture-swizzling.htm)<br>
[3DS CTF](https://reswitched-weekly-reboot.github.io/posts/3ds-ctf/)<br>

If you transfer the texture data to VRAM you can instead use the GX_TRANSFER flags to flip the image vertically and swizzle the data for you:
```c
GX_TRANSFER_FLIP_VERT(1)
GX_TRANSFER_OUT_TILED(1)
```

It's highly recommend to do this processing offline rather than at runtime, [tex3ds](https://github.com/devkitPro/tex3ds) will do this for you along with many other features.<br>

This code is not optimal but free to use (CC0).

```c
static void swizzle8x8Tile(u8* out, const u8* in, unsigned int width, unsigned int bpp)
{
	// See: https://problemkaputt.de/gbatek-3ds-video-texture-swizzling.htm 
	static const u8 swizzleOrder8x8[] =
	{
		0x00,0x01,0x08,0x09,0x02,0x03,0x0A,0x0B,
		0x10,0x11,0x18,0x19,0x12,0x13,0x1A,0x1B,
		0x04,0x05,0x0C,0x0D,0x06,0x07,0x0E,0x0F,
		0x14,0x15,0x1C,0x1D,0x16,0x17,0x1E,0x1F,
		0x20,0x21,0x28,0x29,0x22,0x23,0x2A,0x2B,
		0x30,0x31,0x38,0x39,0x32,0x33,0x3A,0x3B,
		0x24,0x25,0x2C,0x2D,0x26,0x27,0x2E,0x2F,
		0x34,0x35,0x3C,0x3D,0x36,0x37,0x3E,0x3F,
	};

	static u8 tile8x8[8 * 8 * 4];

	unsigned int y;

	// Copy 8x8 tile	
	u8* tile = tile8x8;
	for (y = 0; y < 8; ++y)
	{
		memcpy(tile, (in + (y * width * bpp)), 8 * bpp);
		tile += 8 * bpp;
	}

	// Re-arrange pixels
	const u8* order = swizzleOrder8x8;
	if (bpp == 4)
	{
		u32* out32 = (u32*)out;
		const u32* tile32 = (u32*)tile8x8;
		for (y = 0; y < 64; ++y)
		{
			*(out32++) = tile32[*(order++)];
		}
	}
	else if (bpp == 2)
	{
		u16* out16 = (u16*)out;
		const u16* tile16 = (u16*)tile8x8;
		for (y = 0; y < 64; ++y)
		{
			*(out16++) = tile16[*(order++)];
		}
	}
	else
	{
		u8* out8 = (u8*)out;
		const u8* tile8 = (u8*)tile8x8;
		for (y = 0; y < 64; ++y)
		{
			*(out8++) = tile8[*(order++)];
		}
	}
}

static void swizzle8x8(u8* out, const u8* in, unsigned int width, unsigned int height, unsigned int bpp)
{
	const unsigned int stride = width * bpp;
	const size_t tile8x8Size = (8 * 8) * bpp;

	unsigned int y, x;
	const u8* tileIn;
	for (y = 0; y < height; y += 8)
	{
		for (x = 0; x < width; x += 8)
		{
			tileIn = in + (x * bpp + y * stride);

			swizzle8x8Tile(out, tileIn, width, bpp);

			out += tile8x8Size;
		}
	}
}
```
