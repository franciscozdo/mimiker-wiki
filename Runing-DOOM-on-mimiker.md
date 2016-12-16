This guide will explain what you need to know in order to test DOOM running on mimiker.

I'll assume you have a working toolchain and OVPsim ready (VGA support for QEMU is not implemented).

![screenshot](http://i.imgur.com/rig3Nt3.png)

* Get [this branch](https://github.com/rafalcieslak/mimiker/tree/doom_test3). It incorporates several experimental features not available in `master` (VGA driver stub, file descriptor support, embedfs, and various fixes).

```
git clone -b doom_test3 https://github.com/rafalcieslak/mimiker
```
* Within `user` directory, place [ported DOOM sources](https://github.com/rafalcieslak/headless_doom). These sources vary slightly from the original source - in particular, they write to vga memory using `/dev/vga/*`, and the path to doom resource files is corrected to match our current filesystem layout.

```
cd user
git clone https://github.com/rafalcieslak/headless_doom
```

* Place your `doom.wad` inside `./user/headless_doom/`. This has to be the `doom.wad` from *The Ultimate Doom 1.10*, no other IWADs are supported. The `Makefile` for headless_doom will check the md5 sum of the wad file. Note that the file name is case-sensitive.

```
cp /path/to/your/doom/doom.wad headless_doom/.
cd ../
```

* Compile mimiker as you normally would. This will recursively compile DOOM sources, and will embed DOOM resource files into kernel image.

```
make
```

* Optionally, if you want to read DOOM startup messages, run the following command in a separate terminal. It will filter out most kernel messages. If you do, use the `-t` flag when starting simulator in the next step.

```
while true; do nc localhost 8000 | grep -v "\["; done
```

* Start the simulator using `exec_doom` image, which will load DOOM as userspace program.

```
./launch tests/exec_doom.elf
```

* Once DOOM loads its resources, DoomDoneQuick demo will begin playing.