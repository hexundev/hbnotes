# Misc

## Textures

Converting with the devkitPro tool:
```shell
tex3ds --help
tex3ds -f rgba8 -r -z none image.png
```

## ETC1
The 3DS supports ETC1 compressed textures.<br>
tex3ds is recommended as it will output textures in the correct format.

Other tools:<br>
- [PowerVR pvrtextool](https://developer.imaginationtech.com/solutions/pvrtextool/)<br>
- [wolfpld's etcpak](https://github.com/wolfpld/etcpak/releases)<br>


Unlike OpenGL there are a few requirements to the output:<br>
- Flip the image vertically before conversion
- After conversion to ETC1, the byte order needs to be reversed
- The image must be re-arranged in a tile format instead of linear:

```
For every tile (4x4 pixels/8 bytes)

Linear:
[0][1][2][3]    [4][5][6][7]
[8][9][10][11]  [12][13][14][15]
......

3DS:
[0][1][4][5]  [8][9][12][13]
[2][3][6][7]  [10][11][14][15]
......
```

## Building a .cia

### Required files:
- .elf
- .smdh
- banner file
- .rsf

ELF and SMDH are already created as part of the .3dsx creation process.

### Step 1: Banner

Download [Bannertool 3DS](https://www.gamebrew.org/wiki/Bannertool_3DS)<br>

Option 1: Create an image banner:<br>
```shell
Example:
bannertool makebanner -i banner.png -a audio.wav -o banner.bnr
```

Option 2: Create a banner with a 3D model:<br>
```shell
Example:
bannertool makebanner -ci model.cgfx -a audio.wav -o banner.bnr
```
Tutorial for 3D models, requires a few programs:<br>
[Creating 3D banners from 3D Models for CIA's](https://gbatemp.net/threads/creating-3d-banners-from-3d-models-for-cias.433783/)

### Step 2: RSF
Create an RSF file with information about the app:<br>
[RSF Example](https://gist.github.com/jakcron/9f9f02ffd94d98a72632)

### Final Step
Use makerom
```shell
Example:
makerom -f cia -target t -exefslogo -o target.cia -elf target.elf -rsf app.rsf -ver "1000" -banner banner.bnr -icon target.smdh
```

If everything went well, you should have a .cia file you can install on the 3DS with FBI.
